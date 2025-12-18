# **Rapport de vulnerabilite - Extra Language (Broken Anti-Automation)**

## **1. Methodologie**

1. Analyse du systeme de langues : l'application utilise des fichiers JSON localises dans `/assets/i18n/{code_langue}_{CODE_PAYS}.json`.
2. Interception d'une requete de changement de langue via les outils de developpement du navigateur (Network tab).
3. Observation du format des codes : ISO 639-1 (2 lettres pour la langue) + ISO 3166-1 alpha-2 (2 lettres pour le pays).
4. Consultation de la plateforme Crowdin du projet Juice Shop pour identifier les langues disponibles mais non exposees dans l'interface.
5. Decouverte de la langue Klingon (Star Trek) avec le code `tlh_AA` presente dans les traductions Crowdin.
6. Modification manuelle du parametre de langue dans l'application ou via l'URL pour charger `tlh_AA`.
7. Validation du challenge apres chargement reussi de la langue Klingon.

### **Techniques utilisees**

* Analyse de la structure des fichiers de localisation
* Reconnaissance via plateforme de traduction collaborative (Crowdin)
* Manipulation des parametres de langue
* Exploitation de langues non documentees

### **Outils utilises**

* Outils de developpement du navigateur (Network tab, Application/Storage)
* Plateforme Crowdin (https://crowdin.com/project/owasp-juice-shop)
* Documentation officielle OWASP Juice Shop

---

## **2. Vulnerabilite**

* **Type :** Broken Anti-Automation / Improper Input Validation
* **Composant affecte :** Systeme de localisation (`/assets/i18n/`)
* **Severite :** **Moyenne** (exposition de fonctionnalites cachees, pas de fuite de donnees sensibles)

---

## **3. Risques**

* Acces a des fonctionnalites ou langues non destinees a la production
* Exposition de traductions incompletes ou contenant des informations de debug
* Possibilite de decouvrir la structure interne des fichiers de configuration
* Contournement des restrictions d'interface utilisateur
* Potentiel d'injection si les fichiers de langue ne sont pas correctement sanitises

---

## **4. Actions**

* Implementer une **liste blanche stricte** des codes de langue autorises
* Ne pas exposer directement la structure de fichiers `/assets/i18n/` sans controle
* Separer les langues en production des langues en developpement/test
* Utiliser un mecanisme de configuration centralise pour gerer les langues disponibles

---
