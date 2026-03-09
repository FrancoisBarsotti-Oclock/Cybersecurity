# Docker: Networks

Une des raisons pour lesquelles les containers Docker sont si utilisés, c’est parce qu’on peut facilement les connecter entre eux, sans pour autant mettre en péril la sécurité de la machine hôte.

Bien sûr, pour des raisons évidentes d’architecture, il reste possible de connecter un container Docker à n’importe quel service extérieur même si celui-ci n’est pas « dockerisé ».

Au même titre que le système de fichiers est virtualisé via le concept de [volumes](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/Volumes.md), la couche réseau des containers Docker est elle aussi virtualisé, par l’intermédiaire des *networks*.

Le système de networks de Docker est très modulaire, et s’appuie sur un système de *drivers*.

### Bridge Networks

C’est le type de network par défaut de Docker. Par defaut il existe un réseau du même nom (`brigde`), non supprimable. Lorsqu’on lance un nouveau container sans options particulière, il sera branché sur ce réseau.

On peut par ailleurs créer un réseau personnalisé de type bridge, pour en modifier certaines options, améliorer la sécurité, etc.. Plus de détails [dans la doc (en anglais)](https://docs.docker.com/engine/network/drivers/bridge/)

Ce type de network (utilisant le `bridge driver`) permet de connecter entre eux les containers d’une même instance Docker (i.e. sur la même machine). Les containers auront accès les uns aux autres, ainsi qu’à Internet, mais pas à la machine hôte.

### Host Network

C’est le réseau local de la machine hôte ! En d’autres termes, tous les containers branchés sur le network « host » auront accès à tous les ports de la machine hôte.

C’est le type de network à utiliser lorsque l’on veut accèder à un service « non dockerisé » qui tourne sur la machine hôte (par exemple une DB).

De manière générale, il faut éviter d’utiliser ce type de network car il remet en question le principe d’isolement des containers.

### None Network

Comme son nom l’indique, on peut déclarer un container comme étant totalement isolé du reste du monde.

### Cas d’usage

Un container peut être branché à plusieurs networks simultanément. En utilisant correctement cette fonctionnalité, on peut atteindre un niveau quasi parfait d’isolement entre les différents containers :

![Docker none Network]()

Ainsi, dans cet exemple, le container FrontApp n’a aucun moyen de communiquer directement avec la Database.

L’application frontale étant par définition exposée au public et donc aux attaques, si un hacker arrive à se donner accès à FrontApp, il ne pourra pas directement accéder à la couche sensible des données, qui est totalement isolée dans une autre partie du réseau et dont l’accès est contrôlé & limité.

### Commandes utiles

```apache
# Lister les networks
docker network ls

# Créer un nouveau network de type bridge
docker network create nom-du-network

# Supprimer un network
docker network rm nom-du-network

# Supprimer tous les networks sur lesquels aucun container n'est branché
docker network prune

# Instancier un container en le branchant dans un network
docker run --net=my-network my-image

# Brancher un container sur un network existant
docker network connect my-network my-container

# Déconnecter un container d'un network
docker network disconnect my-network my-container

# Inspecter un network
# Cette commande peut être utile pour trouver l'adresse IP d'un container dans ce network.
docker network inspect my-network
```

#