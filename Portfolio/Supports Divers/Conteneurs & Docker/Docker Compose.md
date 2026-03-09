# Les fiches récap de l'école O'clock

## Docker Compose

Pour déployer des applications construites sous la forme de services multiples.

### Mise en situation

La plupart des applications web utilisent plusieurs services, ne serait-ce qu’une base de données.

Pour respecter les principes d’isolement, on doit donc déployer plusieurs containers (et souvent même construire plusieurs images). Par exemple :

* créer un network `my-app-network`

* `pull` et `run` une image mongoDB, et connecter le container au network `my-app-network`

* `build` et `run` l’image d’une application NodeJS, connecter le container au network `my-app-network`, configurer l’url d’accès à la base de données, et enfin mapper un port d’accès sur la machine locale.

Le nombre d’opérations augmente extrêmement vite dès lors que l’on rajoute des services. Sans compter le fait que les containers doivent parfois être lancés « dans le bon ordre ». Entrer toutes ces commandes une à une dans un terminal peut vite devenir très chronophage !

Docker répond à cette problématique en nous offrant la possibilité de décrire toute l’architecture de notre application à travers un seul fichier : `docker-compose.yml`

### Structure du fichier

Voici un fichier `docker-compose.yml` correspondant à l’exemple précédent :

```ruby
version: '3'
networks:
  my-app-network:
services:
  mongo:
    image: mongo:4.0.10
    networks:
      - my-app-network
  web:
    build: .
    networks:
      - my-app-network
    command: ["run", "start"]
    ports:
      - "5050:5050"
    depends_on:
      - mongo
    env_file: .env
    environment:
      - MONGODB_URI=mongodb://mongo/demo
```

Analysons le contenu de ce fichier :

### version

Spécifie la version de docker-compose utilisée. [Cette page](https://docs.docker.com/reference/compose-file/version-and-name/) indique la version à utiliser en fonction de la version de Docker installée.

### networks (à la racine)

[Reference officielle](https://docs.docker.com/reference/compose-file/networks/)

La liste des networks de l’application. Ici, on en crée un seul (my-app-network) en laissant toutes les options par défaut ([référence en anglais](https://docs.docker.com/reference/compose-file/#network-configuration-reference))

### services
La liste de tous les services de l’application. Ici on en déclare 2 (`mongo` et `web`), chacun ayant ses propres options.

### image

[Reference officielle](https://docs.docker.com/reference/compose-file/#image)

Permet de déclarer qu’un service va utiliser une image déjà construite. A l’instar de `docker run`, si l’image n’est pas présente localement, Docker essaie de la `pull`.

### networks (dans un service)

[Reference officielle](https://docs.docker.com/reference/compose-file/#networks)

Permet de déclarer les networks auquel le service doit se connecter. Les networks utilisés doivent être déclarés dans l’attribut `networks`(à la racine du fichier), ou avoir été créés à la main au préalable.

### build

[Reference officielle](https://docs.docker.com/reference/compose-file/build/)

Indique que l’image doit être construite (i.e. elle n’existe pas encore). La valeur de cet attribut peut être le chemin d’un dossier contenant un `Dockerfile`, ou un objet ([cf. Référence, en anglais](https://docs.docker.com/reference/compose-file/#build)).

### command

[Reference officielle](https://docs.docker.com/reference/compose-file/#command)

Permet de surcharger l’instruction `CMD` d’un Dockerfile. 

### ports

[Reference officielle](https://docs.docker.com/reference/compose-file/#ports)

Mapping de ports 

### depends_on

[Reference officielle](https://docs.docker.com/reference/compose-file/#depends_on-1)

Indique que le service dépend directement d’un autre service présent dans la même stack. Ainsi le service ne sera lancé que lorsque celui dont il dépend sera lui-même lancé et disponible.

### environment

[Reference officielle](https://docs.docker.com/reference/compose-file/#environment)

Permet de fournir des variables d’environnement.

### env_file

[Reference officielle](https://docs.docker.com/reference/compose-file/#env_file)

Permet de fournir des variables d’environnement depuis un fichier local.

## Commandes docker-compose

[Reference](https://docs.docker.com/reference/cli/docker/compose/)

Toutes les commandes sont à lancer dans le dossier ou se trouve le fichier `docker-compose.yml`

```apache
# Build tous les services d'une stack
docker compose build

# Lancer tous les services d'une stack (il seront build si besoin)
docker compose up

# Pour forcer un rebuild au passage
docker compose up --build

# Pour ne lancer qu'un seul service (lancera les "depends_on" si nécessaire)
docker compose up mon-service

# Pour instancier un nouveau container pour un service (lancera les "depends_on" si nécessaire)
docker compose run mon-service

# Rebuild un seul service
docker compose build mon-service
```

Si vous utilisez `docker compose` dans sa `v1` la syntaxe est `docker-compose` à la place de `docker compose`.

#
