# **Rapport de vulnérabilité — Forged Signed JWT**

## **1. Méthodologie**

1. Connexion avec un compte quelconque pour récupérer un **JWT valide**.
2. Ouverture du token dans **jwt.io** et modification :

   * Changement de l’algorithme → de **RS256** à **HS256**
   * Remplacement de l’email → **[rsa_lord@juice-sh.op]**
3. Récupération de la **clé secrète du serveur** via l’endpoint public
   `GET /encryptionkeys/jwt.pub`.
4. Ajout direct de cette clé dans **jwt.io**, qui génère automatiquement
   une signature valide pour le header + payload modifiés.
5. Remplacement du Bearer token dans la requête par le **JWT forgé** → challenge validé.

### **Techniques utilisées**

* Analyse et manipulation de JWT
* Changement de l’algorithme (RS256 → HS256)
* Exploitation d’une clé publique exposée
* Forgery de tokens d’authentification

### **Outils utilisés**

* jwt.io
* Navigateur web (DevTools / Network)

---

## **2. Vulnérabilité**

* **Type :** JWT Forgery / Algorithm Confusion
* **Composant affecté :** Système d’authentification JWT
* **Sévérité :** **Critique** (forgery totale du token = compromission complète)

---

## **3. Risques**

* Usurpation de n’importe quel utilisateur, y compris administrateurs
* Accès illimité à des endpoints protégés
* Possibilité d’escalade de privilèges et d’actions destructrices

---

## **4. Actions**

* Interdire le changement d’algorithme côté serveur
* Ne **jamais** accepter HS256 si la clé est publique
* Éviter d’exposer les clés de signature dans des endpoints accessibles