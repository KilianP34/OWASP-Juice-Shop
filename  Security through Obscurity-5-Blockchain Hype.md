# **Rapport de vulnerabilite - Blockchain Hype (Security through Obscurity)**

## **1. Methodologie**

1. Identification de l'objectif : decouvrir une page ICO (Initial Coin Offering) cachee pour "JuicyCoin".
2. Analyse du fichier `main.js` via DevTools en recherchant des patterns suspects dans les routes.
3. Decouverte d'un code obfusque dans le main.js
4. Extraction du code obfusque :
```javascript
matcher: function op(n) {
  return 0 === n.length ? null : n[0].toString().match(
    function ip(...n) {
      const a = Array.prototype.slice.call(n),
      e = a.shift();
      return a.reverse().map(function (o, i) {
        return String.fromCharCode(o - e - 45 - i)
      }).join('')
    }(25, 184, 174, 179, 182, 186) + 'sal'.toLowerCase() +
    function ap() {
      const a = Array.prototype.slice.call(arguments),
      e = a.shift();
      return a.reverse().map(function (o, i) {
        return String.fromCharCode(o - e - 24 - i)
      }).join('')
    }(13, 144, 87, 152, 139, 144, 83, 138) + 'a'.toLowerCase()
  )
}
```
5. De-obfuscation du code dans la console JavaScript (F12 → Console) :
   - Copier la premiere fonction et l'executer :
```javascript
(function ip(...n) {
  const a = Array.prototype.slice.call(n),
  e = a.shift();
  return a.reverse().map(function (o, i) {
    return String.fromCharCode(o - e - 45 - i)
  }).join('')
})(25, 184, 174, 179, 182, 186)
// Resultat: "token"
```
   - Deuxieme partie est deja en clair : `'sal'.toLowerCase()` → `"sal"`
   - Copier la troisieme fonction et l'executer :
```javascript
(function ap() {
  const a = Array.prototype.slice.call(arguments),
  e = a.shift();
  return a.reverse().map(function (o, i) {
    return String.fromCharCode(o - e - 24 - i)
  }).join('')
})(13, 144, 87, 152, 139, 144, 83, 138)
// Resultat: "e-ico-e"
```
   - Quatrieme partie deja en clair : `'a'.toLowerCase()` → `"a"`
6. Concatenation des 4 parties : `"token" + "sal" + "e-ico-e" + "a"` = `"tokensale-ico-ea"`
7. Acces au chemin decouvert : `https://ctf.juice.cyber.epitest.eu/#/tokensale-ico-ea`
8. Validation du challenge : acces reussi a la page ICO pre-launch de JuicyCoin.

### **Techniques utilisees**

* Analyse de code JavaScript obfusque
* De-obfuscation manuelle (execution dans la console)
* Identification de patterns d'obfuscation par calcul ASCII

### **Outils utilises**

* DevTools navigateur (Sources, Console JavaScript)
* Console pour executer les fonctions obfusquees

---

## **2. Vulnerabilite**

* **Type :** Security through Obscurity
* **Composant affecte :** Routes de l'application (page ICO cachee)
* **Severite :** **Moyenne** (information sensible accessible par obfuscation faible)

**Problemes identifies :**

1. **Obfuscation reversible** : Les calculs `String.fromCharCode()` sont facilement executables dans la console
2. **Aucune authentification** : Une fois le chemin decouvert, la page est directement accessible
3. **Code client-side** : Tout le code JavaScript est accessible et analysable

---

## **3. Risques**

* Exposition d'informations sensibles (details ICO pre-launch)
* Fuite d'informations marketing confidentielles
---

## **4. Actions**

* **Ne JAMAIS compter sur l'obfuscation** pour proteger des fonctionnalites sensibles
* **Implementer une vraie authentification** pour les pages pre-launch
* **Utiliser des controles d'acces serveur-side** (pas client-side)
* **Ne pas exposer les routes cachees** dans le code JavaScript frontend

---
