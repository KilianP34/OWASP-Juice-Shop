# **Rapport de vulnérabilité — Nested Easter Egg (Cryptographic Issues)**

## **1. Méthodologie**

1. Téléchargement du fichier fourni dans le challenge.
2. Identification d’une **chaîne encodée en Base64**.
3. Décodage Base64 → obtention d’un texte représentant un chemin chiffré.
4. Décodage du résultat avec **ROT13**.
5. Le chemin déchiffré mène à une URL secrète affichant une planète → challenge validé.

### **Techniques utilisées**

* Décodage Base64
* Décodage ROT13

### **Outils utilisés**

* `https://www.base64decode.org/`
* `https://rot13.com/`

---

## **2. Vulnérabilité**

* **Type :** Cryptographic Issues
* **Composant affecté :** Donnée encodée dans le fichier easter
* **Sévérité :** **Faible** (Easter egg volontaire, pas une vraie faille)

---

## **3. Risques**

* Aucun risque réel
* **Base64 + ROT13 ≠ chiffrement** et ne doit jamais protéger des données sensibles.

---

## **4. Actions**

* Ne jamais utiliser Base64 ou ROT13 comme mécanisme de sécurité.
* Utiliser des algorithmes de cryptage robustes si un secret doit être protégé.