# Dockerfile

Quand on veut créer une nouvelle image Docker, il faut écrire une « recette de cuisine » qui en détaille les différentes étapes de construction. Cette recette, c’est le Dockerfile.

Un Dockerfile se trouve en général à la racine du projet, mais l’emplacement n’a que peu d’importance, puisqu’il sera possible d’adapter les instructions à l’intérieur pour retrouver les chemins des fichiers requis pour construire l’image.

### Exemple

Voici un exemple de Dockerfile permettant de créer une image qui, lorsqu’elle sera instanciée sous la forme d’un container, affichera des informations à propos de l’OS dans la console :
```ruby
FROM ubuntu:20.04
CMD ["uname", "-a"]
```

>La [commande `uname`](https://www.gnu.org/software/coreutils/manual/html_node/uname-invocation.html) permet d’afficher des informations à propos de l’OS (Operating System).

Une fois le Dockerfile écrit, l’image est construite avec une commande qui a la structure suivante :

```apache
docker build --tag nom-image chemin/vers/Dockerfile
```

Par exemple, pour construire une image nommé « os-inspector » à partir du Dockerfile ci-avant, et sachant que le Dockerfile se trouve dans le dossier courant, on lance :

```apache
docker build --tag os-inspector .
```

>Le . dans la commande de build représente le dossier courant – aurait aussi pu écrire docker build --tag os-inspector ./Dockerfile pour être explicite, mais c’est inutile.

Le processus de build donne lieu à la trace suivante en console :

```apache
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM ubuntu
latest: Pulling from library/ubuntu
5b7339215d1d: Pull complete
14ca88e9f672: Pull complete
a31c3b1caad4: Pull complete
b054a26005b7: Pull complete
Digest: sha256:9b1702dcfe32c873a770a32cfd306dd7fc1c4fd134adfb783db68defc8894b3c
Status: Downloaded newer image for ubuntu:latest
 ---> 4c108a37151f
Step 2/2 : CMD ["uname", "-a"]
 ---> Running in b01af11dd9f3
Removing intermediate container b01af11dd9f3
 ---> 9529855d655e
Successfully built 9529855d655e
Successfully tagged os-inspector:latest
```

Une fois l’image construite, on peut l’exécuter avec :

```apache
docker run os-inspector
```

On voit alors apparaître en console des informations à propos de l’OS du container :
```apache
Linux e726a7f9902e 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u3 (2019-06-16) x86_64 x86_64 x86_64 GNU/Linux
```

### Instructions dans un Dockerfile

Un Dockerfile est constitué d’instructions (on parle d’« instructions Docker »). Au minimum, il contiendra une instruction `FROM`, puisque c’est la seule requise, mais en général bien plus. Ci-dessous, les instructions les plus utiles.

### FROM

[Documentation officielle FROM](https://docs.docker.com/reference/dockerfile#from)

```apache
FROM ubuntu:18.04
```

`FROM` indique sur quelle autre image pré-existante on souhaite se baser pour construire la nouvelle. Dans l’exemple ci-avant, l’image de base s’appelle « ubuntu », le « 18.04 » est le tag (une version spécifique de l’image). Elle fournit l’OS Ubuntu, qui servira donc de base de travail pour la nouvelle image.

Si l’image référencée par le FROM n’est pas disponible sur la machine où se déroule le build, elle est téléchargée depuis un répertoire d’images — par défaut [hub.docker.com](https://hub.docker.com/).

L’image de base référencée peut tout à fait elle-même être basée sur une image, et ainsi de suite. Le Dockerfile décrit donc une nouvelle image qui « hérite » d’une ou plusieurs autres images, chacune ayant été générée à partir d’un Dockerfile.

>Certaines images, notamment celles fournissant un OS, comme c’est le cas pour l’image « ubuntu », ne se basent sur « rien » — la valeur de leur `FROM` est [scratch](https://hub.docker.com/_/scratch), une image vide. L’image scratch est donc le point de départ de la pile d’héritage d’images. Dans l’exemple ci-dessus :
>
>`scratch <-- ubuntu <-- image buildée à partir du Dockerfile`

### LABEL

[Documentation officielle de LABEL](https://docs.docker.com/reference/dockerfile#label)

```apache
LABEL orga=oclock type=application maintainer=tech@oclock.io
```

`LABEL` permet d’ajouter des annotations (métadonnées) à l’image, par un système de clé/valeur librement choisis. On peut ensuite visualiser les labels d’une image avec la commande `docker inspect`.

>Il existe des recommandations à propos de l’utilisation des annotations dans les images Docker, notamment [celles](https://github.com/opencontainers/image-spec) de [OCI](https://opencontainers.org/). Par exemple, pour indiquer l’auteur d’une image, on utilisera :
>```apache
>LABEL org.opencontainers.image.authors=tech@oclock.io
>```
>[Exemple d’une image respectant ce format](https://github.com/tmaier/docker-compose/blob/master/Dockerfile).

### COPY

>[Documentation officielle de COPY](https://docs.docker.com/reference/dockerfile#copy)
```apache
COPY . /src
```

`COPY` permet de copier des fichiers et des dossiers de la machine hôte, dans l’image. Ce contenu fera alors partie intégrante de l’image, qui y aura accès même si elle est utilisé par une autre personne, sur une autre machine.

>L’instruction `ADD` est similaire à `COPY`, mais permet notamment de gérer en plus la copie depuis une URL externe, ce qui peut poser un problème de sécurité. Toujours privilégier `COPY`, sauf cas particulier.

### WORKDIR

`WORKDIR` permet de se placer, dans l’image, à un endroit spécifique. Toutes les instructions Docker ultérieures (en-dessous dans le Dockerfile) seront lancées depuis cet endroit, aussi bien au moment du `build` que du `run`.

>Si le dossier référencé par `WORKDIR` n’existe pas, il est créé, avec les droits de l’utilisateur courant (tel que défini par `USER`, dans le Dockerfile courant ou via une image héritée par `FROM`).

### RUN

[Documentation officielle de RUN](https://docs.docker.com/reference/dockerfile#run)

```apache
RUN echo "une commande lancée pendant le build"
```

`RUN` permet de lancer une commande pendant la construction de l’image (`build`).

Au cours du `build`, chaque instruction Docker crée une nouvelle couche (layer) dans l’image. Les instructions `RUN` sont souvent très nombreuses, aussi pour éviter de multiplier les layers (et donc pour minimiser le poids final de l’image), on regroupera autant que faire se peut les instructions `RUN` en une seule, avec des `&&` (et `\` en fin de ligne pour écrire une seule instruction Docker sur plusieurs lignes) :

```apache
RUN apt-get -y update \
 && apt-get install --no-install-recommends -y vi emacs \
 && rm -rf /var/lib/apt/lists/*
```

### CMD

[Documentation officielle de CMD](https://docs.docker.com/reference/dockerfile#cmd)

```nginx
CMD ["echo", "une commande lancée pendant le run"]
```

`CMD` permet de lancer une commande au démarrage du container.

L’instruction `CMD` est optionnelle. Si le Dockerfile n’en précise pas (et que les images héritées non plus), alors le container ne fera rien par défaut. Il sera par contre possible de fournir dynamiquement une valeur pour `CMD` via la commande `docker run`, l’image n’est donc pas totalement inutile pour autant !

>En l’absence de `CMD`, le container se lance mais ne « fait rien ». On peut par contre entrer dedans pour venir y réaliser un travail en bénéficiant du contexte mis en place par l’image. Avec un `CMD`, le container se lance et « fait quelque chose par défaut » : c’est donc un exécutable à part entière. La plupart des images sont construites à partir de Dockerfile qui spécifient une instruction `CMD`, selon la philosophie « une image = un service » ([source](https://docs.docker.com/engine/containers/multi-service_container/)).

Un Dockerfile ne prend en compte qu’une seule instruction `CMD`. Si plusieurs sont écrites, seule la dernière est mémorisée dans l’image au final : ce sera LA (seule) commande lancée au démarrage du container.

La valeur associée à `CMD` peut s’écrire sous deux formats :

* format exec : ["executable", "param1", "param2"]
* format shell : executable param1 param2

Bien que le format shell soit plus naturel, c’est bien le format exec qui est recommandé. Pour faire simple, le format shell, comme son nom l’indique, lance la commande dans un shell. Ainsi, les deux instructions suivantes sont équivalentes :

* CMD echo "salut"
* CMD ["/bin/sh", "-c", "echo", "salut"]

La plupart du temps, on ne souhaite pas lancer l’exécutable (ici `echo`) dans un shell (parce que c’est inutile, parce que l’image ne contient pas le shell en question, etc.) Par ailleurs, le format shell n’est pas compatible avec l’instruction `ENTRYPOINT`.

### ENTRYPOINT 

[Documentation officielle de ENTRYPOINT](https://docs.docker.com/reference/dockerfile#entrypoint)

```nginx
ENTRYPOINT ["npm"]
```

`ENTRYPOINT` permet de définir un exécutable par défaut qui sera lancé au démarrage du container.

Si une instruction `CMD` est fournie par ailleurs, elle viendra compléter `ENTRYPOINT` par concaténation :

```nginx
# Une image nommée "js-app".
ENTRYPOINT ["npm"]
CMD ["start"]
```
```nginx
docker run js-app # => "npm start"
```

Il est alors possible de surcharger CMD dynamiquement :
```apache
docker run js-app dev # => "npm dev"
```

Il est également [possible de surcharger `ENTRYPOINT`](https://oprearocks.medium.com/how-to-properly-override-the-entrypoint-using-docker-run-2e081e5feb9d), même si ce n’est pas un cas d’usage fréquent :

```nginx
docker run --entrypoint=/bin/sh js-app # => "/bin/sh start"
docker run --entrypoint=/bin/sh js-app -c whoami # => "/bin/sh -c whoami"
```

>Toute image a en fait un `ENTRYPOINT` par défaut : `["/bin/sh", "-c"]`. Cela explique pourquoi la forme shell de `CMD` est équivalente à la forme exec `["/bin/sh", "-c", …]`. Une instruction explicite `ENTRYPOINT` dans un Dockerfile permet tout simplement de surcharger cette valeur par défaut.

### Résumé sur `CMD` et `ENTRYPOINT` :

* Si le Dockerfile contient seulement `CMD`, la commande spécifiée est lancée au démarrage du container. Elle peut être surchargée avec un ou plusieurs argument(s) passé(s) à `docker run` ;

* Si le Dockerfile contient seulement `ENTRYPOINT`, la commande spécifiée est lancée au démarrage du container. Si `docker run` est lancée avec un ou plusieurs argument(s), il(s) est(sont) concaténé(s) à la commande spécifiée par `ENTRYPOINT` ;

* Si le Dockerfile contient `ENTRYPOINT` et `CMD`, la concaténation des deux forme la commande lancée au démarrage du container. Chacun peut être surchargé via `docker run`.

### EXPOSE

[Documentation officielle EXPOSE](https://docs.docker.com/reference/dockerfile#expose)

```nginx
EXPOSE 80 443
```
`EXPOSE` permet de déclarer que le container écoutera un ou plusieurs ports réseau.

Attention : cette instruction n’a aucun effet concret, elle est uniquement déclarative. D’autres commandes ou programmes vont examiner cette information pour agir (ouvrir les ports en question, s’y connecter, etc.)

Par exemple, une image contenant un serveur web et qui sera lancé au démarrage du container avec `CMD` va déclarer les ports écoutés par le serveur web (typiquement, 80 pour le trafic http et 443 pour https). Il s’agit donc bien des ports du container, internes.

Dans un second temps, au moment du `run`, la machine hôte pourra réaliser un mapping de ports pour raccorder le trafic réseau hôte et le trafic réseau dockerisé :

* soit de manière automatique (ports choisis au hasard) avec `docker run -P`

* soit de manière spécifique avec, par exemple, `docker run -p 3000:80 -p 3443:443`

Dans ce second cas, une requête http arrivant sur le port 3000 de la machine hôte sera automatiquement transmise au port 80 du container, donc vers le serveur web dockerisé. La syntaxe du mapping de port est donc `port_hote:port_container`.

### ENV

[Documentation officielle ENV](https://docs.docker.com/reference/dockerfile#env)

```nginx
ENV NODE_ENV=production
```

`ENV` permet de déclarer une variable d’environnement, active pendant le `build` et le `run` de l’image.

>Comme beaucoup d’autres informations du Dockerfile, les variables d’environnement ainsi définies sont visibles sans lancer de container, avec la commande `docker inspect`.

Il est possible de surcharger la valeur d’une variable d’environnement au moment du `run` avec `docker run --env NODE_ENV=development` par exemple.

Si une variable d’environnement ne doit pas être persistante, mais n’est seulement utile que pour la durée d’une commande, alors il ne faut pas utiliser `ENV` mais plutôt l’intégrer directement en préfixe de l’instruction `RUN` correspondante :

```nginx
RUN NODE_ENV=production npm install
```

### Autres instructions utiles

* `VOLUME` : voir la fiche-récap sur les [volumes Docker]().
* `ARG`, `HEALTHCHECK` : voir la doc officiel
* `FROM` multi-stage : voir la documentation officielle
* etc. (documentation officielle Dockerfile)

