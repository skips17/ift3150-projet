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

## Semaine 1 

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

## Semaine 2

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

## Semaine 3 

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

---

## Semaine 4

### Objectifs de la période
- Compléter l'implémentation de POST /auth/initiate
- Implémenter POST /auth/complete
- Tester les deux endpoints localement

### Travail réalisé

!!! abstract "Avancement"
    - [x] Ajout des champs active, locked et suspended sur l'entité User
    - [x] Création de la migration Liquibase 002-user-status.yaml pour ajouter les colonnes sur la table user avec valeurs par défaut
    - [x] Implémentation complète de POST /auth/initiate
        - Vérification de l'existence de l'usager (404 si introuvable)
        - Vérification du statut: actif, non verrouillé, non suspendu
        - Génération d'un token intermédiaire JWT valide 300 secondes
        - Retour du temporaryToken et de expiresInSeconds: 300
    - [x] Création de l'entité UsedToken et du repository associé pour empêcher la réutilisation d'un token temporaire
    - [x] Création de la migration Liquibase 003-used-tokens.yaml pour créer la table used_tokens avec les colonnes id, token et used_at
    - [x] Implémentation complète de POST /auth/complete
        - Validation du token temporaire (format et expiration)
        - Vérification que le token n'a pas déjà été utilisé
        - Vérification du mot de passe via BCrypt
        - Invalidation du token après usage
        - Récupération des privilèges de l'usager via requête JPQL
        - Génération d'un access token JWT et d'un refresh token
        - Retour de l'accessToken, du refreshToken et de expiresInSeconds: 86400
    - [x] Configuration de l'environnement local avec l'ajout du module faaq-security-core et configuration de la base de données MariaDB
    - [x] Tests manuels avec Postman, les deux endpoints retournent les réponses attendues (200 OK)

### Décisions et ajustements

!!! info "Décisions"
    - Utilisation d'une requête JPQL directe (getUserPrivileges) pour charger les privilèges car chaque appel à un repository ouvre sa propre session, ce qui évite les erreurs de session fermée liées à Spring WebFlux
    - Ajout de @ToString.Exclude sur les collections de l'entité User pour éviter les erreurs lors du logging

### Difficultés rencontrées

!!! warning "Difficultés"
    - Le module faaq-security-core n'était pas installé localement, résolu en clonant le repo et en exécutant mvn install
    - Plusieurs erreurs de LazyInitializationException dues à l'architecture réactive de Spring WebFlux qui ferme la session Hibernate avant le chargement des associations lazy, résolu en remplaçant l'accès via les objets Java par une requête JPQL directe dans le repository
    - La fonction UUID_TO_BIN n'est pas disponible dans la version de MariaDB utilisée, contourné avec UNHEX(REPLACE(UUID(), '-', ''))

---

## Semaine 5

### Objectifs de la période
- Mettre à jour les maquettes avec la charte graphique officielle de la FAAQ
- Se documenter sur l'intégration des paiements avec Moneris
- Produire un diagramme du modèle de données du microservice Hauméa

### Travail réalisé

!!! abstract "Avancement"
    - [x] Réception de la charte graphique officielle de la FAAQ envoyée par Jasmin Robert
    - [x] Mise à jour des maquettes HTML avec les couleurs, polices et logo officiels de la FAAQ
        - Couleur principale bleu foncé et couleur d'accent mauve
        - Police Poppins pour le texte et Rubik Mono One pour le logo
        - Intégration du logo officiel FAAQ dans l'en tête de chaque page
        - Boutons arrondis, type pill shape, cohérents avec le style de la charte
    - [x] Pages mises à jour: inscription, connexion, mot de passe oublié, tableau de bord membre, paiement, carte de membre, tableau de bord club, tableau de bord fédération
    - [x] Ajout d'une option de connexion via Google et Facebook sur la page de connexion, à valider avec Marc si ça fait partie de la portée du projet
    - [x] Documentation sur l'intégration des paiements avec Moneris, utilisation d'un iframe fourni par Moneris pour la saisie de la carte de crédit et génération d'un jeton transmis à Chiron
    - [x] Génération d'un diagramme UML du modèle de données du microservice Hauméa avec l'outil Diagrams d'IntelliJ

### Décisions et ajustements

!!! info "Décisions"
    - L'option de connexion via Google et Facebook nécessiterait l'implémentation d'OAuth côté Hauméa, à valider avec Marc avant de l'ajouter à la portée du projet
    - Utilisation de l'outil Diagrams d'IntelliJ pour générer automatiquement le diagramme du modèle de données plutôt que de le dessiner manuellement

### Difficultés rencontrées

!!! warning "Difficultés"
    - Configuration des catégories d'affichage dans IntelliJ pour que le diagramme montre les champs des classes plutôt qu'une vue simplifiée par paquetage

---

## Semaine 6

### Objectifs de la période
- Terminer la mise à jour des maquettes pour toutes les pages
- Participer à la réunion d'équipe hebdomadaire
- Entamer la conception du dashboard configurable par l'usager

### Travail réalisé

!!! abstract "Avancement"
    - [x] Finalisation de la mise à jour des maquettes pour toutes les pages
    - [x] Réunion d'équipe
        - Présentation par Marc du concept de dashboard configurable par l'usager: cartes réordonnables selon les rôles et priorités, taille d'affichage (petit/moyen/grand), sauvegardé côté serveur
        - Annonce de Marc: création de trois nouveaux dépôts Git: MakeMake, Chiron, et un dépôt commun pour les requêtes Bruno

