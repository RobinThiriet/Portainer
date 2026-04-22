# Checklist de mise en production Portainer

## Objectif

Cette checklist sert de version courte et actionnable pour mettre une instance Portainer en service proprement.

## Avant ouverture

- déployer Portainer avec `docker compose up -d`
- vérifier que l'interface répond sur `https://<IP>:9443`
- créer le compte administrateur initial avec un mot de passe fort
- confirmer que le volume `portainer_data` est bien présent

## Configuration initiale

- créer les groupes d'environnements: `production`, `preproduction`, `development`
- enregistrer les environnements avec un nom clair
- appliquer une convention de nommage pour les stacks
- créer les équipes nécessaires
- attribuer les droits minimaux nécessaires

## Règles d'usage

- déployer les applications via `Stacks`
- versionner les fichiers Compose dans Git
- éviter les créations manuelles non tracées dans l'interface
- limiter les comptes administrateurs globaux
- identifier un propriétaire pour chaque stack importante

## Vérifications avant exploitation

- les environnements `prod`, `preprod` et `dev` sont clairement séparés
- les équipes voient uniquement leur périmètre utile
- les stacks ont des noms explicites
- aucune stack de test ambiguë n'est conservée
- les changements urgents faits dans Portainer sont reportés dans Git

## Routine minimale

Chaque semaine:

- vérifier l'état des stacks
- relire les logs des services critiques
- supprimer les ressources de test inutiles

Chaque mois:

- revoir les comptes administrateurs
- revoir les permissions des équipes
- auditer les stacks orphelines
- confirmer la cohérence du naming

## Docs à lire ensuite

- [`PORTAINER-SETUP.md`](./PORTAINER-SETUP.md)
- [`PORTAINER-GOVERNANCE.md`](./PORTAINER-GOVERNANCE.md)
- [`PORTAINER-PLAYBOOK.md`](./PORTAINER-PLAYBOOK.md)
- [`OPERATIONS.md`](./OPERATIONS.md)
