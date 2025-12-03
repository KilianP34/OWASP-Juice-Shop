# **Rapport de vulnérabilité — Forged Review (Broken Access Control)**

## **1. Méthodologie**

1. Observation du fonctionnement du système d’avis produit via l’onglet **Network**.
2. Capture de la requête **PUT** utilisée pour modifier un avis existant.
3. Modification manuelle du champ **"author"** dans le body de la requête.
4. Renvoi de la requête : l’avis a été mis à jour au nom d’un autre utilisateur.

### **Techniques utilisées**

* Analyse de requêtes HTTP
* Manipulation de données côté client
* Bypass d’autorisation (IDOR / BAC)

### **Outils utilisés**

* Navigateur web (Network tab)

---

## **2. Vulnérabilité**

* **Type :** Usurpation d'identitée
* **Composant affecté :** API de gestion des avis (endpoint PUT review)
* **Sévérité :** **Critique** (usurpation d’identité + modification de données)

---

## **3. Risques**

* Usurpation d’identité d’utilisateurs
* Altération de la crédibilité des avis produits
* Manipulation possible de l’image de la marque

---

## **4. Actions**

* Vérifier côté serveur que l’**author** = l’utilisateur authentifié

---