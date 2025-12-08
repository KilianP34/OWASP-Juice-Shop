# **Rapport de vulnérabilité — Allowlist Bypass (Unvalidated Redirect)**

## **1. Méthodologie**

1. Recherche de mots-clés dans le fichier **main.js** (`"redirect"`).
2. Découverte de l’URL autorisée : `http://shop.spreadshirt.com/juiceshop`.
3. Test de la redirection avec un paramètre `to=` manipulé.
4. Bypass de l’allowlist en insérant l’URL autorisée dans un paramètre secondaire :

   * `https://ctf.juice.cyber.epitest.eu/redirect?to=http://google.com/?test=http://shop.spreadshirt.com/juiceshop`
5. Redirection acceptée.

### **Techniques utilisées**

* Inspection du code (JS côté client)
* Manipulation de paramètres d’URL

### **Outils utilisés**

* Navigateur web
* Inspection du code source

---

## **2. Vulnérabilité**

* **Type :** Unvalidated Redirect — **Allowlist Bypass**
* **Composant affecté :** Endpoint `/redirect?to=`
* **Sévérité :** **Élevée**

---

## **3. Risques**

* Redirection d’utilisateurs vers des sites malveillants
* Phishing facilité par des liens légitimes compromettants
* Perte de confiance dans le domaine officiel

---

## **4. Actions**

* Vérifier les redirections **côté serveur**, pas en JavaScript
* Interdire totalement les URLs externes ou utiliser une **allowlist stricte**
