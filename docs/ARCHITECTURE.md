# Architecture

## Vue d'ensemble

Cette stack expose Portainer en HTTPS sur le port `9443` et conserve ses données dans un volume Docker dédié. Portainer communique avec le moteur Docker local via le socket Unix monté dans le conteneur.

## Schéma

```mermaid
flowchart TD
    U[Administrateur] -->|HTTPS 9443| P[Conteneur Portainer]
    P -->|Docker socket| D[Moteur Docker local]
    P -->|Lecture/Ecriture| V[(Volume portainer_data)]
    D --> C[Conteneurs geres]
    P --> N[Reseau docker portainer_net]
```

## Composants

- `Administrateur`: accède à l'interface web Portainer
- `Conteneur Portainer`: interface d'administration et API
- `Moteur Docker local`: cible administrée par Portainer
- `Volume portainer_data`: persistance des utilisateurs, endpoints et configuration
- `Réseau portainer_net`: réseau logique dédié à la stack

## Flux principaux

1. L'administrateur se connecte à l'interface Portainer via HTTPS.
2. Portainer lit et pilote le moteur Docker via `/var/run/docker.sock`.
3. Les données de configuration sont stockées dans `portainer_data`.

## Limites actuelles

- exposition directe de Portainer sur le port `9443`
- dépendance au socket Docker local, très puissant
- absence de supervision et de sauvegarde automatisée dans cette version
