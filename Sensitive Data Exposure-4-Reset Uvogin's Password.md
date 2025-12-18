# **Rapport de vulnerabilite - Reset Uvogin's Password (Sensitive Data Exposure)**

## **1. Methodologie**

1. Identification de l'utilisateur cible : Uvogin (email : `uvogin@juice-sh.op`).
2. Analyse des avis produits sur Juice Shop pour identifier le style d'ecriture de Uvogin : utilisation systematique du "Leet Speak" (substitution de lettres par des chiffres/symboles).
3. Recherche OSINT (Open Source Intelligence) : recherche du nom d'utilisateur sur les reseaux sociaux (Twitter).
4. Decouverte du profil Twitter de Uvogin (@Uv0gin) avec des tweets publics revelant ses preferences personnelles.
5. Identification de la reponse a la question de securite via un tweet : son film prefere est **"Silence of the Lambs"**.
6. Utilisation de la fonctionnalite "Mot de passe oublie" sur Juice Shop.
7. Saisie de l'email `uvogin@juice-sh.op` et reponse a la question de securite : "What is your favorite movie?" avec "Silence of the Lambs".
8. Definition d'un nouveau mot de passe et connexion reussie au compte.

### **Techniques utilisees**

* OSINT (Open Source Intelligence) via reseaux sociaux
* Analyse comportementale (identification du Leet Speak)
* Exploitation de questions de securite faibles
* Reconnaissance d'informations publiquement disponibles

### **Outils utilises**

* Navigateur web
* Twitter / reseaux sociaux
* Fonctionnalite de reinitialisation de mot de passe de Juice Shop

---

## **2. Vulnerabilite**

* **Type :** Sensitive Data Exposure / Weak Security Questions
* **Composant affecte :** Mecanisme de reinitialisation de mot de passe (`/rest/user/reset-password`)
* **Severite :** **Elevee** (prise de controle de compte utilisateur)

**Details techniques :**

Le systeme de reinitialisation repose sur une question de securite dont la reponse peut etre decouverte via des recherches publiques. Faiblesses identifiees :

1. **Questions de securite previsibles** : "Quel est votre film prefere?" est souvent partage publiquement
2. **Absence de rate-limiting** : Aucune limitation du nombre de tentatives
3. **Pas de verification secondaire** : Aucun email de confirmation requis
4. **Correlation identite en ligne** : Meme pseudonyme sur l'application et les reseaux sociaux

**Processus d'exploitation :**
- Identification de l'email cible
- Recherche OSINT sur Twitter (@Uv0gin)
- Extraction de la reponse : "Silence of the Lambs"
- Reinitialisation du mot de passe
- Acces complet au compte

---

## **3. Risques**

* Prise de controle complete du compte utilisateur
* Acces aux donnees personnelles (adresse, historique de commandes)
* Possibilite d'effectuer des achats frauduleux
* Vol d'informations sensibles (cartes bancaires enregistrees)
* Usurpation d'identite

---

## **4. Actions**

* Remplacer les questions de securite par un **lien de reinitialisation envoye par email**
* Implementer une **authentification multi-facteurs (2FA)** obligatoire
* Implementer un **rate-limiting strict** (max 3-5 tentatives par heure)
* Logger toutes les tentatives et alerter l'utilisateur par email
* Invalider les sessions actives lors d'une reinitialisation

---
