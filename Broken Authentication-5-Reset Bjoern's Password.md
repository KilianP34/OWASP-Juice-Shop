# **Rapport de vulnérabilité — Reset Bjoern’s Password (Broken Authentication)**

## **1. Méthodologie**

1. Accès à la fonctionnalité **Forgot Password** pour le compte interne de Bjoern.
2. Recherche OSINT sur son identité : recherche de son **nom sur Internet**.
3. Consultation de son **profil Facebook**, identification de **la ville de son école primaire**.
4. Recherche du **code postal historique** de cette ville (le nouveau ne fonctionnait pas).
5. Soumission de l’email + ancienne réponse correcte → réinitialisation du mot de passe.

### **Techniques utilisées**

* OSINT (réseaux sociaux, informations publiques)
* Exploitation de question secrète faible

### **Outils utilisés**

* Navigateur web
* Facebook (OSINT)
* Google maps

---

## **2. Vulnérabilité**

* **Type :** Broken Authentication — Weak Security Question
* **Composant affecté :** Mécanisme “Forgot Password”
* **Sévérité :** **Critique**

---

## **3. Risques**

* Compromission complète du compte de Bjoern
* Possibilité de récupération de données internes

---

## **4. Actions**

* Supprimer les **questions secrètes sensibles aux OSINT**
* Utiliser un système de reset par **token sécurisé envoyé par email**
* Ajouter des protections contre l’abus et la prédiction de réponses