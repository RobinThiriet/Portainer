# Portainer Stack

Déploiement Docker Compose de Portainer CE avec une configuration volontairement simple, accompagné d'une documentation pour bien structurer et administrer Portainer après installation.

- stack légère et lisible
- paramètres externalisés via `.env`
- documentation d'exploitation
- guide de configuration fonctionnelle de Portainer
- schéma d'architecture et bonnes pratiques d'usage

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
- [`docs/PORTAINER-SETUP.md`](./docs/PORTAINER-SETUP.md): comment bien configurer Portainer après installation
- [`docs/PORTAINER-GOVERNANCE.md`](./docs/PORTAINER-GOVERNANCE.md): gouvernance, équipes, rôles et conventions

## Philosophie du dépôt

L'application Portainer reste ici volontairement simple à déployer. La valeur du dépôt est surtout dans la documentation qui explique comment:

- structurer les environnements Portainer
- organiser les équipes et permissions
- versionner les stacks proprement
- éviter que Portainer devienne une interface d'administration désordonnée

## Parcours recommandé

1. Déployer la stack avec `docker compose up -d`
2. Lire [`docs/PORTAINER-SETUP.md`](./docs/PORTAINER-SETUP.md) pour la mise en route fonctionnelle
3. Appliquer les conventions de [`docs/PORTAINER-GOVERNANCE.md`](./docs/PORTAINER-GOVERNANCE.md)
4. Utiliser [`docs/OPERATIONS.md`](./docs/OPERATIONS.md) pour l'exploitation courante

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
