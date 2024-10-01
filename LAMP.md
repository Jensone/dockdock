# Introduction à Docker avec un conteneur LAMP

## 1. Introduction à Docker

Docker est une plateforme open-source qui permet de créer, déployer et exécuter des applications dans des conteneurs. Les conteneurs sont des environnements légers et portables qui contiennent tout ce dont une application a besoin pour s'exécuter.

Avantages de Docker :
- Portabilité
- Isolation
- Efficacité
- Scalabilité

## 2. Concepts clés de Docker

- **Image** : Un modèle en lecture seule utilisé pour créer des conteneurs.
- **Conteneur** : Une instance exécutable d'une image.
- **Dockerfile** : Un fichier texte contenant les instructions pour construire une image Docker.
- **Docker Compose** : Un outil pour définir et exécuter des applications Docker multi-conteneurs.

## 3. Installation de Docker

Suivez les instructions sur le site officiel de Docker pour installer Docker sur votre système d'exploitation :
[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

## 4. Création d'un conteneur LAMP

Nous allons créer un conteneur LAMP (Linux, Apache, MariaDB, PHP 8.3) avec Adminer pour la gestion de la base de données.

### 4.1 Structure du projet

Créez un nouveau dossier pour votre projet et la structure suivante :

```
projet-lamp/
│
├── docker-compose.yml
├── Dockerfile
└── www/
    └── index.php
```

### 4.2 Contenu des fichiers

#### Dockerfile

```Dockerfile
FROM php:8.3-apache

# Installation des dépendances
RUN apt-get update && apt-get install -y \
    libzip-dev \
    && docker-php-ext-install zip pdo pdo_mysql

# Activation du module rewrite d'Apache
RUN a2enmod rewrite

# Installation de Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Nettoyage
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html
```

#### docker-compose.yml

```yaml
version: '3'

services:
  web:
    build: .
    ports:
      - "80:80"
    volumes:
      - ./www:/var/www/html
    depends_on:
      - db

  db:
    image: mariadb:latest
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: lamp_db
      MYSQL_USER: lamp_user
      MYSQL_PASSWORD: lamp_password
    volumes:
      - db_data:/var/lib/mysql

  adminer:
    image: adminer
    ports:
      - "8080:8080"
    depends_on:
      - db

volumes:
  db_data:
```

#### www/index.php

```php
<?php
phpinfo();
```

## 5. Lancement du conteneur

1. Ouvrez un terminal et naviguez jusqu'au dossier de votre projet.
2. Exécutez la commande suivante pour construire et démarrer les conteneurs :

```
docker-compose up -d
```

ou

```
docker compose up -d
```

## 6. Utilisation du conteneur

- Accédez à votre application PHP : [http://localhost](http://localhost)
- Accédez à Adminer : [http://localhost:8080](http://localhost:8080)
  - Système : MySQL
  - Serveur : db
  - Utilisateur : lamp_user
  - Mot de passe : lamp_password
  - Base de données : lamp_db

## 7. Arrêt du conteneur

Pour arrêter les conteneurs, exécutez la commande suivante dans le dossier de votre projet :

```
docker-compose down
```

## Récap

Vous avez maintenant un environnement LAMP fonctionnel avec Docker. Vous pouvez commencer à développer votre application PHP en modifiant les fichiers dans le dossier `www/`. N'oubliez pas de redémarrer les conteneurs si vous apportez des modifications au `Dockerfile` ou au `docker-compose.yml`.

---

# Procédure de push d'un conteneur Docker

## Prérequis
- Docker installé sur votre machine
- Un compte sur un registre Docker (par exemple, Docker Hub)
- Votre image Docker déjà créée

## Étapes

1. **Connectez-vous à votre registre Docker**
   ```
   docker login
   ```
   Entrez votre nom d'utilisateur et votre mot de passe lorsque vous y êtes invité.

2. **Taguez votre image**
   ```
   docker tag image_locale:tag nomutilisateur/nom_repo:tag
   ```
   Exemple :
   ```
   docker tag mon_app:v1 johndoe/mon_app:v1
   ```

3. **Poussez l'image vers le registre**
   ```
   docker push nomutilisateur/nom_repo:tag
   ```
   Exemple :
   ```
   docker push johndoe/mon_app:v1
   ```

4. **Vérifiez que l'image a été poussée avec succès**
   Allez sur votre compte Docker Hub (ou autre registre) et vérifiez que l'image apparaît dans votre liste de dépôts.

## Notes importantes

- Assurez-vous que le nom de votre image respecte la convention : `nomutilisateur/nom_repo:tag`
- Si vous utilisez un registre privé, vous devrez peut-être spécifier l'URL complète du registre dans la commande de tag et de push.
- Pour les registres privés, la commande de connexion peut être différente. Consultez la documentation de votre registre pour plus de détails.

## Exemple complet

```bash
# Connectez-vous à Docker Hub
docker login

# Taguez l'image locale
docker tag mon_app:v1 johndoe/mon_app:v1

# Poussez l'image
docker push johndoe/mon_app:v1

```

## Dépannage en pépins

- Si vous rencontrez des erreurs d'authentification, assurez-vous que vous êtes bien connecté et que vous avez les permissions nécessaires pour pousser vers le dépôt.
- Si le push est lent, vérifiez votre connexion internet et la taille de votre image.
- En cas d'erreur "denied: requested access to the resource is denied", vérifiez que vous avez les droits nécessaires sur le dépôt et que le nom de l'image est correct.

