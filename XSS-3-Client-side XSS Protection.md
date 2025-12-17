# **Rapport de vulnérabilité — Client-side XSS Protection (XSS)**

## **1. Méthodologie**

1. Accès au formulaire d'inscription utilisateur.
2. Interception de la requête **POST** du registre via les outils de développement.
3. Modification du champ **email** dans le body de la requête pour contourner la validation côté client.
4. Injection d'une payload XSS avec échappement des guillemets : `<iframe src="javascript:alert(`xss`)">`.
5. Envoi de la requête modifiée → le XSS s'exécute → challenge validé.

### **Techniques utilisées**

* Interception et manipulation de requêtes HTTP
* Bypass de validation côté client
* Injection XSS dans un champ d'entrée

### **Outils utilisés**

* Navigateur web (DevTools / Network)

---

## **2. Vulnérabilité**

* **Type :** Client-side XSS Protection Bypass
* **Composant affecté :** Formulaire d'inscription / endpoint POST register
* **Sévérité :** **Élevée** (exécution de JavaScript arbitraire)

---

## **3. Risques**

* Exécution de code malveillant dans le navigateur des utilisateurs
* Vol de sessions ou de tokens d'authentification
* Compromission de comptes utilisateurs
* Atteinte à l'intégrité de l'application web

---

## **4. Actions**

* Valider et échapper toutes les entrées utilisateur **côté serveur**
* Ne jamais se fier uniquement aux validations côté client
* Implémenter des filtres anti-XSS robustes sur tous les champs d'entrée
