# **Rapport de vulnérabilité — Reset Bender's Password (Broken Authentication)**

## **1. Méthodologie**

1. Identification du compte cible : **Bender** (personnage de Futurama).
2. Recherche **OSINT** sur le **Wiki de Bender** (Futurama Wiki).
3. Découverte d'informations clés dans sa biographie :
   * Citation : *"As a bending unit, he spent his life before he met Fry bending girders to be used for Suicide booths."*
4. Recherche approfondie sur l'entreprise fabricant les **Suicide Booths** dans l'univers Futurama.
5. Identification de l'entreprise : **Stop'n'Drop**.
6. Utilisation de la fonctionnalité **"Forgot Password"** pour le compte de Bender.
7. Soumission de la réponse à la question de sécurité (nom de l'entreprise) : **"Stop'n'Drop"** → réinitialisation du mot de passe réussie → challenge validé.

### **Techniques utilisées**

* OSINT (Open Source Intelligence)
* Recherche d'informations sur des wikis de culture populaire
* Exploitation de question de sécurité basée sur des références fictionnelles

### **Outils utilisés**

* Navigateur web
* Futurama Wiki

---

## **2. Vulnérabilité**

* **Type :** Broken Authentication — Weak Security Question
* **Composant affecté :** Mécanisme "Forgot Password" / Question de sécurité
* **Sévérité :** **Critique**

---

## **3. Risques**

* Compromission complète du compte de Bender
* Accès non autorisé aux données personnelles et privilèges associés
* Exploitation similaire sur d'autres comptes avec réponses basées sur des références publiques

---

## **4. Actions**

* Supprimer les **questions secrètes prédictibles** basées sur des références culturelles ou fictionnelles
* Éviter les questions dont les réponses peuvent être trouvées via OSINT (wikis, encyclopédies)
* Utiliser un système de récupération par **token sécurisé envoyé par email**
* Implémenter l'**authentification multi-facteurs (MFA/2FA)**
* Sensibiliser les utilisateurs sur les risques de choisir des réponses basées sur des informations publiquement accessibles
