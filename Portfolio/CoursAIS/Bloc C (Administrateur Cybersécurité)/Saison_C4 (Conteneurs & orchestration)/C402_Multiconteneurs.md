# Session C402. Multiconteneurs

**Notions du jour :**

## Docker : Build & Compose

_Construire ses images et orchestrer ses conteneurs_

### Ce qu'on sait déjà
* **Image** = la recette (immuable)
* **Conteneur** = le plat servi (instance d'une image)
* **DockerHub** = le supermarché des images

Aujourd'hui, on apprend à **cuisiner nos propres recettes !**

### Le Dockerfile

_La recette de votre image_

### C'est quoi ?
Un fichier texte qui décrit **étape par étape** comment construire une image Docker.

Comme une recette de cuisine : on part d'une base, on ajoute des ingrédients, on configure.

### Les instructions essentielles

* **FROM** - l'image de base (point de départ)
* **RUN** - exécuter une commande pendant le build
* **COPY** - copier des fichiers locaux dans l'image (comme un `cp`)
* **WORKDIR** - définir le répertoire de travail (comme un `cd`)
* **EXPOSE** - déclarer un port
* **CMD** - la commande lancée au démarrage du conteneur

## Comparaison installation apache sur LXC et sur Docker

### LXC :

- apt update
- apt upgrade -y
- apt install apache2

### Docker
- FROM ubuntu
- RUN apt update
- RUN apt upgrade -y
- RUN apt install apache2 -y

→ Optimisé en une seule couche:
```apache
RUN apt update && apt upgrade -y && apt install apache2 -y
```

### Exemple : un serveur web simple
```apache
FROM nginx:alpine

COPY ./mon-site/ /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

_4 lignes et on a une image prête à servir notre site !_ 🚀

### Exemple : une app Node.js
```apache
FROM node:20-alpine

WORKDIR /app

COPY package *. json . /
RUN npm install

COPY

EXPOSE 3000

CMD ["node", "server. js"]
```

_On copie d'abord le package.json pour profiter du **cache Docker** sur les dépendances_ 🧠

## Docker Build
_Transformer un Dockerfile en image_

.

### La commande magique
```
docker build -t mon-app:v1
```

* -t mon-app:v1 -> le nom et le tag de l'image
* . -> le contexte de build (le dossier courant)

## Les layers (couches) 🧅

Chaque instruction du Dockerfile crée une **couche** :

* Les couches sont **mises en cache** 💾
* Si rien ne change -> pas de rebuild
* D'où l'importance de l'**ordre des instructions**

_**Astuce** : mettez les éléments qui changent le moins souvent en premier !_

### Le .dockerignore 🚫
Comme un .gitignore, mais pour Docker :
```apache
node_modules
.git
. env
*. md
```

Évite d'envoyer des fichiers inutiles dans le contexte de build.

## Bonnes pratiques 📋

### Les réflexes à avoir

* 🍂 Utiliser des images **légères** (alpine)
* 📦 **Minimiser** le nombre de couches (combiner les RUN)
* 🔒 Ne **jamais** mettre de secrets dans l'image
* 👤 Utiliser un **utilisateur non-root** quand c'est possible

### Exemple : combiner les RUN

❌ Mauvais :
```apache
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y vim
```

✅ Mieux :
```apache
RUN apt-get update && apt-get install -y \
    curl
    vim \
    && rm -rf /var/lib/apt/lists/*
```

## Exemple Exercice : création conteneur sur Ubuntu
Sur nano Dockerfile, 

Une première version incorrecte :
```apache
FROM ubuntu:24.04

# Prérequis d'installation
RUN apt update
RUN apt upgrade -y
# RUN apt install curl -y (on s’est rend compte qu’il n’est plus nécessaire avec le nodejs)

# Installation de NodeJS
# RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.10.1/install.sh | bash
# RUN chmod +x /root/ .nvm/nvm.sh
#RUN ./root/ .nvm/nvm.sh install 24

RUN apt install nodejs -y
RUN apt install npm -y

# Je copie les fichiers de mon application dans mon conteneur
COPY . /app

# J'installe Vite
WORKDIR /app
RUN npm install

# Expose mon port 5173 (Attention il c'est le port COTE CONTENEUR et pas coté hote)
EXPOSE 5173

# Je lance mon application
CMD npm run prod
```

➡️ Pour lancer le document nano avec `sudo docker build -t dockerdemo .`

### Pour optimiser tout cela (réduction des couches)

_Le antislash `\` sert à continuer la même (longue) commande à la ligne_

```apache
FROM ubuntu:24.04

# Prérequis d'installation et Installation de NodeJS
RUN apt update && \
apt upgrade -y && \
apt install nodejs -y --no-install-recommends --no-install-suggests && \ 
apt install npm -y --no-install-recommends --no-install-suggests

# Je copie les fichiers de mon application dans mon conteneur
COPY . /app

# J'installe Vite
WORKDIR /app
RUN npm install

# Expose mon port 5173 (Attention il c'est le port COTE CONTENEUR et pas coté hote)
EXPOSE 5173

# Je lance mon application
CMD npm run prod
```

➡️ Chaque rebuild écrasera le précédent, si l’on ne change pas le numéro.

### Optimisation 2

On peut aussi optimiser encore plus avec un node qui fasse un multiconteneur

D’abord on crée un premier conteneur

```apache
FROM node:alpine

# Prérequis d'installation et Installation de NodeJS
# RUN apt update && \
# apt upgrade -y && \
# apt install nodejs -y --no-install-recommends --no-install-suggests && \
# apt install npm -y --no-install-recommends --no-install-suggests

# Je copie les fichiers de mon application dans mon conteneur
COPY . /app

# J'installe Vite
WORKDIR /app
RUN npm install

# Expose mon port 5173 (Attention il c'est le port COTE CONTENEUR et pas coté hote)
EXPOSE 5173

# Je lance mon application
CMD npm run prod
```

### Optimisation 3

Puis le deuxième conteneur, qui va copier ce qui avait été créé dans le premier conteneur

```apache
FROM node:alpine AS build

# Je me met dans le répertoire de travail
WORKDIR /app

# Je copie uniquement les fichiers packages, car en node j'ai juste besoin de ces fichiers pour construire mon application.
COPY package*.json .

# La commande npm ci installe les dépendances nécessaire pour LE PROJET
RUN npm ci

# Je remet tout le reste de mon projet dans le conteneur
COPY . .

# Construire le projet
RUN npm run build

# 2e conteneur
# Je pars à nouveau d'une base node, pour pouvoir lancer mon application
FROM node:alpine

# Je me met à nouveau dans le répertoire de travail
WORKDIR /app

# Je copie uniquement les fichiers packages, car cette fois ci j'en ai besoin pour LANCER mon application
COPY package*.json .

# Je dois récupérer le dossier dist qui se trouve dans le premier conteneur
COPY --from=build /app/dist ./dist

# Expose mon port 5173 (Attention il c'est le port COTE CONTENEUR et pas coté hôte)
EXPOSE 5173

# Je lance mon application
CMD node dist/index.js
```

## Docker Compose 🎼

_Orchestrer plusieurs conteneurs_

### Le problème
Une application web moderne, c'est souvent :

* 🌐 Un frontend (Nginx, Apache ... )
* ⚙️ Un backend (Node, PHP, Python ... )
* Une base de données (MySQL, PostgreSQL ... )
* 📦 Un cache (Redis ... )

Gérer tout ça à la main avec docker run ?

### La solution : Docker Compose

Un fichier `docker-compose. yml` qui décrit **toute l'architecture** :

Quels conteneurs, quels réseaux, quels volumes, quelles dépendances.

_**Analogie** : le chef d'orchestre qui coordonne tous les musiciens_ 🎵

### Syntaxe YAML - rappel express
_L’indentation est vitale ici_
```apache
# Un commentaire
cle: valeur
liste:
    - element1
    - element2
objet :
    sous_cle: sous_valeur
```

_C'est comme du JSON, mais plus lisible (et sensible à l'indentation !)_

### Exemple concret 💻

_Une stack LAMP en Docker Compose_

### Le fichier docker-compose.yml

```ruby
services:
    web:
        image: php:8.2-apache
        ports:
            - "8080:80"
        volumes :
            +./src:/var/www/html
        depends_on:
            - db
    db:
        image: mariadb:11
        environment:
            MYSQL_ROOT_PASSWORD: secret
            MYSQL_DATABASE: myapp
        volumes :
            - db_data:/var/lib/mysq1
volumes :
    db_data:
```

### Décryptage 🔍

* **services** : les conteneurs à lancer
* **image** : quelle image utiliser
* **ports** : mapping host -> conteneur
* **volumes** : persistance des données
* **depends_on** : ordre de démarrage
* **environment** : variables d'environnement

### Les essentielles

* docker compose up -d -> tout lancer en arrière-plan 🚀
* docker compose down -> tout arrêter et nettoyer 🧹
* docker compose ps ->voir l'état des services 👀
* docker compose logs -f ->suivre les logs en temps réel 📋

### Les pratiques 

* docker compose build -> rebuild les images custom 🔨
* docker compose exec web bash -> entrer dans un conteneur 🧴
* docker compose restart web ->redémarrer un service ♻️
* docker compose pull ->mettre à jour les images ⬇️

## Réseaux dans Compose 🌐

### Par défaut

Docker Compose crée automatiquement un réseau pour chaque projet.

Les conteneurs se trouvent par leur **nom de service** !

```apache
# Dans Le code PHP :
# host = "db" (pas "Localhost" ni "127.0.0.1")
$pdo = new PDO("mysq1:host=db;dbname=myapp", "root", "secret");
```

### Réseaux custom
```apache
services:
    web:
        networks:
            - frontend
            - backend
    db:
        networks:
            - backend
networks:
    frontend:
    backend:
```
_Le service db n'est pas accessible depuis le réseau frontend → segmentation !_

## Volumes & persistance 💾

### Le problème

Un conteneur est **éphémère** : si on le supprime, les données disparaissent

Solution : les **volumes** !

### Deux types
* **Named volumes** : gerés par Docker, persistants `db_data: /var/lib/mysql`
* **Bind mounts** : lien vers un dossier de l'hôte `. /src : /var/www/html`

_Bind mounts = pratique en dev. Named volumes = recommande en prod_

[Video explicatif de Volume](https://www.youtube.com/watch?v=lstTLSM5494)




Challenge du jour 👉 [Challenge_C402]() 👈

