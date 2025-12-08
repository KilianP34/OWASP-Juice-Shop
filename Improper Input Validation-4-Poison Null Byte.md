# **Rapport de vulnérabilité — Poison Null Byte (Improper Input Validation)**

## **1. Méthodologie**

1. Accès au répertoire **/ftp/** pour tester le téléchargement de fichiers.
2. Tentative de télécharger `easteregg.gg`, bloquée car le serveur n’autorise que `.md` ou `.pdf`.
3. Test d’un bypass via **Null Byte injection** : ajout de `%2500` après le nom du fichier → `eastere.gg%2500.md`.
4. Le serveur interprète seulement la première partie (`eastere.gg`) et la validation de l’extension est contournée.
5. Téléchargement réussi → challenge validé.

### **Techniques utilisées**

* Null Byte poisoning (`%00`, encodé → `%2500`)
* Bypass de validation d’extension côté serveur
* Manipulation d’URL dans un endpoint de téléchargement

### **Outils utilisés**

* Navigateur web

---

## **2. Vulnérabilité**

* **Type :** Improper Input Validation — Null Byte Injection
* **Composant affecté :** `/ftp/eastere.gg` / File download
* **Sévérité :** **Élevée** (bypass complet du filtrage d’extensions)

---

## **3. Risques**

* Téléchargement de fichiers non prévus par le système
* Exposition de fichiers internes

---

## **4. Actions**

* Nettoyer et normaliser les chemins (strip des `%00`, `%2500`)
* Appliquer la validation **après décodage complet** de l’URL