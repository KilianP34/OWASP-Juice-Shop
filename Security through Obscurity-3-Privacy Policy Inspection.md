# **Rapport de vulnérabilité — Privacy Policy Inspection (Security through Obscurity)**

## **1. Méthodologie**

1. Accès à la page **Privacy Policy** et observation de mots surlignés au survol.
2. Ouverture du **DevTool** et recherche (`Ctrl+F`) des éléments avec la classe **`.hot`**.
3. Extraction de tous les mots marqués `.hot` directement à partir du HTML.
4. Construction manuelle de l’URL en **concaténant les mots** avec `/` et en remplaçant les espaces par `/`.
5. Saisie de cette URL (`https://ctf.juice.cyber.epitest.eu/We/may/also/instruct/you/to/refuse/all/reasonably/necessary/responsibility`) → challenge validé.

### **Techniques utilisées**

* Lecture et inspection du HTML
* Analyse de classes CSS “cachées”
* Reconstruction d’une URL interne obfusquée

### **Outils utilisés**

* Navigateur web + DevTool

---

## **2. Vulnérabilité**

* **Type :** Security through Obscurity
* **Composant affecté :** Page Privacy Policy (HTML / CSS)
* **Sévérité :** **Très faible** (aucune faille de sécurité réelle)

---

## **3. Risques**

* *Aucun risque de sécurité réel.*

---

## **4. Actions**

---