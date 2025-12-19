# **Rapport de vulnerabilite - GDPR Data Erasure (Broken Authentication)**

## **1. Methodologie**

1. Extraction des informations utilisateurs via injection SQL sur l'endpoint de recherche :
   - Payload : `test')) UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, email, profileImage FROM USERS--`
   - Identification de l'email de Chris : `chris.pike@juice-sh.op`
   - Observation : le champ `deletedAt` de Chris est non-NULL
2. Tentative de connexion normale sur le compte Chris : echec (compte desactive).
3. Test d'injection SQL sur le formulaire de connexion :
   - Saisie d'un apostrophe `'` dans le champ email pour provoquer une erreur SQL
   - Analyse de l'erreur pour comprendre la structure de la requete
4. Construction du payload d'injection pour contourner l'authentification :
   - Payload : `' OR deletedAt IS NOT NULL --`
   - Ce payload modifie la requete pour retourner le premier compte ayant un `deletedAt` non-NULL (Chris)
5. Connexion reussie au compte de Chris sans mot de passe.
6. Une fois connecte, acces a la fonctionnalite "Data Privacy" pour demander l'effacement RGPD.
7. Validation du challenge apres avoir demande l'effacement des donnees.

### **Techniques utilisees**

* Injection SQL UNION (extraction des utilisateurs)
* Injection SQL sur le formulaire de login (bypass d'authentification)
* Exploitation de la logique metier RGPD
* Manipulation du champ `deletedAt` pour cibler des comptes marques comme supprimes

### **Outils utilises**

* Navigateur web (barre d'URL pour injection, formulaire de login)
* Outils de developpement du navigateur (Network tab)

---

## **2. Vulnerabilite**

* **Type :** Broken Authentication + Injection SQL
* **Composant affecte :** API de connexion (`/rest/user/login`) et systeme RGPD
* **Severite :** **Critique** (acces a des comptes marques comme supprimes)

---

## **3. Risques**

* Acces non autorise a des comptes marques pour suppression RGPD
* Contournement complet du mecanisme d'authentification
* Violation des reglementations RGPD (acces a des donnees censees etre supprimees)
* Possibilite de restaurer ou manipuler des comptes desactives
* Exposition des donnees personnelles de comptes "effaces"

---

## **4. Actions**

* Implementer une validation stricte du format d'email 
* **Bloquer definitivement l'acces aux comptes avec `deletedAt` non-NULL**
* Implementer une suppression definitive (hard delete) apres la periode de retention RGPD
* Ajouter une verification supplementaire pour empecher l'acces aux comptes marques pour suppression
* Implementer un systeme de rate limiting sur l'endpoint de login
* Logger toutes les tentatives de connexion sur des comptes desactives/supprimes

---
