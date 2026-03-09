# Les fiches récap de l'école O'clock

## Docker: Commandes utiles

### Utilisation d’images pré-existantes

```ruby
# Lancer un container basé sur l'image pré-existante "hello-world"
# (téléchargée au besoin depuis hub.docker.com)
# Éxécute la CMD définie par défaut dans son Dockerfile :
docker run hello-world

# Lancer une commande perso au sein d'une image pré-existante :
docker run ubuntu ls -l

# Mode interactif -it (pour pouvoir entrer dans un container via un shell) :
docker run -it alpine sh

# Lister les containers et trouver leurs IDs :
docker ps    # seulement les actifs
docker ps -a # tous

# Lister les images :
docker image list # alias : docker images

# Supprimer un container, en connaissant son ID ou son nom:
docker rm {containerID|containerName}

# Supprimer une image, en connaissant son ID ou son nom :
docker rmi {imageID|imageName} # ex. docker rmi ubuntu

# astuce: dans la grande majorité des cas, on peut utiliser uniquement les 4 premiers caractères d'un ID:
# Exemple:
docker ps -a
>CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS                  PORTS               NAMES
>88bfb7b61c5b        oclock/typescript      "npm run dev"       2 days ago          Exited (0) 2 days ago   5050                blissful_heisenberg
# Pour supprimer ce container, les 3 commandes suivantes sont valides
docker rm blissful_heisenberg
docker rm 88bfb7b61c5b
docker rm 88bf

# Supprimer tous les containers éteints
docker container prune

# Télécharger une image depuis hub.docker.com (par défaut) :
docker pull node          # mauvaise pratique : trop général
docker pull node:latest   # mauvaise pratique : trop général
docker pull node:10.15.0-alpine # OK !

# Lancer "yarn -v" dans un container basé sur l'image officielle Node.js :
docker run node yarn -v

# Exécuter du JS dans un container basé sur l'image officielle Node.js :
docker run node node -e "console.log(1+1)"

# Lancer une image dans un container qui s'auto-détruit à la fin de l'execution :
docker run --rm nomImage
```

### Port Mapping
Par défaut, un container est totalement isolé. Pour pouvoir y accéder depuis la machine hôte, on doit *mapper* (cartographier) les ports, c’est-à-dire faire correspondre des ports du container à des ports de la machine hôte.

On utilise l’option -p, selon le format `port-local:port-container `:

```apache
# le serveur de base de données sera accessible depuis localhost:3333
docker run -p 3333:27017 mongo
```

### Construction d’images

```ruby
# Construire une image à partir du Dockerfile du dossier courant :
docker build -t nom-image . # où nom-image est librement choisi

# Lancer l'image (== créer un nouveau container à partir de cette image) :
docker run nom-image
# ou bien docker run --rm -it nom-image, docker run --rm -it nom-image sh, etc.

# Lancer une image (== crée un nouveau container) avec un nom spécifique pour le container :
docker run --name nom-container nom-image

# Stopper un container en ayant trouvé son ID via "docker ps" :
docker stop ID

# Stopper un container avec son nom :
docker stop nom-container

# Relancer un container avec son nom ou son ID :
docker start nom-ou-ID-container

# Forcer un container qui tourne à redémarrer:
docker restart nom-ou-ID-container
```

### Networks

```apache
# Lister les networks
docker network ls

# Créer un nouveau network de type bridge
docker network create nom-du-network

# Supprimer un network
docker network rm nom-du-network

# Supprimer tous les networks sur lesquels aucun container n'est branché
docker network prune

# Instancier un container en le branchant dans un network
docker run --net=my-network my-image

# Brancher un container sur un network existant
docker network connect my-network my-container

# Déconnecter un container d'un network
docker network disconnect my-network my-container

# Inspecter un network
# Cette commande peut être utile pour trouver l'adresse IP d'un container dans ce network.
docker network inspect my-network
```

### Volumes

```apache
# Créer un volume
docker volume create mon-volume-test

# Monter un volume
docker run -v mon-volume-test:/path/to/volume mon-image

# Lister les volumes
docker volume ls

# Supprimer un volume (il ne doit être lié à aucun container - même éteint!)
docker volume rm nom-du-volume

# Supprimer les volumes non-utilisés (c-à-d qui ne sont liés à aucun container)
docker volume prune

# Monter un volume en lecture seule (on concatène :ro)
docker run -v mon-volume:/path/to/volume:ro mon-image

# Bind mount
docker -v /local/path:/container/path mo-image
```

#