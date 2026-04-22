# Portainer Stack

Déploiement Docker Compose de Portainer CE avec une configuration plus propre pour la production légère:

- paramètres externalisés via `.env`
- réseau et volume nommés
- redémarrage automatique
- rotation des logs Docker
- durcissement minimal avec `no-new-privileges`
- documentation d'exploitation et schéma d'architecture

## Démarrage rapide

1. Copier le fichier d'exemple:

```bash
cp .env.example .env
```

2. Démarrer la stack:

```bash
docker compose up -d
```

3. Ouvrir Portainer:

```text
https://<IP_DU_SERVEUR>:9443
```

## Fichiers importants

- [`docker-compose.yaml`](./docker-compose.yaml): stack Portainer
- [`.env.example`](./.env.example): variables d'environnement disponibles
- [`docs/ARCHITECTURE.md`](./docs/ARCHITECTURE.md): schéma et description de l'architecture
- [`docs/OPERATIONS.md`](./docs/OPERATIONS.md): exploitation, sauvegarde, mises à jour

## Axes d'amélioration recommandés

- placer Portainer derrière un reverse proxy avec nom de domaine et certificats TLS publics
- restreindre l'accès réseau à `9443` par firewall ou VPN
- sauvegarder régulièrement le volume `portainer_data`
- superviser l'hôte Docker avec des alertes de disponibilité et d'espace disque
- utiliser des secrets et une gestion d'accès stricte pour les comptes administrateurs

## Commandes utiles

Afficher les logs:

```bash
docker compose logs -f portainer
```

Mettre à jour l'image:

```bash
docker compose pull
docker compose up -d
```

Sauvegarder les données:

```bash
mkdir -p backups
docker run --rm \
  -v portainer_data:/source:ro \
  -v "$(pwd)/backups:/backup" \
  alpine \
  sh -c 'tar czf /backup/portainer_data_$(date +%F_%H%M%S).tar.gz -C /source .'
```
