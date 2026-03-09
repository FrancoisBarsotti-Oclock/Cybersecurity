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

L’image n’est plus autonome (*self-sufficient*), puisqu’il faut réaliser une opération sur la machine hôte. Cela est contraire à l’esprit de [conteneurisation](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/Docker%20Gestion%20Espace%20Disque.md), et va concrètement poser problème pour le déploiement en production, par exemple.

### Solution
Combiner installation des dépendances dans l’image et utilisation d’un volume pour le code source.

```ruby
FROM node:12.4.0-alpine
USER node
COPY . /src
RUN npm -y install
WORKDIR /src
CMD ["npm", "dev"]
```

```apache
docker run --rm -it -v $(pwd):/src projet
```

### Problème 4
Quand le code source du projet a été modifié, et que l’image est re-`build`, même si les dépendances du projet sont les mêmes, elles sont toutes réinstallées.

Cela est la conséquence de l’ordre des instructions du Dockerfile :

* il faut faire le `COPY` avant le `RUN` pour avoir les infos sur les dépendances afin de les installer
* mais si un fichier source est modifié, le cache de l’instruction `COPY` est invalidée, ainsi que le cache de toutes les instructions qui suivent, donc celui de `RUN`, ce qui déclenche une réinstallation des dépendances.

### Solution

Isoler l’installation des dépendances dans le Dockerfile. Il n’y a plus besoin de copier le code source, puisqu’il est fournit par un volume. Désormais, une modification du code source suivi d’un re-`build` d’image utilisera les couches en cache.

Si un dossier d’installation des dépendances existe sur la machine hôte, le supprimer (sinon il écrasera celui de l’image, du fait du volume dans la commande `run`).

```ruby
FROM node:12.4.0-alpine
USER node
COPY package.json package.json /src
RUN npm -y install
WORKDIR /src
CMD ["npm", "dev"]
```

```apache
docker run --rm -it -v $(pwd):/src projet
```

>Il peut être nécessaire de copier des fichiers de configurations supplémentaires dans l’image de dev pour réaliser des opérations de pré-lancement, en plus de l’installation des dépendances. À adapter en fonction du projet.
>
>Si on souhaite que l’image de production intègre le code source pour être 100% autonome, alors il est conseillé de réaliser une image spécifique, différente de celle utilisée en dev. On peut aussi décider de fournir le code source à l’image en production par un volume, optimisé pour cet usage. Dans tous les cas, le déroulé exact des opérations en production a de grandes chances d’être différent du déroulé en dev, à commencer par la `CMD`, ce qui renforce la recommandation d’avoir un image de dev et une image de prod.

## Bonnes pratiques

* Comme pour tout le contenu copié dans une image, de préférence utiliser un utilisateur dédié au lieu de root. Attributer les droits de fichiers correspondants.
* Utiliser les options d’installation rapide et automatique du gestionnaire de paquet.
* Privilégier l’installation non-globale, si elle est possible.

### Exemples

### npm

```ruby
FROM node:12.4.0-alpine

# Utilisateur non-root.
USER node

# Installation des modules npm dans l'espace utilisateur.
ENV NPM_CONFIG_PREFIX=/home/node/.npm-global
ENV PATH=$PATH:/home/node/.npm-global/bin # optionally if you want to run npm global bin without specifying path

# Pré-installation automatique des dépendances du projet.
COPY --chown=node package.json package-lock.json /home/node/
RUN npm --non-interactive --no-optional --no-package-lock install

# Pré-mapping du code source du projet.
COPY --chown=node . /home/node

CMD [...]
```

### Yarn

```ruby
FROM node:12.4.0-alpine
USER node
# Yarn installe les modules dans l'espace utilisateur par défaut, rien à configurer.
COPY --chown=node package.json yarn.lock /home/node/
RUN yarn --non-interactive --ignore-optional --pure-lockfile
COPY --chown=node . /home/node
CMD [...]
```

