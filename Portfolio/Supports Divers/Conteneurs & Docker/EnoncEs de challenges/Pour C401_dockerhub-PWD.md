# DockerHub & PlayWithDocker

> Et si on veut utiliser notre image sur une autre machine ? La partager à nos collègues développeurs ?

Nous allons uploader / **pousser** cette image sur DockerHub ! (vous remarquerez l'emploi du verbe **pousser**, et oui, c'est comme pour GitHub !)

## Uploader l'image sur DockerHub

Rendez-vous sur [vos dépôts](https://hub.docker.com/repositories), et cliquez sur le bouton **Create Repository**.

Choisissez un nom pour votre dépôt, par exemple `my-hello-docker`, et cliquez sur **Create**. Vous l'avez peut-être remarqué, sur la droite DockerHub nous donne la procédure à suivre pour pousser une image sur ce dépôt :

```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

**Docker & DockerHub fonctionnenent comme Git & GitHub** : nous avons des dépôts sur un serveur distant (DockerHub/GitHub), sur lesquels nous allons héberger nos images, mais aussi une sorte de **dépôt local** sur notre hôte, pour stocker les images que nous compilons et celles que nous récupérons depuis DockerHub.

Vous pouvez voir les images que vous avez en local sur votre machine en tapant la commande `docker image ls`. Vous pouvez supprimer une image locale avec `docker image rm <nom_image>` ou `docker image rm <id_image>`. Vous pourrez trouver de la doc pour toutes les commandes de manipulation d'images [ici](https://docs.docker.com/engine/reference/commandline/image/).

On peut retrouver dans la liste des images locales notre image `my-hello-docker`, et on peut remarquer qu'elle a un tag `latest`, c'est le tag par défaut.

Pour pousser cette image sur notre dépôt DockerHub, nous devons d'abord nous connecter à notre compte DockerHub depuis notre terminal, avec la commande `docker login -u <user_dockerhub>`.

Une fois connecté, on peut lancer les commandes suivantes :

```bash
docker tag my-hello-docker:latest <user_dockerhub>/my-hello-docker:latest
docker push <user_dockerhub>/my-hello-docker:latest
```

:warning: Pensez bien à remplacer `<user_dockerhub>` par votre nom d'utilisateur sur DockerHub.

Si vous retournez sur la page de votre dépôt DockerHub, vous devriez voir que votre image vient bien d'être poussée :tada:

## Tester notre image sur une autre machine - PlayWithDocker

Pour tester notre image Docker, nous allons utiliser le service Cloud [PlayWithDocker](https://labs.play-with-docker.com/).

Commencez par vous y connecter avec votre compte Docker/DockerHub, cliquez ensuite sur **Start**. Vous allez arriver sur une page avec un chrono : une fois les 4h écoulées, tout ce qu'on aura fait sur PlayWithDocker sera détruit/perdu (ce qui en fait donc un bon outil de test, mais pas plus !).

Cliquez sur **+ add new instance** sur la gauche. Un terminal va apparaître !

:question: Que vient-il de se passer ? Quand on a ajouté une nouvelle instance, PlayWithDocker a en fait démarré un conteneur Docker et nous donne la main dessus. Ce conteneur fait tourner une image du système d'exploitation **Alpine Linux**, sur laquelle Docker est préinstallé.

Lancez dans ce terminal la commande `docker run -dp 8888:80 <user_dockerhub>/my-hello-docker` (pensez bien à remplacer `<user_dockerhub>` par votre nom d'user DockerHub).

Une fois l'image récupérée et lancée (vous pouvez le vérifier avec `docker ps` pour rappel), vous devriez voir apparaître un bouton **8888** en haut, à coté du bouton **Open port**. Cliquez dessus !

Vous devriez pouvoir admirer notre hello world :tada:

## Et maintenant ?

Vous avez maintenant quelques premières bases sur l'utilisation de Docker :partying_face:

Il nous reste encore plein de choses à découvrir, je vous rassure :smiling_imp: mais ce sera dans un prochain exo !

#