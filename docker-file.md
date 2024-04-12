# Créer une image Docker

Afin de créer une image Docker, vous devez d'abord créer un fichier `Dockerfile` qui définit les étapes nécessaires pour construire l'image. Un `Dockerfile` est un fichier texte qui contient une série d'instructions pour construire une image Docker. Voici un exemple de `Dockerfile` simple :

```Dockerfile
# Utilisez une image de base
FROM nginx:latest

# Copiez votre application dans le conteneur
COPY . /usr/share/nginx/html

# Exposez le port 80 pour que le trafic web puisse y accéder
EXPOSE 80
```

Dans cet exemple, nous utilisons l'image de base `nginx:latest` pour construire notre image. Nous copions ensuite le contenu de notre application dans le répertoire `/usr/share/nginx/html` du conteneur. Enfin, nous exposons le port 80 pour que le trafic web puisse y accéder.

## Les instructions Dockerfile

Voici quelques instructions courantes que vous pouvez utiliser dans un `Dockerfile` :

| Instruction | Description |
|-------------|-------------|
| `FROM`      | Spécifie l'image de base à utiliser pour construire votre image. |
| `COPY`      | Copie des fichiers ou des répertoires de votre système de fichiers local dans le conteneur. |
| `RUN`       | Exécute des commandes dans le conteneur lors de la construction de l'image. |
| `CMD`       | Spécifie la commande par défaut à exécuter lorsque le conteneur est démarré. |
| `EXPOSE`    | Expose un port du conteneur pour que le trafic puisse y accéder. |
| `WORKDIR`   | Définit le répertoire de travail pour les commandes suivantes. |
| `ENV`       | Définit des variables d'environnement dans le conteneur. |

Vous pouvez également utiliser des commentaires dans un `Dockerfile` en utilisant le caractère `#`.