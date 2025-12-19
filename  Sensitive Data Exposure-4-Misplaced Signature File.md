# **Rapport de vulnerabilite - Misplaced Signature File (Sensitive Data Exposure)**

## **1. Methodologie**

1. Acces au serveur FTP de Juice Shop pour explorer les fichiers disponibles.
2. Identification de fichiers suspects ou sensibles stockes sur le FTP.
3. Tentative d'acces a un fichier avec extension `.yaml` : acces bloque par une restriction de visibilite.
4. Utilisation de la technique du **null byte injection** pour contourner la restriction :
   - Ajout de `%2500.md` apres l'extension `.yaml`
   - Payload complet : `fichier.yaml%2500.md`
   - Le `%25` represente le caractere `%` encode en URL
   - `%00` represente le null byte
   - `.md` est ajoute pour tromper le filtre d'extension
5. Le serveur traite la requete et ignore tout apres le null byte `%00`, permettant d'acceder au fichier `.yaml`.
6. Telechargement reussi du fichier de signature (signature file) contenant des informations sensibles.
7. Validation du challenge apres avoir accede au fichier normalement protege.

### **Techniques utilisees**

* Null byte injection (%00 encode en %2500)
* Bypass de restrictions d'extension de fichiers
* Exploitation de vulnerabilites de parsing d'URL
* Reconnaissance via serveur FTP non securise

### **Outils utilises**

* Client FTP ou navigateur web
* URL encoding pour injecter le null byte

---

## **2. Vulnerabilite**

* **Type :** Sensitive Data Exposure / Path Traversal / Null Byte Injection
* **Composant affecte :** Serveur FTP + mecanisme de validation d'extension de fichiers
* **Severite :** **Elevee** (acces a des fichiers sensibles normalement proteges)

---

## **3. Risques**

* **Exposition de fichiers de configuration sensibles** : Fichiers .yaml contenant des secrets, cles API, credentials
* **Vol de signatures numeriques** : Compromission de l'integrite des signatures
* **Acces a des fichiers systeme** : Potentiel d'acces a d'autres fichiers proteges via la meme technique
* **Escalade d'attaque** : Les informations obtenues peuvent mener a d'autres vulnerabilites
* **Violation de confidentialite** : Exposition de donnees proprietaires ou sensibles

---

## **4. Actions**

### **Mesures immediates**

* **Retirer immediatement les fichiers sensibles du FTP** : Les fichiers de signature ne doivent jamais etre accessibles publiquement
* **Corriger la validation d'extension** : Utiliser une validation robuste resistant aux null bytes
* **Bloquer les null bytes** : Rejeter toute requete contenant `%00`, `%2500`, ou autres variantes encodees
* **Valider le chemin complet** : Verifier le chemin reel du fichier apres toutes les transformations

### **Mesures de securite additionnelles**

* **Desactiver le serveur FTP public** : Utiliser des methodes securisees (SFTP, SCP) avec authentification
* **Implementer une whitelist stricte** : Autoriser uniquement les extensions explicitement autorisees
* **Normaliser les chemins** : Decoder completement les URLs et valider apres normalisation
* **Implementer un controle d'acces strict** : Authentification et autorisation pour tout acces FTP
* **Logger tous les acces FTP** : Detection des tentatives d'acces anormales

### **Bonnes pratiques**

* **Principe du moindre privilege** : Limiter l'acces aux fichiers au strict necessaire
* **Defense en profondeur** : Combiner validation d'extension + controle d'acces + chiffrement
* **Audits reguliers** : Scanner les serveurs pour detecter les fichiers sensibles mal places

---
