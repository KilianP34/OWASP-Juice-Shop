# **Rapport de vulnérabilité — Product Tampering (Broken Access Control)**

## **1. Méthodologie**

1. Inspection de l’endpoint **`/api/products/:id`** pour voir s’il acceptait des modifications.
2. Passage manuel de la requête en **PUT** pour modifier le produit.
3. Récupération du body JSON, puis suppression de tous les champs inutiles sauf **`description`**.
4. Modification du lien présent dans la description → remplacement du `href` par
   **`https://owasp.slack.com`**.
5. Envoi de la requête PUT modifiée → description du produit changée avec succès → challenge validé.

### **Techniques utilisées**

* Manipulation d’API REST
* Modification non autorisée de champs protégés
* Bypass des contrôles d’accès côté serveur

### **Outils utilisés**

* Navigateur (DevTools / Network)

---

## **2. Vulnérabilité**

* **Type :** Broken Access Control — Unauthorized Product Modification
* **Composant affecté :** API `/products/:id`
* **Sévérité :** **Élevée** (données produit compromise)

---

## **3. Risques**

* Altération de descriptions produits (désinformation, phishing)
* Manipulation de liens ou contenus affichés à tous les utilisateurs
* Atteinte à l’image de marque ou redirection vers des sites externes non désirés

---

## **4. Actions**

* Implémenter un contrôle strict d’autorisation sur les opérations **PUT**
* Restreindre les champs modifiables côté serveur