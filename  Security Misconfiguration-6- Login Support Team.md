# **Rapport de vulnerabilite - Login Support Team (Security Misconfiguration)**

## **1. Methodologie**

1. Analyse du fichier `main.js` de l'application Juice Shop.
2. Decouverte d'une phrase en roumain obfusquee : "Le mot de passe de l'equipe support ne respecte pas la politique d'entreprise pour les comptes privilegies ! Veuillez changer votre mot de passe en consequence !"
3. Identification du regex de politique de mot de passe dans le code :
   ```regex
   /(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{12,30}/
   ```
   - Au moins 1 minuscule
   - Au moins 1 majuscule
   - Au moins 1 chiffre
   - Au moins 1 caractere special (@$!%*?&)
   - Longueur : 12 a 30 caracteres
4. Acces au serveur FTP de Juice Shop pour rechercher des fichiers sensibles.
5. Telechargement du fichier **incident-support.kdbx** (base de donnees KeePass) depuis le FTP.
6. Installation de KeePass2 pour ouvrir le fichier .kdbx.
7. Tentative de craquage du mot de passe maitre du fichier KeePass :
   - Conversion du fichier avec `keepass2john incident-support.kdbx > keepasshash.txt`
   - Utilisation de John the Ripper ou Hashcat avec une wordlist personnalisee respectant le regex
   - Mot de passe maitre trouve : **Support2022!**
8. Ouverture de la base KeePass avec le mot de passe maitre.
9. Extraction des credentials du compte Support Team :
   - Email : `support@juice-sh.op`
   - Password : `J6aVjTg0pRs@?5l!Zkq2AYnCE@RF$P`
10. Connexion reussie au compte Support Team sur Juice Shop.

### **Techniques utilisees**

* Analyse de code source JavaScript (main.js)
* Reconnaissance d'informations sensibles dans le code
* Exploitation du serveur FTP non securise
* Craquage de mot de passe KeePass (brute-force avec wordlist)
* Extraction de credentials depuis une base de donnees de mots de passe

### **Outils utilises**

* Navigateur web (analyse de main.js)
* Client FTP
* KeePass2 (ouverture du fichier .kdbx)
* keepass2john (conversion pour craquage)
* John the Ripper ou Hashcat (craquage de mot de passe)

---

## **2. Vulnerabilite**

* **Type :** Security Misconfiguration / Sensitive Data Exposure
* **Composant affecte :** Serveur FTP + Fichier KeePass expose + Code source revelant la politique
* **Severite :** **Critique** (exposition de credentials privilegies)   

---

## **3. Risques**

* **Compromission de comptes privilegies** : Acces au compte Support Team avec privileges eleves
* **Exposition de multiples credentials** : Le fichier KeePass peut contenir d'autres comptes sensibles
* **Escalade de privileges** : Le compte Support peut avoir acces a des fonctionnalites administratives
* **Violation de confidentialite** : Acces aux tickets de support et donnees clients
* **Perte de confiance** : Compromission d'un compte de supportmine la credibilite
* **Non-conformite** : Violation des standards de securite (ISO 27001, SOC 2)
* **Attaques ulterieures** : Les credentials peuvent etre reutilises sur d'autres systemes

---

## **4. Actions**

### **Mesures immediates**

* **Supprimer immediatement le fichier KeePass du FTP** : Ne jamais stocker de fichiers sensibles sur des serveurs accessibles
* **Changer tous les mots de passe exposes** : Reinitialiser le mot de passe du compte Support Team
* **Securiser le serveur FTP** : Implementer une authentification forte ou desactiver le FTP
* **Ne jamais exposer la politique de mot de passe cote client** : Valider uniquement cote serveur

---
