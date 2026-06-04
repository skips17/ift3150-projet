---
title: Suivi du projet
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Suivi de projet

---

## Semaine 1 (18–24 mai 2026)

### Objectifs de la période
- Participer à la réunion d'introduction avec l'équipe FAAQ
- Configurer l'environnement de développement
- Lire et comprendre la documentation du projet

### Travail réalisé

!!! abstract "Avancement"
    - [x] Réunion d'introduction 
    - [x] Lecture du livre blanc et du document d'appel d'offres
    - [x] Lecture du fichier Excel des besoins techniques
    - [x] Installation et configuration de Git
    - [x] Génération d'une clé SSH ed25519 et envoi à Normand Rivard
    - [x] Installation de JetBrains Toolbox, IntelliJ et DataGrip
    - [x] Installation et configuration de MariaDB
    - [x] Création des bases de données haumeadb, ceresdb et chirondb
    - [x] Prise en main de ClickUp et des documents sur Google Drive
    - [x] Compréhension de l'architecture microservices — Hauméa, Cérès, Chiron, MakeMake

### Décisions et ajustements

!!! info "Décisions"
    - Compréhension que Hauméa agit comme gateway central et serveur d'authentification
    - Compréhension que Cérès gère tout ce qui est au niveau de la fédération et des clubs
    - Identification des trois paliers d'accès — Membre, Club, Fédération

### Difficultés rencontrées

!!! warning "Difficultés"
    - Accès au repo Git du projet en attente de confirmation 

---

## Semaine 2 (25–31 mai 2026)

### Objectifs de la période
- Obtenir l'accès au repo Git du projet
- Commencer la conception des maquettes de l'interface web
- Participer aux réunions de l'équipe

### Travail réalisé

!!! abstract "Avancement"
    - [x] Clonage du repo Hauméa via SSH
    - [x] Réunion avec l'équipe: charge de concevoir les maquettes de l'interface web basées sur le document d'appel d'offres
    - [x] Début de la conception des maquettes dans Figma
    - [x] Choix des tâches dans ClickUp — initialisation de l'authentification et complétion de l'authentification
    - [x] Réunion avec l'équipe, discussion sur l'avancement des maquettes
        - Marc a précisé que c'est l'usager qui achète un abonnement pour devenir membre, pas le gestionnaire qui ajoute un usager
        - Marc a demandé de concevoir une page de gestion des rôles avec un arbre de privilèges interactif avec checkboxes
        - Introduction à Bruno, outil de test d'APIs
        - Discussion sur l'architecture, les requêtes passent toujours par UUID pour des raisons de sécurité

### Décisions et ajustements

!!! info "Décisions"
    - Ajout d'une page de gestion des rôles avec arbre de privilèges interactif à la liste des maquettes
    - Choix de commencer par les deux tâches d'authentification 

### Difficultés rencontrées

!!! warning "Difficultés"
    - Comprendre la structure du code existant

---

## Semaine 3 (1–7 juin 2026)

### Objectifs de la période
- Finaliser les maquettes Figma
- Choisir les tâches à réaliser dans ClickUp
- Présenter les maquettes lors de la réunion

### Travail réalisé

!!! abstract "Avancement"
    - [x] Finalisation des maquettes dans Figma: 14 pages principales de la plateforme
    - [x] Analyse du fichier Privileges.java dans le repo Hauméa pour construire l'arbre interactif des privilèges
    - [x] Réunion avec l'équipe
        - Chaque membre a discuté de l'avancement de son travail
        - Marc a présenté le projet MakeMake et les tests avec Cucumber et Gherkin
        - Discussion sur l'organisation des tâches: entités, contrôleur, service, repository, tests unitaires
        - Marc a mentionné l'outil de génération automatique de documentation Swagger
        - Jasmin Robert a envoyé la charte graphique de la FAAQ pour adapter les maquettes au design 
    - [x] Présentation des maquettes lors de la réunion
    - [x] Avancement sur la tâche initialisation de l'authentification — POST /api/auth/initiate

### Décisions et ajustements

!!! info "Décisions"
    - Compréhension du flux complet, l'initialisation génère un token temporaire valide 300 secondes, la complétion valide le mot de passe et retourne un JWT final

### Difficultés rencontrées

!!! warning "Difficultés"
    -  La propriété active sur l'entité User: la tâche demande d'ajouter active: boolean sur l'entité User, il faut s'assurer que Liquibase génère correctement la migration de base de données correspondante.
