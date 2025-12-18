# **Rapport de vulnérabilité — CSP Bypass (XSS)**

## **1. Méthodologie**

1. Identification d'une injection possible dans la **Content Security Policy (CSP)**.
2. Modification d'une valeur prévue pour une image afin d'y ajouter une nouvelle directive **`script-src 'unsafe-inline'`**, ce qui désactive la protection contre les scripts inline.
3. Contournement du filtre regex en utilisant la technique **`a|a`** pour faire passer une balise `<script>` sans être détecté par la validation.
4. Injection de la payload finale : **`<<a|ascript>alert(`xss`)</script>`**.
5. Grâce à la CSP affaiblie, le payload `<script>alert('xss')</script>` s'exécute → challenge validé.

![alt text](img/image.png)

### **Techniques utilisées**

* Injection dans la Content Security Policy
* Bypass de validation regex (`a|a`)
* Exploitation de `script-src 'unsafe-inline'`
* Injection XSS avec obfuscation de balises

### **Outils utilisés**

* Navigateur web (DevTools)

---

## **2. Vulnérabilité**

* **Type :** CSP Bypass + XSS
* **Composant affecté :** Content Security Policy / Filtrage d'entrée utilisateur
* **Sévérité :** **Critique** (bypass complet de la CSP et exécution de JavaScript)

---

## **3. Risques**

* Contournement des mécanismes de sécurité CSP
* Exécution de code malveillant dans le navigateur des utilisateurs
* Vol de sessions, tokens d'authentification ou données sensibles
* Compromission complète de l'application côté client
* Atteinte à la confiance des utilisateurs et à l'intégrité du système

---

## **4. Actions**

* Ne jamais permettre l'injection de directives CSP via des entrées utilisateur
* Définir une CSP stricte et statique côté serveur, non modifiable dynamiquement
* Éviter l'utilisation de `'unsafe-inline'` dans `script-src`
* Améliorer les filtres de validation pour détecter les techniques d'obfuscation (regex bypass)
* Encoder et échapper toutes les entrées utilisateur avant insertion dans le HTML