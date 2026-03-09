# Les fiches récap de l'école O'clock

## Docker: Astuces & bonnes pratiques

### Dockerfile

Consulter la [fiche-récap dédiée](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/DockerfileR%C3%A9cap.md).

### Container

### Gestionnaire de process

TODO: documenter la problématique, krallin/tini avec `docker run --init`, [s6-overlay](https://github.com/just-containers/s6-overlay)…

### Variables d’environnement

Si l’applicatif dockerisé et/ou son runtime, ses dépendances… ont des attentes particulières en terme d’environnement, les gérer avec des variables d’environnement.

Par exemple, une application Node.js en production attend traditionnellement que `NODE_ENV` existe et ait la valeur `production`.

La variable peut être fournie at runtime (`docker run -e NODE_ENV=production ...`) ou statiquement par l’image :

```apache
FROM ...
ENV NODE_ENV production
```

### Gestionnaires de paquets

Consulter la [fiche-récap dédiée](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/Docker%20%26%20package%20managers.md).

### Dépendances systèmes

Certains applicatifs dockerisés ont des dépendances systèmes, c’est-à-dire des dépendances gérées non pas par le gestionnaire de paquet du langage d’implémentation, mais directement au niveau de l’OS.

Par exemple, une application Node.js peut avoir besoin de compiler des modules natifs avec [node-gyp](https://github.com/nodejs/node-gyp), ce qui requière d’avoir installé Python, `make` et un compilateur C/C++. L’image officielle node ne fournit pas ces dépendances système, il faut donc les intégrer à l’image custom du projet.

### Version 1

```ruby
FROM node:12.4.0-alpine
RUN apk add --no-cache --virtual .gyp python make g++ \
 && npm install \
 && apk del .gyp
```

### Version 2

En utilisant un [Dockerfile multi-stage](https://docs.docker.com/build/building/multi-stage/), il est possible de pré-installer les dépendances système, de compiler les modules natifs, puis de se débarasser des dépendances systèmes devenues inutiles à l’image finale.

```ruby
FROM node:12.4.0-alpine as builder
RUN apk add --no-cache --virtual .gyp python make g++ \
 && npm install

FROM node:12.4.0-alpine as app
COPY --from=builder node_modules .
```

#