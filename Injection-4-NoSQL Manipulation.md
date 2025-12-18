# **Rapport de vulnerabilite - NoSQL Manipulation (Injection NoSQL)**

## **1. Methodologie**

1. Connexion en tant qu'administrateur pour obtenir les privileges necessaires.
2. Identification du point d'injection : l'endpoint de modification des avis produits `/rest/products/reviews` via la methode PATCH.
3. Analyse de la structure de la requete PATCH normale pour modifier un seul avis.
4. Test de differents operateurs NoSQL dans le champ `id` pour contourner la validation.
5. Decouverte que l'operateur `$gt` (greater than) avec une chaine vide permet de matcher tous les avis.
6. Payload final utilise :
```http
PATCH /rest/products/reviews HTTP/2
Host: ctf.juice.cyber.epitest.eu
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGc...
Content-Type: application/json

{
  "id": {
    "$gt": ""
  },
  "message": "LAZARUS !!"
}
```
7. Validation du challenge : **29 avis modifies simultanement** au lieu d'un seul.

### **Techniques utilisees**

* Injection NoSQL avec operateur MongoDB `$gt` (greater than)
* Modification en masse de documents dans une base de donnees NoSQL
* Exploitation de la validation insuffisante des parametres
* Interception et modification de requetes PATCH via Burp Suite

### **Outils utilises**

* Burp Suite (interception et modification des requetes PATCH)
* Navigateur web avec outils de developpement (Network tab)
* Compte administrateur compromis

---

## **2. Vulnerabilite**

* **Type :** Injection NoSQL (Mass Assignment)
* **Composant affecte :** API de gestion des avis produits (`/rest/products/reviews`)
* **Severite :** **Critique** (modification en masse de donnees sans autorisation)

---

## **3. Risques**

* Modification en masse de donnees : 29 avis produits modifies simultanement
* Perte d'integrite des donnees (avis authentiques remplaces)
* Atteinte a la reputation (avis falsifies visibles par les clients)
* Escalade de privileges (un utilisateur lambda pourrait modifier tous les avis)
* Possibilite d'injection XSS combinee via les messages d'avis

---

## **4. Actions**

### **Mesures immediates**

* Valider STRICTEMENT le type de donnees du champ `id` (doit etre une string, pas un objet)
* Utiliser des requetes parametrees avec validation de schema
* Interdire les operateurs NoSQL dans les entrees utilisateur

---
