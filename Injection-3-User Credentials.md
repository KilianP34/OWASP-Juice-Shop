# **Rapport de vulnérabilité — User Credentials (Injection SQL)**

## **1. Méthodologie**

1. Identification du point d'injection : la fonctionnalité de recherche de produits via le paramètre `q` à l'URL `https://ctf.juice.cyber.epitest.eu/rest/products/search?q=payload`.
2. Test d'injection SQL basique avec `' OR 1=1 /**/` pour confirmer la vulnérabilité.
3. Test d'injection UNION SELECT pour déterminer le nombre de colonnes attendues dans le résultat.
4. Analyse de la structure de la table USERS via des tentatives successives.
5. Payload final utilisé : `test')) UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, email, profileImage FROM USERS--`
6. Extraction réussie de toutes les informations d'identification des utilisateurs (usernames, mots de passe hashés, emails, rôles).

### **Techniques utilisées**

* Injection SQL UNION
* Reconnaissance du schéma de base de données
* Extraction de données sensibles via UNION SELECT
* Commentaire SQL pour neutraliser la fin de la requête

### **Outils utilisés**

* Navigateur web (barre d'URL)
* Outils de développement du navigateur (Network tab)

---

## **2. Vulnérabilité**

* **Type :** Injection SQL
* **Composant affecté :** API de recherche de produits (`/rest/products/search`)
* **Sévérité :** **Critique** (extraction complète des données d'identification)

**Payload décomposé :**
- `test'))` : Fermeture de la requête originale
- `UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, email, profileImage` : Sélection de 9 colonnes correspondant à la structure de la table USERS
- `FROM USERS` : Extraction depuis la table des utilisateurs
- `--` : Commentaire SQL pour ignorer le reste de la requête

---

## **3. Risques**

* Exposition complète des informations d'identification de tous les utilisateurs
* Accès aux mots de passe hashés (risque de cracking offline)
* Identification des rôles utilisateurs (admin, utilisateurs standards)
* Vol d'emails pouvant mener à du phishing ciblé
* Compromission totale de la sécurité de l'application
* Violation grave des réglementations sur la protection des données (RGPD)
* Atteinte majeure à la réputation du service

---

## **4. Actions**

* Utiliser des **requêtes préparées** (prepared statements) avec des paramètres liés
* Valider et échapper toutes les entrées utilisateur avant traitement SQL
* Implémenter une validation stricte des entrées (whitelist de caractères autorisés)
* Utiliser un ORM avec protection intégrée contre les injections SQL
* Limiter les privilèges du compte de base de données utilisé par l'application
* Ne jamais exposer les mots de passe, même hashés, via des API
* Mettre en place une détection d'anomalies pour les requêtes suspectes
* Implémenter un système de rate limiting sur les endpoints sensibles

---
