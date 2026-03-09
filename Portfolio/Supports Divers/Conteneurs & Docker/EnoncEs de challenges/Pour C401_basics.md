# Exercice : bases de la CLI Docker

> Le "hello world" en console c'est bien, mais on fait du dev web nous !

Et si on se faisait un petit "hello world" dans un navigateur ?

## Étape #1 : votre premier conteneur

Ouvrez un terminal, et lancez la commande suivante : `docker run -p 8888:80 bdelphin/hello-docker`.

Vous pouvez maintenant ouvrir votre navigateur à l'adresse [http://localhost:8888](http://localhost:8888), vous devriez voir un message "Hello, world !" :tada:

Que vient-il de se passer ? Nous avons lancé un nouveau conteneur grâce à la commande `docker run`. Cette commande attends au minimum un argument, **l'image à lancer**. Ici, nous lui avons indiqué de lancer l'image `bdelphin/hello-docker`.

Nous lui avons founi un argument supplémentaire : `-p 8888:80`. Cet argument permet d'indiquer à Docker qu'il faut mettre en place une redirection de port, et dans notre cas on lui demande de rediriger le port 8888 de notre hôte sur le port 80 du conteneur.

Vous pouvez stopper le conteneur en appuyant sur Ctrl+C.

## Étape #2 : lancer en tâche de fond

Afin d'éviter de "bloquer" notre terminal, on peut lancer un conteneur en tâche de fond. Pour ce faire, on utilise l'argument `-d`.

Essayez de relancer le conteneur avec la commande suivante : `docker run -d -p 8888:80 bdelphin/hello-docker`. Une chaîne de caractères s'affiche dans le terminal, et on récupère immédiatement la main dessus. Vérifiez dans votre navigateur que vous avez bien toujours votre "hello world" à l'adresse [http://localhost:8888](http://localhost:8888).

:bulb: _Astuce : certains arguments peuvent être combinés, ici par exemple on peut combiner `-d` et `-p` en `-dp` ! La commande `docker run -dp 8888:80 bdelphin/hello-docker` fait donc exactement la même chose._

> Mais comment on fait pour stopper/supprimer ce conteneur maintenant ? :scream:

Pas de panique ! Docker a tout prévu, et nous fourni des commandes pour stopper, démarrer, supprimer, et consulter l'état de nos conteneurs.

Pour consulter l'état de nos conteneurs, lancez la commande `docker ps`. On retrouve dans la sortie de cette commande de nombreuses informations utiles, comme l'image lancée, depuis combien de temps, son status, les redirections de ports actives, et surtout le nom et l'ID de nos conteneurs.

Pour stopper un conteneur, vous pouvez utiliser la commande `docker stop <nom_conteneur>` ou `docker stop <id_conteneur>`, en remplaçant `<nom_conteneur>` ou `<id_conteneur>` par celui indiqué par la commande `docker ps`.

Essayez de stopper le conteneur que nous venons de lancer ! Vérifiez ensuite avec `docker ps` que le conteneur est bien stoppé.

## Étape #3 : nommer nos conteneurs

Vous l'avez peut-être remarqué, Docker assigne à nos conteneurs un nom choisi au hasard. Pour ne pas avoir à lancer `docker ps` en permanence juste pour connaître le nom de nos conteneurs, on peut choisir leur nom (et pour cela, on va utiliser l'argument `--name`) !

Relancez le conteneur avec la commande suivante : `docker run -dp 8888:80 --name hello-docker bdelphin/hello-docker`. Vérifiez avec `docker ps` que le nom a bien été pris en compte.

Stoppez le conteneur et relancez-le avec commande ci-dessus. Vous devriez avoir un beau message d'erreur, indiquant que le nom "hello-docker" est déjà utilisé par un conteneur ...

Si l'on veut relancer ce conteneur, deux options :

- on peut le relancer directement avec la commande `docker start hello-docker`
- on peut supprimer au préalable le conteneur avec la commande `docker rm hello-docker`, puis le relancer avec `docker run`.

:bulb: _Astuce : on peut stopper et supprimer un conteneur en une seule commande : `docker rm -f <nom_conteneur>` ou `docker rm -f <id_conteneur>`._

## D'autres commandes ?

Nous allons découvrir par la suite quelques commandes supplémentaires, mais si vous êtes curieux, l'interface en ligne de commande de Docker est [très bien documentée](https://docs.docker.com/engine/reference/commandline/cli/).

## Dépôt d'images : DockerHub

Vous vous êtes peut-être demandé d'où venait l'image `bdelphin/hello-docker` que nous venons d'utiliser ... la réponse est **de DockerHub** !

Comme GitHub pour les projets Git, il existe des dépôts d'images Docker en ligne sur lesquels nous allons pouvoir récupérer des images existantes ou héberger les notres. L'image `bdelphin/hello-docker` que nous avons utilisé y est hébergée ([lien DockerHub](https://hub.docker.com/repository/docker/bdelphin/hello-docker)).

Avant de voir comment ~~construire~~ compiler nos propres images, je vous invite à vous [créer un compte sur DockerHub](https://hub.docker.com/signup) !

## Compiler nos propres images Docker

C'est fait, vous avez un compte sur DockerHub ? Alors c'est parti, compilons notre première image Docker :rocket: La suite par [ici (dockerfile)](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/EnoncEs%20de%20challenges/Pour%20C401_dockerfile.md).

#