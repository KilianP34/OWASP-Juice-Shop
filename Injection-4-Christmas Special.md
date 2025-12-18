# **Rapport de vulnérabilité — Christmas Special (Injection SQL)**

## **1. Méthodologie**

1. Identification du point d'injection : la fonctionnalité de recherche de produits via le paramètre `q` à l'URL `https://ctf.juice.cyber.epitest.eu/rest/products/search?q=payload`.
2. Hypothèse : les produits spéciaux (comme l'offre de Noël) pourraient être marqués comme supprimés (soft delete) plutôt que complètement effacés de la base de données.
3. Test d'injection SQL pour récupérer les produits avec `deletedAt IS NOT NULL`.
4. Payload utilisé : `test')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--`
5. Identification du produit caché : l'offre de Noël avec l'ID `10`.
6. Consultation du panier via `GET /rest/basket/6` pour comprendre la structure des requêtes.
7. Ajout du produit caché au panier via interception de requête : `POST /api/BasketItems/` avec le body `{"ProductId":10,"BasketId":"6","quantity":1}`.

### **Techniques utilisées**

* Injection SQL UNION
* Exploitation de la logique de soft delete (suppression logique)
* Interception et modification de requêtes HTTP
* Manipulation de paramètres API

### **Outils utilisés**

* Navigateur web (barre d'URL)
* Outils de développement du navigateur (Network tab)
* Burp Suite (interception de requêtes)

---

## **2. Vulnérabilité**

* **Type :** Injection SQL + Broken Access Control
* **Composant affecté :** API de recherche de produits (`/rest/products/search`) et API panier (`/api/BasketItems/`)
* **Sévérité :** **Élevée** (accès à des produits censés être inaccessibles)

**Problèmes identifiés :**
1. **Injection SQL** : Permet d'extraire les produits marqués comme supprimés
2. **Contrôle d'accès défaillant** : L'API `/api/BasketItems/` accepte n'importe quel `ProductId` sans vérifier si le produit est actif ou accessible
3. **Logique métier exposée** : Le soft delete révèle l'existence de produits cachés

**Payload décomposé :**
- `test'))` : Fermeture de la requête originale
- `UNION SELECT * FROM PRODUCTS` : Sélection de tous les champs de la table PRODUCTS
- `WHERE deletedAt IS NOT NULL` : Filtrage pour n'obtenir que les produits "supprimés"
- `--` : Commentaire SQL pour ignorer le reste de la requête

**Exploitation de l'API panier :**
```json
{
  "ProductId": 10,
  "BasketId": "6",
  "quantity": 1
}
```

---

## **3. Risques**

* Accès à des produits censés être retirés de la vente
* Possibilité d'acheter des produits à des prix non autorisés
* Impact financier potentiel pour l'entreprise
* Découverte de la structure de la base de données
* Base pour des attaques plus complexes (extraction d'autres données sensibles)

---

## **4. Actions**

* Valider et échapper toutes les entrées utilisateur avant traitement SQL
* Implémenter une validation stricte des entrées (whitelist de caractères autorisés)
* **Vérifier la disponibilité des produits** lors de l'ajout au panier (vérifier que `deletedAt IS NULL`)
* Implémenter un contrôle d'accès strict sur l'API panier
* Envisager une suppression définitive (hard delete) plutôt qu'une suppression logique pour les produits sensibles
* Ajouter une validation côté serveur pour s'assurer que seuls les produits actifs peuvent être ajoutés au panier
* Mettre en place une détection d'anomalies pour les requêtes suspectes

---
