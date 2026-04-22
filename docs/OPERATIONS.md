# Exploitation

## Préparation

Créer le fichier `.env` à partir du modèle:

```bash
cp .env.example .env
```

Adapter si besoin:

- `TZ`: fuseau horaire du serveur
- `PORTAINER_HTTPS_PORT`: port d'exposition HTTPS
- `PORTAINER_TAG`: version de l'image Portainer

## Lancement

```bash
docker compose up -d
```

Vérifier l'état:

```bash
docker compose ps
docker compose logs --tail=100 portainer
```

## Mise à jour

1. Vérifier et ajuster `PORTAINER_TAG` dans `.env`.
2. Télécharger la nouvelle image:

```bash
docker compose pull
```

3. Redéployer:

```bash
docker compose up -d
```

## Sauvegarde

Exemple de sauvegarde du volume:

```bash
mkdir -p backups
docker run --rm \
  -v portainer_data:/source:ro \
  -v "$(pwd)/backups:/backup" \
  alpine \
  sh -c 'tar czf /backup/portainer_data_$(date +%F_%H%M%S).tar.gz -C /source .'
```

## Restauration

1. Arrêter Portainer:

```bash
docker compose down
```

2. Restaurer une archive dans le volume:

```bash
docker run --rm \
  -v portainer_data:/target \
  -v "$(pwd)/backups:/backup" \
  alpine \
  sh -c 'rm -rf /target/* && tar xzf /backup/<archive>.tar.gz -C /target'
```

3. Redémarrer:

```bash
docker compose up -d
```

## Recommandations sécurité

- restreindre le port `9443` au réseau d'administration
- protéger l'hôte avec un pare-feu
- mettre Portainer derrière un reverse proxy si vous utilisez un domaine public
- réaliser des sauvegardes récurrentes du volume `portainer_data`
- limiter le nombre de comptes administrateurs et activer des mots de passe robustes
