# **Rapport de vulnerabilite - Email Leak (JSONP Callback Manipulation)**

## **1. Methodologie**

1. Identification de l'endpoint : `/rest/user/whoami` retourne les infos de l'utilisateur connecte.
2. Test de parametres URL dans Burp Suite pour modifier le comportement de l'endpoint.
3. Decouverte du parametre `callback` souvent utilise pour JSONP (JSON with Padding).
4. Ajout du parametre `?callback=admin` dans Burp Suite.
5. Requete finale :
```http
GET /rest/user/whoami?callback=admin HTTP/2
Host: ctf.juice.cyber.epitest.eu
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9...
```
6. Reponse obtenue expose l'email :
```
typeof admin === 'function' && admin({"user":{"id":11,"email":"amy@juice-sh.op",...}});
```
7. Challenge valide : email recupere via JSONP.

### **Techniques utilisees**

* Exploitation de JSONP (JSON with Padding)
* Fuzzing de parametres URL
* Manipulation de requetes avec Burp Suite

### **Outils utilises**

* Burp Suite (Repeater pour tester le parametre callback)

---

## **2. Vulnerabilite**

* **Type :** JSONP Callback Manipulation + Information Disclosure
* **Composant affecte :** `/rest/user/whoami`
* **Severite :** **Moyenne** (exposition d'informations sensibles)

### **Details techniques**

L'endpoint accepte un parametre `callback` qui active JSONP. Au lieu de retourner du JSON standard, le serveur encapsule la reponse dans un appel de fonction JavaScript.

**Requete normale :**
```
GET /rest/user/whoami
Reponse : {"user":{"email":"amy@juice-sh.op"}}
```

**Requete avec callback :**
```
GET /rest/user/whoami?callback=admin
Reponse : admin({"user":{"email":"amy@juice-sh.op"}});
```

Le Content-Type change de `application/json` a `text/javascript`. Le parametre `callback` wrapper le JSON dans une fonction JavaScript executable, permettant a n'importe quel site d'appeler cet endpoint et recuperer les donnees utilisateur.

**Vulnerabilites :**
1. JSONP active sans restriction
2. Pas de validation du parametre callback
3. Header `Access-Control-Allow-Origin: *` trop permissif
4. Email et donnees utilisateur exposes

---

## **3. Risques**

* Vol d'informations utilisateur (email, ID, IP)
* JSONP Hijacking : un site malveillant peut recuperer ces donnees
* Enumeration d'utilisateurs
* Base pour du phishing avec les emails recuperes
* Violation de la confidentialite

---

## **4. Actions**

* Supprimer le support JSONP (obsolete depuis CORS)
* Valider strictement le parametre callback
* Remplacer `Access-Control-Allow-Origin: *` par des origines specifiques
* Implementer des tokens anti-CSRF
* Utiliser SameSite cookies

---
