# **Rapport de vulnerabilite - Upload Type (Improper Input Validation)**

## **1. Methodologie**

1. Acces a la fonctionnalite de plainte (Complaint) sur Juice Shop.
2. Tentative d'upload d'un fichier texte (.txt) via l'interface : rejet par la validation cote client (seules les images sont acceptees).
3. Interception de la requete d'upload d'une image legitime avec Burp Suite ou les outils de developpement du navigateur.
4. Observation de la structure multipart/form-data :
   ```
   Content-Disposition: form-data; name="file"; filename="image.jpg"
   Content-Type: image/jpeg
   ```
5. Modification de la requete interceptee pour uploader un fichier texte :
   - Changement du `filename` : `filename="unamed.txt"`
   - Changement du `Content-Type` : `Content-Type: application/txt`
   - Remplacement du contenu par du texte arbitraire
6. Envoi de la requete modifiee au serveur.
7. Validation du challenge : le serveur accepte le fichier .txt sans verification cote serveur.

### **Techniques utilisees**

* Interception et modification de requetes HTTP
* Bypass de la validation cote client
* Manipulation des headers HTTP (Content-Type, Content-Disposition)
* Exploitation d'une validation de type MIME insuffisante

### **Outils utilises**

* Burp Suite (interception et modification de requetes)
* Outils de developpement du navigateur (Network tab, Edit and Resend)

---

## **2. Vulnerabilite**

* **Type :** Improper Input Validation / Unrestricted File Upload
* **Composant affecte :** Fonctionnalite d'upload de fichiers (`/file-upload` ou `/api/Complaints`)
* **Severite :** **Elevee** (risque d'upload de fichiers malveillants)

---

## **3. Risques**

* **Upload de webshells** : Fichiers PHP/JSP/ASPX permettant l'execution de commandes sur le serveur
* **Upload de malwares** : Fichiers executables (.exe, .sh, .bat) pouvant infecter le serveur ou les utilisateurs
* **Attaques XSS stockees** : Upload de fichiers HTML/SVG avec du JavaScript malveillant
* **Defacement** : Remplacement de fichiers legitimes par du contenu malveillant
* **Deni de service** : Upload de fichiers tres volumineux saturant l'espace disque
* **Compromission totale du serveur** : Si execution de code possible via les fichiers uploades
* **Propagation de virus** : Fichiers infectes telecharges par d'autres utilisateurs

---

## **4. Actions**

### **Mesures immediates**

* **Valider TOUS les uploads cote serveur** : Ne jamais faire confiance au client
* Implementer une **whitelist stricte des extensions** autorisees (exemple : .jpg, .png, .pdf uniquement)
* Verifier le **contenu reel du fichier** (magic bytes/file signature) et non juste le Content-Type
* Rejeter tout fichier dont l'extension ne correspond pas au contenu reel

---
