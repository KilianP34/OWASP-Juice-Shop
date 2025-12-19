# **Rapport de vulnérabilité — Leaked API Key (Sensitive Data Exposure)**

## **1. Méthodologie**

1. Recherche d'un point d'automatisation possible via API sur l'application.
2. Exploration du dépôt GitHub du projet lié (recherche de « juicy-coupon-bot ») pour repérer d'éventuelles références à des clés API ou automatismes.
3. Analyse de l'historique Git (commits) pour détecter d'anciens commits contenant des secrets en clair : recherche par mots-clés ("API", "key", "secret").
4. Identification d'un commit pertinent : **`5c752f5`** contenant des lignes supprimées — parmi ces lignes supprimées se trouvait la clé API en clair : **`6PPi37DBxP4lDwlriuaxP15HaDJpsUXY5TspVmie`**.
5. Utilisation de la clé trouvée dans le champ attendu du site (page **Contact** / fonctionnalité de soumission) → envoi de la clé → challenge validé.

### **Techniques utilisées**

* Analyse de l'historique Git / recherche dans les commits
* OSINT sur les repos liés (juice shop / juicy-coupon-bot)
* Exploitation d'une clé API divulguée

### **Outils utilisés**

* Navigateur web
* Interface Git / exploration des commits (GitHub)
* Recherche de texte dans les diffs de commits

---

## **2. Vulnérabilité**

* **Type :** Sensitive Data Exposure — Leaked API Key / Secrets in Git History
* **Composant affecté :** Référentiel source (`juicy-coupon-bot`) / Historique Git
* **Sévérité :** **Critique** (présence d'une clé active en clair dans l'historique Git)

---

## **3. Risques**

* Utilisation abusive de la clé API par des attaquants (accès non autorisé aux fonctions exposées par l'API)
* Exfiltration de données, automatisation de fraudes ou actions non autorisées via l'API
* Compromission de comptes, abus de fonctionnalités ou déclenchement de coûts (selon les droits de la clé)
* Perte de confiance et dommages réputationnels si la clé est exploitée
* La simple suppression d'une clé du code ne la retire pas de l'historique Git — risque persistant

---

## **4. Actions**

* **Révoquer immédiatement** la clé API exposée (rotation immédiate des secrets affectés).
* Purger l'historique Git pour supprimer définitivement la clé des commits
* Mettre en place une **politique de secrets** : jamais stocker de clés en clair dans le code ni dans les commits.
* Utiliser des **variables d'environnement** pour stocker les clés.
* Auditer les anciens commits pour détecter d'autres clés potentiellement exposées; régénérer/révoquer si nécessaire.
* Restreindre la portée/permissions des clés (principe du moindre privilège) et configurer des quotas et logs d'utilisation.
* Mettre en place une surveillance active (alertes sur usage anormal des API keys) et journalisation (logs).