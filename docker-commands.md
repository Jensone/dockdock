# Commandes de base Docker

Voici quelques commandes de base pour travailler avec Docker dans un terminal.

## Lister les images Docker

```bash
docker images
```

Cette commande liste toutes les images Docker actuellement disponibles sur votre machine.

## Lister les conteneurs Docker

```bash
docker ps
```

Cette commande liste tous les conteneurs Docker actuellement en cours d'exécution sur votre machine.

## Rechercher une image Docker

```bash
docker search <terme_recherche>
```

Cette commande recherche une image Docker sur un registre public en fonction du terme de recherche spécifié. Par exemple, pour rechercher une image `nginx`, vous pouvez exécuter `docker search nginx`. Il existe 2 types de registres Docker : public et privé. Le registre public est [Docker Hub](https://hub.docker.com/).

## Télécharger une image Docker

```bash
docker pull <nom_image>
```

Cette commande télécharge une image Docker depuis un registre public ou privé. Par exemple, pour télécharger l'image `ngnix`, vous pouvez exécuter `docker pull nginx`.

## Exécuter un conteneur Docker

```bash
docker run <nom_image> -it
```

Cette commande exécute un conteneur Docker à partir de l'image spécifiée. Par exemple, pour exécuter un conteneur `nginx`, vous pouvez exécuter `docker run nginx`. Le drapeau `-it` permet d'ouvrir une session interactive avec le conteneur, ce qui peut être utile pour le débogage ou l'exploration.

Vous pouvez également spécifier des options supplémentaires pour personnaliser le comportement du conteneur :

- `-d` : Exécute le conteneur en arrière-plan (mode détaché).
- `-p` : Mappe un port du conteneur sur un port de l'hôte.

## Arrêter un conteneur Docker

```bash
docker stop <id_conteneur>
```

Cette commande arrête un conteneur Docker en cours d'exécution. Vous pouvez obtenir l'ID du conteneur en utilisant la commande `docker ps`.

## Supprimer un conteneur Docker

```bash
docker rm <id_conteneur>
```

Cette commande supprime un conteneur Docker. Vous pouvez obtenir l'ID du conteneur en utilisant la commande `docker ps`.

## Supprimer une image Docker

```bash
docker rmi <nom_image>
```

Cette commande supprime une image Docker de votre machine. Vous pouvez obtenir le nom de l'image en utilisant la commande `docker images`.

## Exécuter une commande dans un conteneur Docker

```bash
docker exec <id_conteneur> <commande>
```

Cette commande exécute une commande spécifique dans un conteneur Docker en cours d'exécution. Vous pouvez obtenir l'ID du conteneur en utilisant la commande `docker ps`. Par exemple, pour exécuter une commande `ls` dans un conteneur, vous pouvez exécuter `docker exec <id_conteneur> ls`.

## Supprimer toutes les données Docker

```bash
docker system prune
```

Cette commande supprime tous les conteneurs, images et réseaux Docker inutilisés. Cela peut être utile pour nettoyer votre système et libérer de l'espace disque.

---

Ces commandes de base devraient vous aider à démarrer avec Docker et à explorer ses fonctionnalités. Pour plus d'informations, consultez la [documentation officielle de Docker](https://docs.docker.com/). 
