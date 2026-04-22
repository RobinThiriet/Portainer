# Configuration fonctionnelle de Portainer

## Objectif

Ce guide explique comment bien paramétrer Portainer après son installation, sans complexifier la stack Docker elle-même. L'idée est de garder un déploiement simple, mais une utilisation propre, lisible et gouvernée.

## Principes à suivre

- utiliser Portainer comme plateforme d'exploitation, pas comme zone de bricolage
- éviter les actions manuelles non tracées
- structurer l'accès par équipes et rôles
- séparer clairement les environnements `dev`, `preprod` et `prod`
- faire des `Stacks` la méthode standard de déploiement

## Paramétrage recommandé après la première connexion

### 1. Créer le compte administrateur initial

Bonnes pratiques:

- utiliser un mot de passe long et unique
- réserver le rôle administrateur global à un nombre très limité de personnes
- éviter d'utiliser ce compte pour les opérations courantes si des rôles moins puissants suffisent

### 2. Déclarer et nommer proprement les environnements

Créer des environnements explicites, par exemple:

- `docker-prod-paris`
- `docker-preprod-paris`
- `docker-dev-local`

Conseils:

- inclure le niveau d'environnement dans le nom
- inclure la localisation ou l'hôte si utile
- éviter les noms trop génériques comme `docker1` ou `serveur-test`

### 3. Créer des groupes d'environnements

Exemple de groupes:

- `production`
- `preproduction`
- `development`

Utilité:

- simplifier la lecture dans l'interface
- préparer une gestion des permissions plus claire
- éviter les erreurs de déploiement sur le mauvais environnement

### 4. Créer les équipes

Exemple d'équipes:

- `admins-platform`
- `ops`
- `developers`
- `auditors`

Recommandation:

- aligner les équipes Portainer avec les responsabilités réelles
- éviter les comptes partagés
- privilégier les permissions par équipe plutôt que par utilisateur

### 5. Associer des rôles adaptés

Approche recommandée:

- `admins-platform`: administration complète
- `ops`: gestion des stacks et supervision sur les environnements autorisés
- `developers`: accès limité à leurs environnements ou ressources
- `auditors`: consultation uniquement si nécessaire

Le but est de limiter les droits globaux et de donner le minimum nécessaire.

### 6. Standardiser les stacks

Utiliser les `Stacks` comme mode de déploiement principal.

À éviter:

- créer des conteneurs manuellement dans l'interface sans source versionnée
- multiplier les stacks "temporaires" qui restent en place
- modifier en urgence une stack sans reporter ensuite le changement dans Git

À faire:

- une stack par application ou domaine fonctionnel
- des noms explicites et stables
- des fichiers Compose versionnés dans Git
- une séparation claire entre applicatif, base de données, proxy et observabilité

### 7. Ajouter des tags cohérents

Exemples de tags utiles:

- `prod`
- `preprod`
- `dev`
- `critical`
- `customer-facing`
- `internal`

Les tags permettent de filtrer plus vite les environnements et d'améliorer la lisibilité globale.

## Structure cible recommandée

```text
Portainer
├── Groupes d'environnements
│   ├── production
│   ├── preproduction
│   └── development
├── Equipes
│   ├── admins-platform
│   ├── ops
│   ├── developers
│   └── auditors
└── Stacks
    ├── proxy-traefik-prod
    ├── app-billing-prod
    ├── app-billing-preprod
    └── observability-dev
```

## Checklist de mise en route

- créer le compte admin initial
- enregistrer les environnements avec une convention de nommage
- regrouper les environnements par niveau
- créer les équipes
- attribuer les permissions minimales nécessaires
- décider d'une convention de nommage des stacks
- interdire les déploiements manuels non versionnés

## Erreurs fréquentes à éviter

- tout faire avec un seul compte administrateur
- mélanger `dev` et `prod` dans le même groupe logique
- nommer les stacks de façon imprécise
- créer des conteneurs hors `Stacks`
- ne pas documenter qui a le droit de déployer où
