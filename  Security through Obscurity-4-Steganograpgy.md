# **Rapport de vulnerabilite - Steganography (Security through Obscurity)**

## **1. Methodologie**

1. Acces a la galerie de photos sur la page About de Juice Shop.
2. Identification des images du carrousel : 7 images disponibles dans `/assets/public/images/carousel/`.
3. Telechargement de toutes les images (1 a 7) pour analyse.
4. Observation : la 5eme image (`carousel/5`) est au format **PNG** alors que les autres sont en JPG.
5. Utilisation de **OpenStego** specifiquement sur le fichier PNG.
6. Extraction reussie d'une image cachee montrant **Pickle Rick** (personnage de la serie Rick and Morty).
8. Soumission du nom "Pickle Rick" pour valider le challenge.

### **Techniques utilisees**

* Steganographie (dissimulation de donnees dans des fichiers images)
* Analyse de formats de fichiers (PNG vs JPG)
* Extraction de donnees cachees avec OpenStego

### **Outils utilises**

* Navigateur web (telechargement des images)
* OpenStego (https://www.openstego.com/) - extraction de donnees cachees
* Google Lens - identification visuelle

---

## **2. Vulnerabilite**

* **Type :** Security through Obscurity
* **Composant affecte :** Galerie d'images (`/assets/public/images/carousel/`)
* **Severite :** **Moyenne** (exposition d'informations cachees, pas de donnees critiques)

---

## **3. Risques**

* Dissimulation d'informations sensibles dans des fichiers publics
* Exfiltration de donnees via steganographie (canal cache)
* Possibilite de cacher des malwares ou du code malveillant
* Communication secrete via des images apparemment innocentes
* Difficulte de detection par les outils de securite traditionnels

---

## **4. Actions**

* Implementer une **analyse steganographique automatisee** sur les fichiers uploades
* Analyser les metadonnees des images uploadees
* Implementer une validation stricte des fichiers (taille, format, contenu)
* Re-encoder les images uploadees pour detruire les donnees steganographiques

---
