# Conteneurisation d’applicatifs

Les technologies évoluent vite : n'hésite pas à vérifier que les exemples correspondent toujours aux versions actuelles, via une documentation officielle ou une recherche rapide.

Dernière modification : 09 mars 2026

## Conteneurisation d’applicatifs
Une application, ou applicatif, est un logiciel décrit sous forme de code [exécutable]() qui sera interprété par un runtime (environnement d’exécution).

>Exemple : fichier index.php comportant le code <?php echo "salut"; ?>, lancé avec le runtime php grace à la commande /usr/bin/php index.php.

Les applications, hormis les plus simples, ont toutes :

* des dépendances vers d’autres applicatifs
* des besoins spécifiques qui dépendent de l’environnement d’exécution (development, production…)
* des contraintes de sécurité
* des contraintes d’automatisation (logs, relevé d’erreurs…)
* etc.

Traditionnellement, l’applicatif ne désigne donc pas uniquement le code source, mais aussi toutes ses dépendances, pré-installées (ex. dossier vendors/ en PHP, node_modules/ en JS), ses fichiers de configuration, etc.

À mesure que l’application grossit et se complexifie, cette approche monolithique trouve toutefois ses limites. En particulier, pour un développeur dont le métier est, à la base, d’écrire du code applicatif, toutes les problématiques « opérationnelles » telles que l’installation et la configuration du runtime, la sécurité serveur, l’automatisation du déploiement et du cycle de vie de l’application, etc. représentent une surcharge de travail, avec le risque de mal faire les choses puisque tout ça est en-dehors de son champ d’expertise. Si l’équipe et le budget s’y prêtent, on pourra confier ces tâches à des administrateurs système, mais ce n’est pas toujours possible ni même souhaitable.

Une tendance est donc apparue, consistant à mettre les applicatifs dans des boîtes d’exécution légères, appelées containers. Ces boîtes embarquent une partie des aspects opérationnels, de sorte que le développeur n’est plus seul responsable de cette thématique, et dans le même temps peut plus facilement intervenir sur ces aspects. Le code source sera simplement injecté dans la boîte, pour y être exécuté. Le container, quant à lui, est une unité facile à transférer d’une machine à une autre, à paramétrer, à recycler, etc. — ce qu’on appelle l’orchestration des containers.

>L’analogie entre container logiciel et container physique est donc la suivante : tout comme un producteur de marchandise n’a pas à se soucier des problématiques de transport et peut simplement utiliser le système de containers et de gestion de flotte de bateaux, le développeur de code applicatif n’a pas à se soucier des problématiques d’exécution et de déploiement et peut simplement utiliser le système de containers et d’orchestration.

Dans cette optique, le développeur se concentre sur l’applicatif (le code métier), tandis que l’administrateur système se concentre sur l’orchestration de containers. Le point de jonction, c’est évidemment le container lui-même. On parle de devops : development & operations, c’est-à-dire développement de l’applicatif d’un coté, mise en opération / production de l’autre. Grâce aux nouveaux outils devops, par exemple [Docker](), le travail est simplifié, de sorte qu’une seule et meme personne peut parfois assumer les deux rôles – on parle donc de profils devops pour les fiches de poste.

### Conteneurisation vs virtualisation

Techniquement, un container est un espace-mémoire privé au niveau du système d’exploitation. Il est isolé de l’espace-mémoire commun (dit « espace utilisateur » dans lequel les programmes sont traditionnellement installés), ainsi que d’éventuels autres containers. Dans cet espace privé, on peut installer et exécuter des programmes sans risquer d’envahir les autres espace-mémoires.

Certaines ressources concrètes de l’OS sont toutefois partagées entre tous les containers : mémoire vive, CPU, accès réseau, etc. C’est, entre autre, ce qui distingue la conteneurisation de la virtualisation : cette dernière est une approche plus lourde où même les ressources systèmes sont virtualisées et cloisonnées. La virtualisation consiste donc à exécuter de nombreux OS en parallèles sur une seule et même machine (OS hôte), tandis que la conteneurisation se base sur le partage d’un unique système d’exploitation et isole seulement des processus / logiciels entre eux.

### Docker
Il existe de nombreuses implémentations des containers. La plus connue est Docker. Il s’agit en fait d’une famille d’outils, d’une plateforme basée autour de l’idée de conteneurisation.

[Fiche-récap Docker]()

