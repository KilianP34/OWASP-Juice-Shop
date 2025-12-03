# **Rapport de vulnérabilité — Broken Anti-Automation / CAPTCHA Bypass**

## **1. Méthodologie**

1. Analyse du formulaire de feedback et observation du mécanisme CAPTCHA.
2. Capture de la requête POST via l’outil navigateur (**Network**).
3. Renvoi manuel automatisé : répétition de la même requête **10 fois en <20 secondes**.
4. Soumission multiple acceptée malgré le CAPTCHA.

### **Techniques utilisées**

* Rejeu de requêtes (request replay)

### **Outils utilisés**

* Navigateur web (onglet Network)

---

## **2. Vulnérabilité**

* **Type :** Broken Anti-Automation / CAPTCHA bypass
* **Composant affecté :** Formulaire d’avis clients + système CAPTCHA
* **Sévérité :** **Élevée**

---

## **3. Risques**

* Spam massif de feedback
* Pollution de données (fausses statistiques)
* Impact réputationnel (avis frauduleux, manipulation d’opinions)

---

## **4. Actions**

* Lier chaque requête au CAPTCHA validé via un **token serveur** unique
* Mettre en place un **rate-limiting strict** (ex : 3 requêtes / minute)