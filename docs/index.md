---
title: Vue d'ensemble du projet
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Vue d'ensemble du projet

!!! info "Informations générales"
    **Session**: Été 2026  
    **Auteur(s)**: Yassine Azmani (20267812)
    **Thème(s)**: Développement web, architecture microservices, gestion de membres  
    **Superviseur(s)**: Louis-Edouard Lafontant (DIRO, Université de Montréal)  
    **Collaborateur(s):** FAAQ — Jasmin Robert (Directeur général), Normand Rivard (accès au code), Marc St-Pierre (suivi et tâches)

## Description du projet

### Contexte

La Fédération des astronomes amateurs du Québec (FAAQ) compte 28 clubs membres répartis dans 15 régions du Québec. Le nombre d'astronomes amateurs, membres des clubs ou membres individuels de la FAAQ, s'élève à environ 2000. 

Depuis quelques années, la FAAQ est à la recherche d'une plateforme moderne pour combler ses besoins en ce qui concerne la gestion de membres. Elle souhaite remplacer les différentes plateformes de service utilisées actuellement qui ne sont pas harmonisées, et dont certaines sont inefficaces et n'offrent pas un service à la clientèle jugé adéquat.

### Problématique

La FAAQ utilise actuellement plusieurs outils pour gérer ses activités : WordPress pour le site web, ARMember pour l'accès à l'espace membre, MailChimp pour les communications, et PayPal ainsi que d'autres plateformes pour les paiements.

Ces outils causent plusieurs problèmes :

- Les plateformes ne sont pas synchronisées entre elles — la mise à jour de la base de données d'ARMember doit être faite manuellement
- Le système d'adhésion seul coûte 77,25$/mois, auquel s'ajoutent des frais de modifications lors des mises à jour
- Les mises à jour ne sont effectuées que lorsqu'elles sont critiques
- Le service à la clientèle des fournisseurs actuels est problématique — plusieurs relances et suivis doivent être effectués lorsqu'il y a des problèmes
- La base de données des membres n'est pas sous le contrôle de la FAAQ

### Proposition et objectifs

Le projet consiste à développer une plateforme web de gestion des membres qui regroupe tous les services actuellement dispersés. La plateforme doit être en français, sécuritaire, fiable, intuitive et accessible. Elle doit agir en tant que base de données primaire d'utilisateurs pour les autres plateformes utilisées, et respecter les lois en vigueur au Québec, dont la loi sur la protection des renseignements personnels dans le secteur privé.

Chaque club, ainsi que la FAAQ, doit être autonome à son niveau, par la mise en place de trois paliers de gestion :

- **Palier Membre** — permet à ce dernier de s'abonner, lui ou sa famille, aux clubs de son choix et de modifier ses informations personnelles au besoin
- **Palier Club** — permet d'abonner n'importe quel membre déjà existant et sa famille, de créer de nouveaux membres, de modifier les tarifs d'abonnement, d'envoyer des rappels automatisés pour les membres dont l'abonnement vient à échéance, et de créer des rapports sur les membres
- **Palier Fédération** — a le même rôle que le palier Club, en permettant en plus d'abonner des membres à n'importe quel club et de pouvoir créer de nouveaux clubs

### Méthodologie

Le projet suit une stratégie purement Agile. Les points importants de cette démarche sont la livraison régulière de fonctionnalités afin qu'elles soient testées rapidement, les tests constants par tous les intervenants, et la rétroaction rapide entre les développeurs et les représentants des usagers. Les tâches sont gérées dans ClickUp sous forme de stories avec critères d'acceptation.

### Validation et Évaluation

La validation se fait à plusieurs niveaux:tests unitaires pour chaque composante du backend, tests d'intégration via MakeMake avec des scénarios Gherkin et Cucumber, et tests manuels via Bruno pour valider les APIs.

## Équipe

| Membre | Rôle |
|--------|------|
| Yassine Azmani (20267812) | APIs d'authentification et maquettes de l'interface web |
| Gabriel Viana | API de création de club |

## Échéancier

!!! info
    Le suivi complet est disponible dans la page [Suivi de projet](suivi.md).

| Tâche | Assigné | Statut |
|-------|---------|--------|
| Créer serveur testadhesion.faaq.org | Normand Rivard | 🔄 En cours |
| Cérès : installer le système de mise-à-jour de BD | — | ⏳ À faire |
| Implémenter la stratégie de tests automatisés | — | ⏳ À faire |
| Ajout d'usager dans un compte par le gestionnaire | — | ⏳ À faire |
| Initialisation de l'authentification | Yassine Azmani | 🔄 En cours |
| Compléter l'authentification | Yassine Azmani | 🔄 En cours |
| API de création de club | Gabriel Viana | 🔄 En cours |
| Création d'une notification | — | ⏳ À faire |
| Consultation des notifications | — | ⏳ À faire |
| Marquer une notification comme lue | — | ⏳ À faire |
