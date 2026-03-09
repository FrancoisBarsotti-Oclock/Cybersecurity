# Les fiches récap de l'école O'clock

## Volumes

Dans Docker, les *volumes* sont des fichiers spécifiques, liés à un ou plusieurs containers, et destinés à gérer les données persistantes indépendamment du cycle de vie du container, c’est-à-dire que ces données ne sont pas détruites ou modifiées quand le container est arrêté/relancé.

On peut considérer les volumes comme des disques dur virtuels.

### Principe de montage

Dans une architecture UNIX (ce qui est le cas de Docker), pour être utilisée, une unité de stockage (physique ou virtuelle) doit être montée, c’est-à-dire qu’on doit lui attribuer un chemin d’accès dans l’arborescence du système.

Par exemple, un disque dur externe, une fois branché et monté, pourra être accessible depuis le chemin `/media/monDisqueDur`.

Pour plus d’infos, cf [la doc d’Ubuntu](https://doc.ubuntu-fr.org/montage).

### Création et utilisation de volumes

Par défaut, lorsqu’on instancie un nouveau container à partir d’une image qui doit conserver des données (par exemple, une image de base de données), Docker crée un ou plusieurs nouveau(x) volume(s) en générant un nom unique.

Exemple :

```apache
# on lance un nouveau container à partir d'une image de mongoDB
docker run mongo:4.0.10

# Dans un autre terminal, on liste les containers
docker ps
> CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
> 54245ec0e05a        mongo               "docker-entrypoint.s…"   4 minutes ago       Up 2 seconds        27017/tcp           youthful_fermi

# Puis on inspecte le container ainsi créé
docker inspect youthful_fermi
> ...
> "Mounts": [
>     {
>         "Type": "volume",
>         "Name": "7a410f3f862b664cc9f5c54b5849877648df5a73d238291939399810d8b3c9a6",
>         "Source": "/var/lib/docker/volumes/7a410f3f862b664cc9f5c54b5849877648df5a73d238291939399810d8b3c9a6/_data",
>         "Destination": "/data/db",
>         "Driver": "local",
>         "Mode": "",
>         "RW": true,
>         "Propagation": ""
>     },
>     {
>         "Type": "volume",
>         "Name": "6e034f13d1ef60a7781d9944155f5b3dec06522ae7d9b71ebcde9fb07c35adaa",
>         "Source": "/var/lib/docker/volumes/6e034f13d1ef60a7781d9944155f5b3dec06522ae7d9b71ebcde9fb07c35adaa/_data",
>         "Destination": "/data/configdb",
>         "Driver": "local",
>         "Mode": "",
>         "RW": true,
>         "Propagation": ""
>     }
> ],
> ...

# On peut aussi lister les volumes existants
docker volume ls
> DRIVER              VOLUME NAME
> local               6e034f13d1ef60a7781d9944155f5b3dec06522ae7d9b71ebcde9fb07c35adaa
> local               7a410f3f862b664cc9f5c54b5849877648df5a73d238291939399810d8b3c9a6
```

Dans cet exemple, on peut constater qu’en instanciant le container, Docker a créé 2 volumes et les a montés dans le container, sur les chemins d’accès `/data/db` et `/data/configdb`.

Afin de manipuler plus facilement les volumes, on peut les créer manuellement et ainsi les nommer :

```apache
docker volume create nomDuVolume
```

Une fois le volume créé, on doit en spécifier le chemin de montage dans le container. Pour cela on utilise l’option `--volume` (ou `-v` pour les feignants). La syntaxe de cette option se décompose de la manière suivante : `volumeOrigine:cheminDansContainer`.

Exemple :

```ruby
# on crée un volume
docker volume create mon-volume-test

# puis on lance une image mongo en montant le volume créé sur /data/db
docker run -v mon-volume-test:/data/db mongo:4.0.10

# on inspecte le container
docker ps
> CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
> b65dc7066246        mongo:4.0.10        "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        27017/tcp           suspicious_diffie

docker inspect suspicious_diffie
> ...
> "Mounts": [
>     {
>         "Type": "volume",
>         "Name": "mon-volume-test",
>         "Source": "/var/lib/docker/volumes/mon-volume-test/_data",
>         "Destination": "/data/db",
>         "Driver": "local",
>         "Mode": "",
>         "RW": true,
>         "Propagation": ""
>     },
>     ...
```

## Commandes utiles

```ps
# Lister les volumes
docker volume ls

# Supprimer un volume (il ne doit être lié à aucun container - même éteint!)
docker volume rm nom-du-volume

# Supprimer les volumes non-utilisés (c-à-d qui ne sont liés à aucun container)
docker volume prune

# Monter un volume en lecture seule (on concatène :ro)
docker run -v mon-volume:/path/to/volume:ro mon-image
```

### Cas particulier : les bind mounts

Notamment lors des phases de développement, il est parfois plus pratique de partager directement un répertoire de la machine hôte. Heureusement, Docker nous permet de considérer les répertoires comme des volumes !

Exemple :

```apache
docker -v /home/db:/data/db mongo:4.0.10
# Le répertoire /home/db de la machine hôte sera automatiquement monté comme un volume, sur le chemin /data/db dans le container !

docker ps
> CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
> d61dc7046246        mongo:4.0.10        "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes        27017/tcp           affectionate_davinci

docker inspect affectionate_davinci
> ...
> "Mounts": [
>     {
>         "Type": "bind",
>         "Source": "/data/db",
>         "Destination": "/data/db",
>         "Mode": "",
>         "RW": true,
>         "Propagation": "rprivate"
>     },
>     ...
```

On constate que le point de montage n’est pas un « volume » mais un « bind » : le container lit et écrit directement dans la machine hôte !

### Utilité des volumes

Les volumes peuvent servir à mutualiser des ressources entre différents containers, et de ce fait offrent une grande palette de solutions dont voici quelques exemples :

* Persister des données lors d’une phase de développement :

    * on build une image à partir de notre code source, et on instancie un container avec un volume créé à la main

    * à chaque modification du code, on va devoir reconstruire notre image. Donc instancier un nouveau container. Sans remonter notre volume, on perd toutes les données à chaque nouveau build !

* Faire accéder plusieurs containers à une base commune de fichiers (par exemple, des avatars utilisateur)

* Externaliser le système de fichier, car un volume peut être monté à travers le cloud

* Et même, partager le code source avec la machine hôte pour ne pas reconstruire l’image à chaque modification lors de la phase de développement ! 

#

