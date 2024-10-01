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
