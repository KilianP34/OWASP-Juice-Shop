# **Rapport de vulnerabilite - Expired Coupon (Validation Client-Side)**

## **1. Methodologie**

1. Identification de la fonctionnalite de coupons de réduction
2. Recherche du mot "coupon" dans le fichier `main.js` via les DevTools (Sources).
3. Decouverte de la fonction JavaScript responsable de l'application des coupons avec validation client-side.
4. Analyse du code JavaScript : identification d'une liste hardcodee de campagnes contenant :
   - Codes de coupons
   - Pourcentages de reduction
   - Timestamps de validite (debut et fin)
5. Selection d'un coupon expire dans la liste (par exemple : code expire depuis plusieurs mois).
6. Conversion du timestamp Unix du coupon en date lisible via un convertisseur en ligne.
7. **Manipulation de l'horloge systeme** :
   - Modification de la date/heure du systeme pour correspondre a la periode de validite du coupon
   - Par exemple : si le coupon etait valide du 01/01/2024 au 31/01/2024, regler l'horloge sur le 15/01/2024
8. Application du code de coupon expire via l'interface web pendant le processus de checkout.
9. Validation du challenge : le systeme accepte le coupon car la validation client-side utilise l'horloge systeme manipulee.

### **Techniques utilisees**

* Analyse du code JavaScript client-side (main.js)
* Manipulation de l'horloge systeme (date/heure)
* Exploitation de validation cote client au lieu de cote serveur
* Reverse engineering de la logique de validation des coupons

### **Outils utilises**

* DevTools navigateur (Sources, recherche dans le code)
* Convertisseur de timestamp Unix en ligne
* Parametres systeme (modification date/heure)

---

## **2. Vulnerabilite**

* **Type :** Input Validation Client-Side Validation
* **Composant affecte :** Systeme de coupons de reduction
* **Severite :** **Moyenne** (contournement des regles metier, perte financiere)

**Exploitation :**

1. L'attaquant trouve un coupon expire dans le code source JavaScript
2. Il modifie son horloge systeme pour correspondre a la periode de validite du coupon
3. Il applique le coupon qui est accepte car la validation JavaScript utilise l'horloge manipulee
4. Le serveur ne reverifie pas l'expiration et applique la reduction

---

## **3. Risques**

* Perte financiere pour l'entreprise (reductions non autorisees)
* Utilisation de coupons expires deliberement pour obtenir des avantages indus
* Fraude a grande echelle si automatisee
* Impact sur la rentabilite et les marges commerciales

---

## **4. Actions**

* **TOUJOURS valider cote serveur** : Ne jamais faire confiance au client pour la validation
* **Ne pas exposer la liste complete des coupons** dans le code JavaScript
* **Implementer une API securisee** pour valider les coupons
* **Audit logging** : Logger toutes les applications de coupons avec timestamp serveur
* **Rate limiting** : Limiter le nombre de tentatives d'application de coupon
* **Limites d'utilisation** : Implementer des compteurs d'utilisation par coupon

---