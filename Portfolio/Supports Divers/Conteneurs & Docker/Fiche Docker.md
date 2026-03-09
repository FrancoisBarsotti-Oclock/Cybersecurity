# Les fiches récap de l'école O'Clock: Docker

Docker
Docker est une plateforme de gestion de [containers](). Il permet de facilement créer, lancer, distribuer des images et les containers qui en découlent.

### Fonctionnement général
Docker utilise un modèle client-serveur :

* la partie serveur est le Docker Engine (programme dockerd), c’est le runtime d’exécution des images Docker
* la partie cliente est « Docker » (programme docker), c’est l’outil utilisé au quotidien pour créer et gérer les images Docker

>La communication entre le client et le serveur se fait au moyen d’[une API REST](), ce qui rend possible l’utilisation de Docker à travers n’importe quel réseau, pas uniquement en mode local !

### Images
Les images Docker sont des [exécutables]() à destination du Docker Engine. Ces exécutables contiennent typiquement :

* du code source applicatif
* ses dépendances éventuelles
* sa configuration éventuelle
* le runtime applicatif requis pour lancer le code source (par exemple, Apache & mod PHP pour une application PHP, Node.js pour une application JS, Bash pour un script .sh, etc.).

### Containers
Lorsqu’une image Docker est exécutée par le Docker Engine, elle est instanciée sous la forme d’un container, ce dernier ayant son propre cycle de vie, distinct de celui de l’image : démarrage, exécution, arrêt.

>Par analogie avec la programmation orientée objet, une image Docker serait similaire à une classe, et un container à une instance de cette classe. Plus précisément, le Dockerfile est le code source de la classe, l’image Docker en est la version compilée / binaire, et le container est le processus qui s’exécute à partir de cet exécutable binaire.

### Clusters
Enfin, Docker fournit des moyens d’orchestrer ces images et ces containers (typiquement en production), sous la forme d’un cluster, avec Docker Swarm. Par exemple, en cas d’erreur, Docker peut automatiquement relancer l’applicatif, c’est-à-dire ré-instancier l’image pour recréer un nouveau container. Plusieurs images peuvent être utilisées de concert, formant ainsi un stack, qui sera exécuté sous la forme d’une cohorte de containers avec des interdépendances et une communication par réseau.

Les possibilités de Docker sont très larges, et d’autres outils complémentaires ou alternatifs existent pour aller encore plus loin (ex. Kubernetes pour l’orchestration en remplacement de Docker Swarm). Néanmoins, quelques commandes suffisent pour commencer à être productif.

### Principe de base
1. Définir une image Docker (avec un Dockerfile)
2. Construire l’image (avec docker build)
3. Exécuter l’image (avec docker run)
4. Inspecter le container résultant (avec docker ps etc.)
5. Stopper le container (avec docker stop)

## Fiches-récap
[Dockerfile]()
[Commandes utiles]()
[Utilisation des volumes]()
[Utilisation des networks]()
[Docker Compose]()
[Astuces & bonnes pratiques]()

