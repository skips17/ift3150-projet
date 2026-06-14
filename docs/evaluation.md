---
title: Évaluation & Discussion
---

<style>
    @media screen and (min-width: 76em) {
        .md-sidebar--primary {
            display: none !important;
        }
    }
</style>


# Évaluation
## Maquettes
Les 14 maquettes conçues dans Figma respectent les fonctionnalités attendues selon le document d'appel d'offres. La charte graphique de la FAAQ a été reçue et les maquettes seront mises à jour pour être en accord avec le design général de la plateforme.
## Developement de l'authentification au backend
## Méthodes de validation

Les endpoints développés ont été validés manuellement via Postman en testant le flux complet d’authentification en deux étapes.

**Scénario de test principal :**

1. Appel de POST /auth/initiate avec un nom d’usager valide, vérification que le temporaryToken est retourné avec expiresInSeconds: 300
2. Appel de POST /auth/complete avec le token reçu et le mot de passe, vérification que l’accessToken, le refreshToken et expiresInSeconds: 86400 sont retournés

**Cas d’erreur testés :**

- Usager inexistant : 404 USER_NOT_FOUND
- Token temporaire invalide ou expiré : 401 INVALID_TEMPORARY_TOKEN
- Mot de passe incorrect : 401 INVALID_CREDENTIALS
- Token déjà utilisé : 401 TEMPORARY_TOKEN_ALREADY_USED

---

## Résultats obtenus

### Fonctionnalités validées

- POST /auth/initiate retourne 200 OK avec un token temporaire JWT valide 300 secondes
- POST /auth/complete retourne 200 OK avec un access token JWT, un refresh token et une durée de validité de 86400 secondes
- Le token temporaire est correctement invalidé après usage, une deuxième tentative avec le même token retourne TEMPORARY_TOKEN_ALREADY_USED
- Les cas d’erreur retournent les codes HTTP et messages attendus selon les critères d’acceptation de la tâche ClickUp

### Observations

- L’application démarre correctement et les migrations Liquibase s’exécutent sans erreur au démarrage

---

## Analyse critique

Les deux tâches d’authentification ont été complétées et correspondent aux critères d’acceptation définis dans ClickUp. Le flux en deux étapes, token intermédiaire puis vérification du mot de passe, est fonctionnel et sécurisé car le token temporaire ne peut être utilisé qu’une seule fois.

La principale limite observée est l’absence de tests unitaires automatisés. La validation a été faite uniquement de façon manuelle via Postman. Des tests automatisés avec JUnit ou Cucumber seraient nécessaires pour garantir la non-régression lors des prochaines modifications.

Une autre limite est liée à l’architecture réactive de Spring WebFlux car les associations Hibernate lazy ne peuvent pas être chargées après la fermeture de la session, ce qui a nécessité d’utiliser une requête JPQL directe dans le repository pour charger les privilèges.

---

## Limites du projet

- Pas de tests unitaires automatisés pour le moment, validation manuelle uniquement
- Dépendance au module faaq-security-core qui doit être installé manuellement en local via mvn install
