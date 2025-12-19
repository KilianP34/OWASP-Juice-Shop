# **Rapport de vulnerabilite - Payback Time (Improper Input Validation)**

## **1. Methodologie**

1. Ajout d'un produit au panier via l'interface utilisateur normale.
2. Interception de la requete PUT de mise a jour du panier avec les outils de developpement ou Burp Suite :
   - Endpoint : `PUT /api/BasketItems/{id}`
   - Observation de la structure JSON contenant le champ `quantity`
3. Modification de la requete interceptee :
   - Changement de la quantite a une valeur negative (exemple : `-200`)
   - Envoi de la requete modifiee au serveur
4. Validation cote serveur : le serveur accepte la quantite negative sans validation.
5. Passage de la commande via l'interface de checkout.
6. Resultat : le total du panier devient negatif, creant un credit au lieu d'un debit.
7. Validation du challenge apres avoir complete une commande avec un montant negatif.

### **Techniques utilisees**

* Manipulation de requetes HTTP (interception et modification)
* Exploitation d'une faille de logique metier (business logic error)
* Bypass de la validation cote client
* Manipulation de parametres API

### **Outils utilises**

* Burp Suite (interception et modification de requetes)
* Outils de developpement du navigateur (Network tab, Edit and Resend)

---

## **2. Vulnerabilite**

* **Type :** Improper Input Validation + Business Logic Error
* **Composant affecte :** API de gestion du panier (`PUT /api/BasketItems/{id}`)
* **Severite :** **Critique** (perte financiere directe pour l'entreprise)

**Problemes identifies :**
1. **Pas de validation cote serveur** : Aucune verification que `quantity > 0`
2. **Logique metier defaillante** : Le systeme traite les quantites negatives comme des credits
3. **Validation uniquement cote client** : Facilement bypassable via interception de requetes

---

## **3. Risques**

* **Pertes financieres directes** : Les utilisateurs peuvent obtenir des credits au lieu de payer
* **Fraude a grande echelle** : Exploitation massive par des attaquants
* **Impact sur la tresorerie** : Credits non justifies verses aux comptes utilisateurs
* **Faillite potentielle** : Si exploite massivement avant detection
* **Atteinte a la reputation** : Perte de confiance des investisseurs et clients

---

## **4. Actions**

### **Mesures immediates**

* **Valider TOUTES les entrees cote serveur** : Ne jamais faire confiance aux donnees client
* Implementer une validation stricte : `quantity` doit etre un entier positif (> 0)
* Ajouter des contraintes de base de donnees : `CHECK (quantity > 0)`
* Rejeter toute requete avec des valeurs negatives ou nulles

---
