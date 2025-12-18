# **Rapport de vulnerabilite - Imaginary Challenge (Manipulation de Continue Code)**

## **1. Methodologie**

1. Objectif : valider le challenge #999 (inexistant) en forgeant un continue code.
2. Recuperation du continue code actuel depuis le cookie :  continueCode=`5OKnrREzQX92kbvgVpA6NTYfNlu1kIlOtDotnpu35A7yqY1mwe83joLJMDaN`
3. **Identification de la bibliotheque utilisee** :
   - Acces au fichier `package.json.bak` via `/ftp/package.json.bak%2500.md`
   - Decouverte de la dependance `"hashids": "~1.1"` dans les dependencies
   - Hashids est une bibliotheque pour encoder/decoder des IDs en chaines courtes
4. **Recherche des parametres Hashids** :
   - Consultation de la documentation officielle Hashids (hashids.org)
   - Identification du salt d'exemple : `"this is my salt"` (valeur par defaut de la doc)
   - Test avec ce salt → fonctionne ! (les developpeurs n'ont pas change le salt par defaut)
   - Longueur observee : 60 caracteres
   - Alphabet : lettres + chiffres (pas de caracteres speciaux)
5. Script JavaScript pour avoir le code :
```javascript
// Copier cette ligne dans la console pour charger Hashids
var script = document.createElement('script');
script.src = 'https://cdn.jsdelivr.net/npm/hashids@2.2.10/dist/hashids.min.js';
document.head.appendChild(script);

// puis 
var hashids = new Hashids("this is my salt", 60, "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890");

var currentCode = "5OKnrREzQX92kbvgVpA6NTYfNlu1kIlOtDotnpu35A7yqY1mwe83joLJMDaN"; // continueCode du Cookie
var challenges = hashids.decode(currentCode);
console.log("Challenges resolus:", challenges);

challenges.push(999);
var newCode = hashids.encode(challenges);
console.log("Nouveau continue code:", newCode);
```
6. Requete PUT pour valider :
```http
PUT /rest/continue-code/apply/RESULTAT HTTP/2
Host: ctf.juice.cyber.epitest.eu
Authorization: Bearer [token]
```
7. Challenge #999 valide avec succes.

### **Techniques utilisees**

* Analyse du package.json.bak pour identifier les dependances
* Reverse engineering avec Hashids
* Exploitation de parametres par defaut non modifies

### **Outils utilises**

* Acces FTP (`/ftp/package.json.bak%2500.md`)
* Hashids JavaScript
* Burp Suite

---

## **2. Vulnerabilite**

* **Type :** Parametres par defaut + Absence de signature
* **Composant :** `/rest/continue-code/apply`
* **Severite :** **Critique**

**Problemes :**
1. **Salt par defaut** : `"this is my salt"` est l'exemple de la doc Hashids (valeur publique)
2. **Validation absente** : serveur accepte n'importe quel ID (meme 999 inexistant)
3. **package.json.bak accessible** : expose les dependances et versions utilisees

---

## **3. Risques**

* Validation de challenges non resolus (triche totale)
* Contournement du systeme de progression

---

## **4. Actions**

* **JAMAIS utiliser les valeurs par defaut** pour la securite
* **Salt aleatoire unique** genere au deploiement
* **JWT avec signature** au lieu de Hashids
* **Validation serveur** des IDs de challenges
* **Proteger package.json.bak** (ne pas exposer les dependances)

---
