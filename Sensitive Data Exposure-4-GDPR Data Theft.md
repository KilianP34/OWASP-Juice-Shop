# **Rapport de vulnérabilité — Sensitive Data Exposure / GDPR Data Theft**

## **1. Méthodologie**

1. Création d'un compte `kik@txt.fr` puis achat d'articles.
2. Analyse de la réponses du serveur sur le paquet de la commande `6d5c-3419ce0e91a93a6c`.
3. Observation du format du champ "email" qui hide avec des `*` uniquement les voyelles.
4. Création d'un nouvel utilisateur avec l'adresse email `kok@txt.fr` (la variation de l'email de l'utilisateur `kik@txt.fr` je remplace donc la voyelle, suspectant une mauvaise gestion des caractères).
5. Demande d'exportation des données pour ce nouvel utilisateur `kok@txt.fr` sans historique de commande.
6. Réception des données personnelles (commandes, adresse, etc.) de l'utilisateur `kik@txt.fr` au lieu de celles du nouveau compte.

### **Techniques utilisées**

* Analyse de trafic HTTP
* Exploitation de la logique
* Test de confusion d'identité

### **Outils utilisés**

* Burp Suite
* Navigateur web (Outils de développement)

---

## **2. Vulnérabilité**

* **Type :** Exposition de données sensibles
* **Composant affecté :** Fonctionnalité d'exportation de données
* **Sévérité :** Très critique

---

## **3. Risques**

* Vol de données personnelles d'autres utilisateurs particulier ou admin.
* Violation grave des réglementations sur la protection des données (RGPD).
* Perte de confiance des utilisateurs et atteinte à la réputation.

---

## **4. Actions**

* S'assurer que l'exportation de données utilise l'identifiant unique de l'utilisateur authentifié (User ID) et non une recherche approximative sur l'email.
* Désactiver l'utilisation de caractères génériques dans les requêtes de base de données pour récupérer les informations utilisateur sensibles.