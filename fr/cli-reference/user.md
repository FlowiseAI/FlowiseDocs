# Utilisateur

## Lister les e-mails des utilisateurs

Cette commande vous permet de lister tous les e-mails des utilisateurs enregistrés dans le système.

### Utilisation locale

```bash
pnpm user
```

Ou si vous utilisez npm

```bash
npx flowise user
```

### Utilisation de Docker

Si vous exécutez Flowise dans un conteneur Docker, utilisez la commande suivante :

```bash
docker exec -it FLOWISE_CONTAINER_NAME pnpm user
```

Replace `FLOWISE_CONTAINER_NAME` par le nom réel de votre conteneur Flowise.

## Réinitialiser le mot de passe de l'utilisateur

Cette commande vous permet de réinitialiser le mot de passe d'un utilisateur.

### Utilisation locale

```bash
pnpm user --email "admin@admin.com" --password "myPassword1!"
```

Ou si vous utilisez npm

```
npx flowise user --email "admin@admin.com" --password "myPassword1!"
```

### Utilisation de Docker

Si vous exécutez Flowise dans un conteneur Docker, utilisez la commande suivante :

```bash
docker exec -it FLOWISE_CONTAINER_NAME pnpm user --email "admin@admin.com" --password "myPassword1!"
```

Replacez `FLOWISE_CONTAINER_NAME` par le nom de votre conteneur Flowise.

### Paramètres

* `--email` : L'adresse e-mail de l'utilisateur dont vous souhaitez réinitialiser le mot de passe
* `--password` : Le nouveau mot de passe à définir pour l'utilisateur
