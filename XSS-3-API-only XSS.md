Voici ton **rapport court**, même format que les précédents, adapté à l’**API-only XSS** :

---

# **Rapport de vulnérabilité — API-Only XSS**

## **1. Méthodologie (bref)**

1. Connexion avec un **compte administrateur** pour récupérer un **Bearer token** valide.
2. Inspection des endpoints disponibles et découverte de `api/products:id`.
3. Envoi d'une requête **PUT** pour obtenir la structure du payload.
4. Ajout de l’header `Content-Type: application/json`.
5. Injection d’une payload XSS dans le champ **description** :

   * `"<iframe src=\"javascript:alert(`xss`)\"></iframe>"`
6. Mise à jour réussie: le XSS s’exécute lors de l'affichage du produit.

### **Techniques utilisées**

* Enumeration d’API
* Manipulation d’API REST (PUT)
* Injection XSS dans un champ JSON

### **Outils utilisés**

* Navigateur web (Network)
* Bearer token administrateur

---

## **2. Vulnérabilité**

* **Type :** API-Only XSS
* **Composant affecté :** Endpoint `PUT /api/products`
* **Sévérité :** **Critique** (exécution de JS)

---

## **3. Risques**

* Exécution de code dans le navigateur d’un admin
* Vol de tokens / compromission de compte administrateur
* Défaçage ou prise de contrôle de l’interface web

---

## **4. Actions**

* Échapper et valider toutes les entrées JSON côté backend
* Bloquer les tags HTML dangereux (`iframe`, `<script>`, ...)
