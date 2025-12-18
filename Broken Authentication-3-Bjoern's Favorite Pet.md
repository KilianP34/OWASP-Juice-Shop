# **Rapport de vulnérabilité — Bjoern's Favorite Pet (Broken Authentication)**

## **1. Méthodologie**

1. Identification du nom complet de Bjoern : **Bjoern Kimminich**.
2. Recherche **OSINT** sur Google avec le nom complet.
3. Découverte d'une **conférence de Bjoern** sur la chaîne YouTube **OWASP Foundation**.
4. Visualisation de la vidéo où Bjoern révèle publiquement la réponse à sa question de sécurité : **"Zaya"** (nom de son animal de compagnie).
5. Utilisation de la fonctionnalité **"Forgot Password"** ou question de sécurité pour se connecter au compte de Bjoern.
6. Soumission de la réponse **"Zaya"** → connexion réussie au compte → challenge validé.

### **Techniques utilisées**

* OSINT (Open Source Intelligence)
* Exploitation de question de sécurité faible
* Recherche d'informations publiques sur Internet

### **Outils utilisés**

* Google
* YouTube (OWASP Foundation)
* Navigateur web

---

## **2. Vulnérabilité**

* **Type :** Broken Authentication — Weak Security Question
* **Composant affecté :** Mécanisme de récupération de compte / Question de sécurité
* **Sévérité :** **Critique**

---

## **3. Risques**

* Compromission complète du compte de Bjoern Kimminich
* Accès non autorisé à des données sensibles ou privilèges administrateurs
* Possibilité de récupération de comptes via des informations publiquement accessibles
* Exploitation massive si d'autres utilisateurs ont des réponses facilement trouvables

---

## **4. Actions**

* Supprimer les **questions secrètes prédictibles** ou basées sur des informations publiques
* Utiliser un système de récupération par **token sécurisé envoyé par email**
* Implémenter l'**authentification multi-facteurs (MFA/2FA)**
* Éviter les questions dont les réponses peuvent être trouvées via OSINT
* Sensibiliser les utilisateurs sur les risques de partage d'informations personnelles en ligne
