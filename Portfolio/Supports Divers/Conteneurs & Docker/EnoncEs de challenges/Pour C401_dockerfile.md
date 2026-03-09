# Compiler nos propres images Docker

> Utiliser des images existantes c'est bien, mais comment faire pour "dockeriser" notre code à nous ?

Il y a deux solutions pour exécuter notre code à l'intérieur d'un conteneur Docker, la première c'est de compiler (construire, en quelque sorte) notre propre image contenant le code de notre application ! (et la deuxième solution, c'est le point de montage qu'on a vu en cours).

## Cloner le dépôt

Clonez ce dépôt sur votre machine Windows ou sur votre VM (pour le cloner sur la VM, il faudra installer git et le configurer...)

## Dockerfile

Pour compiler une image Docker, nous allons avoir besoin de créer un fichier `Dockerfile` (sans extension, avec un 'D' majuscule). Si vous regardez les fichiers de notre projet VSCode, vous devriez voir ce fichier !

Il contient les instructions suivantes :

```
FROM php:7.2-apache
WORKDIR /var/www/html
COPY src .
```

La première instruction `FROM php:7.2-apache` indique à Docker que nous allons baser notre image sur une image existante : `php:7.2-apache`. Le nom de l'image c'est `php`, et après le `:` on vient préciser le **tag** de cette image que l'on souhaite utiliser. Les tags sont des "versions" des images, et dans notre cas la version `7.2-apache` est une image qui embarque PHP en version 7.2 avec le serveur web apache préconfiguré.

On peut retrouver tous les tags d'une image ainsi que de la documentation sur sa page DockerHub, exemple [ici](https://hub.docker.com/_/php) avec php.

La deuxième instruction `WORKDIR /var/www/html` permet de changer le dossier dans lequel on se trouve **à l'intérieur de l'image** (c'est comme si on lançait la commande `cd /var/www/html`). On indique ici qu'il faut se positionner dans le dossier `/var/www/html`, dossier "racine" du serveur web apache.

La dernière instruction `COPY src .` effectue, comme son nom l'indique ... une copie ! C'est comme si on avait lancé la commande `cp src/* ./`, on demande à docker de copier le contenu du dossier `src/` (sur notre hôte !) dans un dossier à l'intérieur de l'image. Le dossier de destination `./` correspond au **dossier courant**, c'est à dire `/var/www/html`, vu qu'on vient de s'y déplacer avec l'instruction précédente.

Nous serons amenés par la suite à ajouter d'autres instructions à nos fichiers Dockerfile, si vous êtes curieux la [documentation du fichier Dockerfile](https://docs.docker.com/engine/reference/builder/) est très complète.

> Ok c'est très intéressant tout ça, mais on a toujours pas compilé l'image !

## Compiler avec `docker build`

Une fois le fichier `Dockerfile` configuré, on peut lancer la compilation de l'image avec la commande suivante : `docker build -t my-hello-docker .` ! La commande `docker build` prend au moins un argument, l'emplacement du fichier `Dockerfile`. Dans notre cas, il est dans le dossier courant, on peut donc spécifier `.` comme emplacement.

L'argument `-t my-hello-docker` permet de "tagger" l'image, de lui donner un nom. Ce nom nous permettra de la retrouver facilement dans les différentes images présentes sur notre hôte.

Si ce n'est pas déjà fait, je vous invite donc à compiler cette image et démarrer un conteneur à partir de cette nouvelle image ! (cherchez un peu comment faire, et si besoin, le spoiler ci-dessous donne la solution)

<details>
  <summary>Comment lancer notre nouvelle image ?</summary>
  
  Pour démarrer un conteneur à partir d'une image, on va utiliser la même commande que tout à l'heure : `docker run`.

  Il faut préciser les mêmes arguments, et simplement remplacer le nom de l'image `bdelphin/hello-docker` par `my-hello-docker`.
  
  <details>
    <summary>Toujours pas réussi ?</summary>
  
  La commande à lancer est `docker run -dp 8888:80 my-hello-docker` !
  
  </details>
</details>

Notre première image Docker compilée par nos soins devrait fonctionner :tada:

## Et après ?

Ce n'est pas fini, on va maintenant uploader notre image sur le DockerHub et la tester sur une autre machine ! Ça se passe par [ici]().

#