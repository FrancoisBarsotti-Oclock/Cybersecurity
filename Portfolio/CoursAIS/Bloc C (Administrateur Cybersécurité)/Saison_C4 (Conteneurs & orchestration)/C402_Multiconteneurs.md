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

