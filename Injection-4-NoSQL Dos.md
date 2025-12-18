# **Rapport de vulnerabilite - NoSQL DoS (Injection NoSQL)**

## **1. Méthodologie**

1. Identification du point d'injection : l'endpoint de modification des avis produits `/rest/products/[productId]/reviews`.
2. Analyse des requetes PUT interceptees via Burp Suite pour identifier les parametres non valides.
3. Test de differents payloads NoSQL dans le parametre `productId` de l'URL.
4. Decouverte que l'injection se fait directement dans l'URL et non dans le corps JSON de la requete.
5. Utilisation de la fonction `sleep()` de MongoDB pour provoquer un Denial of Service (DoS).
6. Payload final utilise :
```
GET /rest/products/sleep(20000)/reviews HTTP/2
```
7. Confirmation du DoS : le serveur devient non-reactif pendant 20 secondes (20000 millisecondes).

### **Techniques utilisees**

* Injection NoSQL dans le parametre URL
* Exploitation de la fonction `sleep()` de MongoDB
* Attaque par Denial of Service (DoS)
* Interception et modification de requetes HTTP via proxy
* Bypass de la validation cote client (injection cote serveur)

### **Outils utilises**

* Burp Suite (interception et modification des requetes)
* Navigateur web (acces direct a l'URL vulnerable)
* Outils de developpement du navigateur (Network tab)

---

## **2. Vulnerabilite**

* **Type :** Injection NoSQL + Denial of Service (DoS)
* **Composant affecte :** API de gestion des avis produits (`/rest/products/[productId]/reviews`)
* **Severite :** **Elevee** (possibilite de rendre le serveur indisponible)

---

## **3. Risques**

* **Denial of Service (DoS)** : Indisponibilite du service pour les utilisateurs legitimes
* **Impact sur la reputation** : Perte de confiance des clients lors d'indisponibilites
* **Pertes financieres** : Interruption de service pour une plateforme e-commerce
* **Exploitation facilitee d'autres vulnerabilites** : Pendant le DoS, les systemes de detection peuvent etre debordes
* **Escalade possible** : Si l'injection NoSQL permet d'executer du code arbitraire, risque de compromission totale

---

## **4. Actions**

### **Mesures immediates**

* **Valider et sanitiser TOUS les parametres d'entree**, y compris ceux dans l'URL
---