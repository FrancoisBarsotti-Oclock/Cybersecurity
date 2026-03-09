# Docker : gérer son espace disque

Toutes les images utilisées sont téléchargées et stockées sur le disque dur. Avec le temps, on se retrouve avec énormément d’images, qu’on n’utilise plus forcément (qui utilise l’image php5.4 ?).
Et quand ce ne sont pas les images qui occupent de la place, ce sont les containers…

## Visualiser

Pour voir toutes les images actuellement stockées sur sa machine. Il faut bien prendre le temps de regarder quelles images ne sont plus utilisées pour les supprimer à la main avec la commande `docker image rm <name>`, `docker image ls` ou `docker images`

### Pour voir les containers
`docker ps`

### Pour voir l’occupation du système de fichiers Linux
`df -h`

### Pour voir l’espace disque occupé par Docker
`sudo du -hs /var/lib/docker`

## Libérer de la place

### Nettoyer les images inutilisées (recommandé)
`docker image prune --all`

Pour plus de détails sur [prune](https://docs.docker.com/config/pruning/)

### Nettoye tout le système, y compris les volumes inutilisées (recommandé)
`docker system prune --volumes`

Si besoin :

### Supprimer une image
`docker image rm <name>`
où `<name>` est à remplacer par le nom du repository de l’image à supprimer. Si plusieurs images ont le même repository, utiliser plutôt

`docker image rm <name>:<tag>`

Pour savoir quelles images sont supprimables, il faut regarder dans le `docker-compose.yml` de notre projet en cours quelles sont les images utilisées. Toutes les autres images peuvent être supprimées.

#