### Décisions et ajustements

!!! info "Décisions"
    - Intégration Moneris en attente: accès au compte de test, aux dépôts et aux informations nécessaires doivent être fournis par Marc avant de pouvoir commencer

---

## Semaine 7

### Objectifs de la période
- Présenter l'avancement du projet
- Commencer l'implémentation du frontend

### Travail réalisé

!!! abstract "Avancement"
    - [x] Réunion et discussion avec Jasmin Robert sur l'avancement du projet
    - [x] Présentation de l'avancement du projet à Louis
    - [x] Début de l'implémentation du frontend
        - Création de la page de connexion (composant React avec gestion des deux étapes d'authentification)
        - Création de la page du tableau de bord membre

### Décisions et ajustements

!!! info "Décisions"
    - Démarrage du frontend en React avec TypeScript et Vite, en cohérence avec la charte graphique FAAQ déjà établie

## Semaine 8

### Objectifs de la période
- Commencer les tâches backend dans Cérès
- Commencer l'intégration frontend React dans WordPress
- Ajuster les maquettes suite aux commentaires de Jasmin

### Travail réalisé

!!! abstract "Avancement"
    - [x] Initialisation du projet frontend React avec TypeScript et Vite, création du composant de connexion branché sur les deux étapes d'authentification Hauméa
    - [x] Réunion avec Marc
        - Discussion sur l'architecture frontend : WordPress utilisé comme vitrine publique, application React chargée dans un iframe pour l'espace membre
        - Attribution des tâches backend dans Cérès (compléter les entités existantes et ajouter les nouvelles énumérations)
        - Discussion sur l'utilisation de HSQL comme base de données en mémoire pour les tests unitaires
    - [x] Recherche et documentation de la méthode d'intégration d'une application React dans WordPress via iframe
    - [x] Ajustements des maquettes suite aux commentaires de Jasmin : indication du délai d'expiration dans le tableau de bord club, clarification du club concerné dans la page de gestion des tarifs, ajout du type d'abonnement dans la page de paiement, ajout de l'option de don à 25$ avec la note sur le Programme Placements Sports et Loisirs, et ajout des logos des cartes de crédit et du virement Interac

### Décisions et ajustements

!!! info "Décisions"
    - Ne pas commencer le développement frontend complet avant confirmation de Marc et Jasmin sur l'intégration WordPress/React

---

## Semaine 9

### Objectifs de la période
- Compléter les tâches backend assignées dans Cérès
- Mettre en place la structure du projet frontend React

### Travail réalisé

!!! abstract "Avancement"
    - [x] Réunion avec Marc
        - Discussion sur le projet MakeMake et l'intégration des tests avec Cucumber et Gherkin : Marc a expliqué que les tests d'intégration seront écrits en dehors des microservices individuels et qu'il expliquera comment contribuer à ce projet
        - Marc a confirmé que la priorité côté frontend est de d'abord établir une structure de projet claire et des conventions avant de développer les pages
    - [x] Complétion des tâches backend dans Cérès
        - Tâche 2.1 : ajout du champ `renewalReminderDaysBefore` dans l'entité `MembershipPlan`
        - Tâche 2.2 : complétion de l'énumération `AudienceType` avec les valeurs MEMBER, NON_MEMBER, YOUTH, ADULT, FAMILY, SENIOR
        - Tâche 3.1 : création de l'entité `MembershipPlanAudience` avec UUID, clé étrangère vers `MembershipPlan`, type d'audience, prix, et contrainte d'unicité sur la combinaison plan/type
        - Tâche 3.2 : création du repository `MembershipPlanAudienceRepository` avec les méthodes `findByMembershipPlanId` et `findByMembershipPlanIdAndAudienceType`
        - Tâche 6.1 : création de l'entité `AuditLog` et du repository `AuditLogRepository`
        - Tâche 6.2 : création du service `AuditService` pour journaliser les actions (log, getLogsForEntity, getLogsByUser)
        - Tâche 6.3 : création du listener JPA `AuditEntityListener` avec `@PostPersist` et `@PostUpdate` pour journaliser automatiquement les créations et modifications d'entités
        - Correction d'un bug préexistant dans `FederationMapper` : méthodes de mapping manquantes pour `PhoneNumber` et type incorrect dans `PhoneNumberDto`
    - [x] Mise en place de la structure du projet frontend React
        - Organisation des dossiers : `components/`, `pages/`, `services/`, `hooks/`, `types/`
        - Création du service d'authentification `authService.ts` pour centraliser les appels à Hauméa et la gestion des tokens
        - Mise en place du routage avec `react-router-dom`, séparation des routes publiques et des routes protégées
        - Création d'un composant `ProtectedRoute` pour rediriger vers la page de connexion si l'utilisateur n'est pas authentifié
        - Avancement sur la page tableau de bord membre, affichage des informations de l'abonnement en cours

### Décisions et ajustements

!!! info "Décisions"
    - Convenu avec Marc d'établir des conventions de nommage et une structure de dossiers claire dès le départ pour faciliter les contributions futures
    - Les tests d'intégration MakeMake seront abordés dans une prochaine réunion avec Marc
