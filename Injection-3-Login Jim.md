# **Rapport de vulnérabilité — Injection SQL**

## **1. Méthodologie**

1. Recherche d’informations (**OSINT**) : email de *Jim* trouvé dans un avis produit.
2. Test d'injection sur le formulaire de connexion.
3. Injection SQL manuelle avec : `jim@juice-sh.op'--`
4. Connexion réussie au compte sans mot de passe.

## **2. Vulnérabilité**

* **Type :** Injection SQL
* **Composant touché :** Système d’authentification / requête SQL login
* **Sévérité :** **Critique** (bypass complet de login)

## **3. Risques**

* Accès non autorisé aux comptes utilisateurs
* Fuite potentielle de données personnelles
* Atteinte majeure à la réputation du service

## **4. Actions**

* Utiliser des **requêtes préparées** / paramètres SQL
* Valider et échapper toutes les entrées utilisateur
