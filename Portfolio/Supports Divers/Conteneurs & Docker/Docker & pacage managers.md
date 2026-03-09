# Docker et les package managers

**Situation** : un projet en développement dans lequel un *package manager* est utilisé (composer en PHP, npm en JS, etc.) Le projet est dockerisé sous la forme d’une image optimisée pour le développement, avec un Dockerfile du type :

```ruby
# Exemple : image "projet" sur une base Node.js avec npm.
FROM node:12.4.0-alpine
USER node
COPY . /src
RUN npm -y install
WORKDIR /src
CMD ["npm", "dev"]
```

```apache
docker run --rm -it projet
```

## Optimisation & workflow de dev

### Problème 1

À chaque modification du code source, il faut rebuild l’image et relancer un container pour vérifier son travail.

### Solution
Utiliser un volume au lieu du `COPY`, pour partager le dossier courant :

```ruby
FROM node:12.4.0-alpine
USER node
WORKDIR /src
CMD ["npm", "dev"]
```

```apache
docker run --rm -it -v $(pwd):/src projet
```

### Problème 2
Quand le container démarre, il lance `npm dev` qui exécute le code source. Ce runtime a besoin que les dépendances du projet soient installées, mais ce n’est plus le cas.

### Solution
Lancer `npm install` sur la machine hôte, pour que le dossier d’installation des dépendances soit bien embarqué dans le volume du container.

### Problème 3

L’image n’est plus autonome (*self-sufficient*), puisqu’il faut réaliser une opération sur la machine hôte. Cela est contraire à l’esprit de [conteneurisation](), et va concrètement poser problème pour le déploiement en production, par exemple.

### Solution
Combiner installation des dépendances dans l’image et utilisation d’un volume pour le code source.