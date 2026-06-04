---
title: Travail réalisé
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Réalisation

---

## Architecture ou structure générale

### Architecture logicielle

La plateforme est développée selon une architecture microservices. Le système est découpé en quatre services indépendants, chacun avec sa propre base de données :


Hauméa: Point d'entrée principal — authentification, gestion des usagers, gestion des JWT, envoi des courriels
Cérès :Gestion des clubs, membres et abonnements 
Chiron :Données bancaires et paiements 
MakeMake: Tests d'intégration automatisés 

Le frontend communique uniquement avec Hauméa. Hauméa redirige les requêtes vers Cérès ou Chiron selon le besoin.

### Technologies utilisées

- **Backend** : Java avec Spring Boot
- **Base de données** : MariaDB
- **Migration de base de données** : Liquibase
- **Tests d'intégration** : Cucumber, Gherkin
- **Tests d'APIs** : Bruno
- **Gestion de projet** : ClickUp
- **Versionnement** : Git

### Environnement de développement

- IntelliJ IDEA — éditeur de code
- DataGrip — gestion des bases de données
- JetBrains Toolbox — gestionnaire des outils JetBrains
- MariaDB local avec les bases de données haumeadb, ceresdb et chirondb

---

## Fonctionnalités ou composantes réalisées

### Maquettes de l'interface web

14 maquettes ont été conçues dans Figma pour couvrir les principales pages de la plateforme, basées sur le document d'appel d'offres et les discussions avec l'équipe:

**Pages publiques**

- Page d'inscription — formulaire avec nom d'usager et email, message de confirmation avec lien valide 72 heures
- Page de connexion — nom d'usager et mot de passe, lien mot de passe oublié
- Page de récupération de mot de passe — envoi d'un lien de réinitialisation par courriel

**Espace Membre**

- Tableau de bord Membre — statut, clubs actifs, date d'expiration, bouton de renouvellement
- Page de profil — modification des informations personnelles et du mot de passe
- Page de paiement — renouvellement d'abonnement avec option de don à la FAAQ, paiement par carte certifié PCI
- Carte de membre numérique — à imprimer, une carte par club

**Espace Club**

- Tableau de bord Club — statistiques des membres, membres actifs, inactifs et expirant bientôt
- Page d'ajout de membre — formulaire pour ajouter un membre manuellement
- Gestion des tarifs — créer et modifier les tarifs d'abonnement avec date d'activation
- Page de rapports — filtres par statut, type, région avec option d'export

**Espace Fédération**

- Tableau de bord Fédération — vue globale de tous les clubs et membres
- Configuration de la plateforme — personnalisation des messages, courriels, politiques et champs de données
- Gestion des rôles et privilèges — arbre interactif des privilèges avec checkboxes pour créer des rôles

### Tâches backend en cours

**Initialisation de l'authentification — POST /api/auth/initiate**

L'usager envoie son nom d'usager. Le système vérifie que l'usager existe, qu'il est actif et qu'il n'est pas verrouillé. Si valide, un token temporaire JWT est généré avec une expiration de 300 secondes et retourné à l'usager.

**Complétion de l'authentification — POST /api/auth/complete**

L'usager envoie le token temporaire et son mot de passe. Le système vérifie la validité du token et du mot de passe. Si valide, un JWT d'authentification final est retourné avec un refresh token.

---

## Difficultés rencontrées

!!! warning "Difficultés"
    - Clé SSH non reconnue par GitHub lors du test initial — résolu en ajoutant la clé directement sur GitHub
    - Compréhension de la structure du code existant dans le repo Hauméa — nécessite de lire le code avant de commencer à développer

---

## Décisions et ajustements

!!! info "Décisions et ajustements"
    - Décision de concevoir les maquettes dans Figma basées sur le document d'appel d'offres
    - Ajout d'une page de gestion des rôles avec arbre de privilèges interactif — demandée par Marc lors d'une réunion
    - Choix de prioriser les tâches d'authentification 
