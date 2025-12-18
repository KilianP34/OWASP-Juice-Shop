# **Rapport de vulnérabilité — HTTP-Header XSS (XSS)**

## **1. Méthodologie**

1. Énumération de toutes les routes API disponibles sous **`/rest`**.
2. Découverte de l'endpoint **`/rest/saveLoginIp`**.
3. Récupération d'une requête avec un **token d'authentification** valide.
4. Ajout d'un header personnalisé **`True-Client-IP`** dans la requête.
5. Injection de la payload XSS dans la valeur du header `True-Client-IP` : **`<iframe src="javascript:alert(`xss`)">`**.
6. Envoi de la requête vers l'endpoint `/rest/saveLoginIp` → le payload s'exécute → challenge validé.

![alt text](img/httpheader.png)

### **Techniques utilisées**

* Énumération d'API REST
* Injection XSS via header HTTP personnalisé
* Exploitation d'un endpoint de logging/tracking

### **Outils utilisés**

* Navigateur web (DevTools / Network)

---

## **2. Vulnérabilité**

* **Type :** HTTP-Header XSS
* **Composant affecté :** Endpoint `/rest/saveLoginIp` / Header `True-Client-IP`
* **Sévérité :** **Élevée** (exécution de JavaScript via header HTTP)

---

## **3. Risques**

* Exécution de code malveillant dans le navigateur des utilisateurs
* Vol de sessions ou de tokens d'authentification
* Compromission de comptes utilisateurs
* Atteinte à l'intégrité de l'application

---

## **4. Actions**

* Valider et échapper tous les headers HTTP avant de les stocker ou afficher
* Ne jamais faire confiance aux headers fournis par le client (ex: `True-Client-IP`)
* Encoder correctement les données provenant des headers avant insertion dans le HTML