# **Rapport de vulnérabilité — Database Schema (Injection SQL)**

## **1. Méthodologie**

1. Identification du point d'injection : la recherche de produits via le paramètre `q` à l'URL `https://ctf.juice.cyber.epitest.eu/rest/products/search?q=payload`.
2. Test d'injection SQL pour déterminer le nombre de colonnes attendues dans le résultat.
3. Utilisation d'une injection UNION pour extraire le schéma de la base de données.
4. Payload utilisé : `test')) UNION SELECT 1, 2, 3, 4, 5, 6, 7, 8, sql FROM sqlite_schema--`
5. Extraction réussie du schéma complet de la base de données SQLite.

### **Techniques utilisées**

* Injection SQL UNION
* Exploitation de la table système `sqlite_schema`
* Analyse du nombre de colonnes requises
* Commentaire SQL pour neutraliser la fin de la requête

### **Outils utilisés**

* Navigateur web (barre d'URL)
* Outils de développement du navigateur (Network tab)

---

## **2. Vulnérabilité**

* **Type :** Injection SQL
* **Composant affecté :** API de recherche de produits (`/rest/products/search`)
* **Sévérité :** **Critique** (exposition complète du schéma de base de données)


**Payload décomposé :**
- `test'))` : Fermeture de la requête originale
- `UNION SELECT 1, 2, 3, 4, 5, 6, 7, 8, sql` : Sélection de 8 colonnes (alignement requis) + colonne `sql`
- `FROM sqlite_schema` : Table système SQLite contenant les définitions de toutes les tables
- `--` : Commentaire SQL pour ignorer le reste de la requête

---

## **3. Risques**

* Exposition complète de la structure de la base de données (tables, colonnes, relations)
* Identification de cibles sensibles pour des attaques plus avancées
* Possibilité d'extraire des données sensibles via des injections SQL ultérieures
* Compréhension complète de l'architecture applicative par un attaquant
* Base pour des attaques de type data exfiltration

---

## **4. Actions**

* Implémenter une validation stricte des entrées (whitelist de caractères autorisés)
* Limiter les privilèges du compte de base de données utilisé par l'application
* Mettre en place une détection d'anomalies pour les requêtes suspectes

---
