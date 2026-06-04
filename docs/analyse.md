---
title: Études préliminaires
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>

# Études préliminaires

---

## Compréhension du problème

La FAAQ utilise actuellement plusieurs outils distincts et non harmonisés pour gérer ses membres et ses activités. Ces outils ne sont pas synchronisés entre eux, ce qui entraîne des mises à jour manuelles, des risques d'erreurs et des coûts élevés. La FAAQ souhaite une plateforme unique qui regroupe la gestion des membres, des abonnements, des paiements et des communications, tout en respectant la Loi 25 sur la protection des renseignements personnels au Québec.

---

## Analyse des solutions existantes

Avant de développer une solution à l'interne, la FAAQ a évalué plusieurs plateformes existantes sur le marché.


Yapla :Plateforme québécoise, gestion d'associations, Ne répond pas à tous les besoins spécifiques de la FAAQ
Amilia :Gestion de membres et inscriptions, Coût élevé, fonctionnalités non adaptées 
Spordle :Gestion sportive, Ne répond pas aux besoins de la FAAQ

Le constat est qu'aucune plateforme existante ne répond complètement aux besoins de la FAAQ. Le développement d'une solution à l'interne a donc été retenu.

---

## Contraintes et besoins

### Contraintes techniques
- La plateforme doit être en français
- Elle doit respecter la Loi 25 — loi sur la protection des renseignements personnels dans le secteur privé (RLRQ c P-39.1)
- Le paiement en ligne doit être certifié par la norme PCI
- La plateforme doit pouvoir s'intégrer avec d'autres systèmes via une API sécurisée et documentée

### Contraintes organisationnelles
- La plateforme doit être maintenable par des ressources ayant des notions de développement logiciel
- Elle doit pouvoir être mise à jour et évoluée sans dépendre d'un fournisseur tiers

### Besoins fonctionnels prioritaires
- Gestion des abonnements multiclubs
- Traitement de paiement en ligne, PayPal, Stripe, Square
- Rappels automatisés d'expiration d'abonnement
- Trois paliers de gestion,Membre, Club, Fédération
- Rapports configurables sur les membres
- Dons croisés lors des transactions

---

## Explorations techniques


Le projet est développé avec Spring Boot et Java. Plusieurs éléments importants ont été identifiés :
L'architecture suit le pattern Controller—Service—Repository

### Analyse des tâches ClickUp
Les tâches sont décrites sous forme de stories avec critères d'acceptation précis, endpoint, requête attendue, validations, traitement et erreurs possibles. Chaque tâche contient suffisamment d'information pour être développée de façon autonome.

### Analyse des maquettes
14 maquettes ont été conçues par moi dans Figma pour couvrir les principales pages de la plateforme, basées sur le document d'appel d'offres et les discussions avec l'équipe.

---

## Choix retenus

### Architecture microservices
Le système est découpé en quatre services indépendants — Hauméa, Cérès, Chiron et MakeMake. Chacun avec sa propre base de données et sa propre responsabilité. Ce choix permet des mises à jour rapides, renforce la sécurité et permet d'utiliser la technologie la plus pertinente pour chaque sous-système.

### Technologies backend
Spring Boot avec Java a été retenu pour le backend, c'est une technologie largement connue dans le milieu, ce qui facilite la maintenabilité par des ressources futures.

### Base de données
MariaDB a été retenu avec Liquibase pour la gestion des migrations de base de données.

### Tests
Cucumber et Gherkin ont été retenus pour les tests d'intégration, ils permettent à n'importe qui d'écrire des scénarios de tests en langage naturel. Bruno a été retenu pour les tests manuels d'APIs.

### Approche Agile
Une stratégie Agile a été retenue, livraisons régulières, tests constants et rétroaction.

---

## Références

- Livre blanc :Plateforme de gestion des membres FAAQ
- Document d'appel d'offres FAAQ
- Fichier Excel des besoins techniques 
- Repo Git Hauméa — org.faaq
- Tâches ClickUp, stories avec critères d'acceptation
