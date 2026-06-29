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

## Architecture 

### Architecture logicielle

La plateforme est développée selon une architecture microservices. Le système est découpé en quatre services indépendants, chacun avec sa propre base de données :


Hauméa: Point d'entrée principal — authentification, gestion des usagers, gestion des JWT, envoi des courriels
Cérès :Gestion des clubs, membres et abonnements 
Chiron :
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

## Fonctionnalités et composantes réalisées

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

### Mise à jour des maquettes avec la charte graphique de la FAAQ

Les 14 maquettes ont été révisées pour adopter la charte graphique officielle de la FAAQ (couleurs, polices, logo, style des boutons).

## Décisions et ajustements

!!! info "Décisions et ajustements"
    - Décision de concevoir les maquettes dans Figma basées sur le document d'appel d'offres
    - Ajout d'une page de gestion des rôles avec arbre de privilèges interactif demandée par Marc lors d'une réunion
### Tâches backend réalisées

**Initialisation de l'authentification — POST /auth/initiate** ✅

L'usager envoie son nom d'usager. Le système vérifie que l'usager existe, qu'il est actif et qu'il n'est pas verrouillé ou suspendu. Si valide, un token temporaire JWT est généré avec une expiration de 300 secondes et retourné à l'usager.

Fichiers modifiés ou créés :

- User.java : ajout des champs active, locked et suspended
- 002-user-status.yaml : migration Liquibase pour les nouvelles colonnes
- AuthController.java : implémentation de l'endpoint avec gestion des cas d'erreur

Codes de retour : 200 OK avec temporaryToken et expiresInSeconds: 300, 404 si usager introuvable, 403 si inactif ou suspendu, 423 si verrouillé.

**Complétion de l'authentification — POST /auth/complete** ✅

L'usager envoie le token temporaire et son mot de passe. Le système vérifie la validité du token, qu'il n'a pas déjà été utilisé, et que le mot de passe correspond au hash BCrypt en base de données. Si valide, le token est invalidé, les privilèges sont chargés et un JWT d'accès ainsi qu'un refresh token sont retournés.

Fichiers modifiés ou créés :

- UsedToken.java : entité pour tracer les tokens déjà utilisés
- UsedTokenRepository.java : repository avec méthode existsByToken
- 003-used-tokens.yaml : migration Liquibase pour la table used_tokens
- AuthController.java : implémentation de l'endpoint avec invalidation du token et génération des tokens de session
- UserRepository.java : correction du type du paramètre uuid et utilisation de la requête JPQL getUserPrivileges pour charger les privilèges via une session indépendante
- User.java : ajout de @ToString.Exclude sur les collections pour éviter les erreurs de logging

Codes de retour : 200 OK avec accessToken, refreshToken et expiresInSeconds: 86400, 401 si token invalide, déjà utilisé ou mot de passe incorrect.

**Profil de dashboard configurable** ✅

Mise en place d'un profil permettant à chaque usager de personnaliser l'affichage de son tableau de bord (ordre des cartes, taille d'affichage). Le profil est sauvegardé côté serveur et un profil par défaut est utilisé tant que l'usager n'a rien personnalisé.



## Difficultés rencontrées

!!! warning "Difficultés"
    - Le module faaq-security-core n'était pas installé localement, résolu en clonant le repo et en exécutant mvn install
    - Plusieurs erreurs de LazyInitializationException dues à Spring WebFlux qui ferme la session Hibernate avant le chargement des données, résolu en remplaçant l'accès via les objets Java par une requête JPQL directe dans le repository
    - Le hash BCrypt du mot de passe admin en base de données ne correspondait à aucun mot de passe connu, résolu en mettant à jour le hash directement via DataGrip
    - La fonction UUID_TO_BIN n'est pas disponible dans la version de MariaDB utilisée, contourné avec UNHEX(REPLACE(UUID(), '-', ''))

---

## Décisions et ajustements

!!! info "Décisions et ajustements"
    - Utilisation d'une requête JPQL directe pour charger les privilèges de l'usager plutôt que de naviguer les associations entre objets Java, car chaque appel au repository ouvre sa propre session et évite les problèmes liés à Spring WebFlux
    - Ajout de @ToString.Exclude sur les collections de l'entité User pour éviter que Lombok essaie d'afficher les rôles dans les logs et déclenche une erreur de session fermée
