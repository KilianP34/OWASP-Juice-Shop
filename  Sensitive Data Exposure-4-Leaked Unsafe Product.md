# **Rapport de vulnerabilite - Leaked Unsafe Product (Sensitive Data Exposure)**

## **1. Methodologie**

1. Consultation de la liste des produits sur Juice Shop.
2. Identification du produit **Rippertuer Special Juice** (ID: 11) dans les produits supprimes via injection SQL :
   - Endpoint : `https://ctf.juice.cyber.epitest.eu/rest/products/search?q=payload`
   - Payload : `test')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--`
   - Decouverte du produit marque comme supprime (soft delete)
3. Analyse de la description du produit : contient une URL externe `https://listverse.com/2011/07/08/top-20-fruits-you-probably-dont-know/`.
4. Visite de l'URL Listverse pour rechercher des informations sur les ingredients dangereux.
5. Recherche dans les commentaires de l'article avec CTRL+F pour trouver un lien Pastebin.
6. Decouverte du lien Pastebin dans les commentaires : `https://pastebin.com/90dUgd7s`.
7. Consultation du Pastebin revelant la composition complete du produit avec les ingredients dangereux :
   - **Hueteroneel** (fruit du Manchineel / Mancenillier)
   - Ingredient extremement toxique et dangereux pour la consommation
8. Utilisation de la fonctionnalite de plainte (Complaint) sur Juice Shop pour signaler le produit dangereux avec les ingredients toxiques.
9. Validation du challenge apres avoir signale les ingredients dangereux.

### **Techniques utilisees**

* Injection SQL pour extraire les produits supprimes (soft delete)
* OSINT (Open Source Intelligence) via liens externes
* Analyse de commentaires et sources externes (Listverse, Pastebin)
* Exploitation de fuites d'informations sensibles

### **Outils utilises**

* Navigateur web (injection SQL via URL, recherche CTRL+F)
* Pastebin (consultation de documents exposes)
* Fonctionnalite de plainte de Juice Shop

---

## **2. Vulnerabilite**

* **Type :** Sensitive Data Exposure / Information Disclosure
* **Composant affecte :** Description des produits + liens externes non sanitises
* **Severite :** **Elevee** (exposition d'informations sensibles sur des produits dangereux)

---

## **3. Risques**

* **Risque sanitaire** : Vente de produits dangereux pour la sante des consommateurs
* **Responsabilite legale** : Poursuites judiciaires en cas d'intoxication ou deces
* **Exposition d'informations confidentielles** : Composition des produits accessible publiquement via Pastebin
* **Atteinte a la reputation** : Decouverte de produits dangereux mine la confiance des clients
* **Fuite de propriete intellectuelle** : Recettes et compositions exposees publiquement
* **Exploitation par des concurrents** : Acces aux formulations secretes

---

## **4. Actions**

### **Mesures immediates**

* **Supprimer definitivement les produits dangereux** : Hard delete au lieu de soft delete pour les produits retires pour raisons de securite
* **Supprimer le Pastebin expose** : Contacter Pastebin pour retirer le document contenant les informations sensibles
* **Valider tous les liens externes** : Interdire ou sanitiser les URLs dans les descriptions de produits

---
