# Docker : découvrons les bases :muscle:

Prêts à pratiquer un peu ?

Ce premier exercice va vous guider pour installer Docker sur votre machine et vous permettre de vous familiariser avec les commandes usuelles et fonctionnalités de base de Docker.

## Installation

Première étape, il faut installer Docker sur votre poste de travail !

Vous pouvez aussi choisir de bosser sur la VM sur laquelle on a installé Docker ce matin.

### Sur VM

Rien à faire, Docker est déjà installé.

### Instructions pour Windows

Docker sur Windows est un peu plus complexe à utiliser/installer. Si vraiment vous n'avez pas accès à une VM Cloud / au téléporteur, suivez les instructions ci-dessous.

Ouvrez un terminal PowerShell **en tant qu'administrateur** et lancez la commande suivante : `wsl --install`.

Vous pouvez vérifier que WSL2 a bien été installé en lançant la commande `wsl -l -v`.

Téléchargez ensuite [ce fichier](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe), et lancez-le. Suivez les instructions d'installation, et sélectionnez WSL2 si on vous demande de choisir avec Hyper-V.

Pour vérifier que l'installation s'est bien passée, vous pouvez lancer la commande `docker run hello-world` dans un terminal. Vous devriez voir le message "Hello from Docker!".

## Exercice : bases de la CLI Docker

Maintenant que nous avons installé Docker, il est temps de découvrir comment s'en servir ! La suite par [ici]().

#
