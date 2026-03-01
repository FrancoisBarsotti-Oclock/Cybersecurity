# 🐧⌨️ Session 502. Linux Shell.

### Notions du jour

On en a déjà parlé : un système GNU/Linux est construit en assemblant plusieurs briques. Voici les briques principales :

·        Shell

·        Gestionnaire de paquets

·        Chargeur d'amorçage

·        init / gestionnaire de services

·        Éditeur de texte

·         Serveur graphique

·        Pilotes de périphériques

·        Gestionnaire d'affichage

·        Environnement de bureau

·        Gestionnaire de fenêtres

·        Bibliothèques d'interface graphique

·        Explorateur de fichiers

·        Outil de configuration réseau

·        Serveur & sous-système audio

Dans ces slides, on va faire un tour rapide de ces différents composants.

À la fin de la saison, vous aurez un TP sur lequel vous devrez choisir et installer vous-même certains de ces composants !

## Shell (ou Terminal)

Certaines briques sont indispensables dans un système GNU/Linux (ou Unix) : le shell en fait partie.

Le shell, interpréteur de commande en français, est le logiciel qui permet d'avoir une interface homme-machine en ligne de commande

(CLI).

Il existe différents shells disponibles sur les systèmes GNU/Linux.

### Thompson shell & Bourne shell

Le premier shell est le **Thomson shell**, crée en 1971 pour la première version d'Unix par **Ken Thompson**.

Il est remplacé par le **Bourne shell**, créé en **1977** par Stephen Bourne pour la version 7 d'Unix. Il est resté le shell par défaut des systèmes Unix depuis cette époque-là. Ce shell est souvent abrégé bsh et même sh dans de nombreuses versions d'Unix.

Le Bourne shell a apporté la notion de pipes (redictions de la sortie d'une commande sur l'entrée d'une autre avec | ), et a inspiré en conséquence la plupart des shells plus modernes/récents.

### Bash

En **1988**, Brian Fox crée le **Bourne-Again SHell** (bash), pour la Free Software Foundation dans le cadre du projet GNU.

Il est "compatible" avec le Bourne shell (reprend la plupart de ses fonctionnalités), dont il se veut une implementation libre.

C'est le shell par défaut sur la plupart des systèmes libres bases sur Unix, dont la plupart des distributions GNU/Linux, et également sur MacOS jusqu'en 2019. C'est également le shell utilisé par WSL sous Windows.

### zsh

En **1990**, **Paul Falstad**, un étudiant de l'université de Princeton, publie la première version du Z shell, abrégé **zsh**.

On peut le voir comme une **version étendue du Bourne shell ou de bash**. Il reprend également les fonctions les plus pratiques d'autres shells comme ksh (Korn shell) et tcsh (TENEX C shell).

Ce shell est de plus en plus populaire ! Depuis 2019, il remplace meme bash par defaut sur MacOS.

### Autres shells notables

- **csh** : C shell, créé par Bill Joy en 1978, a une syntaxe plus proche du langage C.
- **tcsh** : TENEX C shell, créé par Ken Green et Paul Placeway au début des années 1980. C'est le shell par défaut sur plusieurs versions de BSD, et également de MacOS X, jusqu'à la version 10.2 du système.
- **ksh** : Korn shell, créé par David Korn au début des années 1980. Il est compatible avec le Bourne shell, reprend un grand nombre de fonctionnalites du C shell. C'est le shell par défaut sur OpenBSD.
- **fish** : Friendly Interactive shell, créé par Axel Liljencrantz en 2005. C'est **l'un des rares à ne pas être compatible avec la syntaxe du Bourne shell** ! Il est pensé pour être plus convivial et simple d'utilisation que bash ou zsh, mais attention : beaucoup de scripts bash/shell existants ne fonctionneront pas !

#### Quel shell sur ma distro ?

Pour connaître la liste des shells installes sur une machine GNU/Linux, on peut lancer la commande : cat /etc/shells
![[Pasted image 20251211115843.png]]
Sur un système GNU/Linux (ou Unix !), chaque utilisateur peut utiliser un shell différent.

Vous pouvez connaître le shell utilisé par défaut par votre utilisateur en consultant le fichier /etc/passwd, et en cherchant la ligne avec votre nom d'utilisateur :

cat /etc/passwd | grep baptiste

Remplacez baptiste par votre nom d'utilisateur ! Ce fichier contient la liste des utilisateurs du système, on y reviendra.
![[Pasted image 20251211120100.png]]
Vous pouvez également connaître le shell actif a un instant t en affichant le contenu de la variable d'environnement $SHELL :

**echo $SHELL**
Les variables d'environnement permettent de stocker des données accessibles par différents processus/programmes sur le système.
![[Pasted image 20251211120210.png]]
### Scripts shell
Pour automatiser l'exécution de plusieurs tâches, on peut créer des scripts shell.

Exemple de script basique :

**#!/bin/bash** (shebang), il faudra rentrer dans **nano** pour pouvoir tout copier/coller, pour coller on peut se servir de la souri, si jamais le clavier ne répond pas)
![[Pasted image 20251211120355.png]]
### Lancer un script shell

Copiez-collez le contenu du script d'exemple précédent dans un fichier demo- script. sh (vous pouvez creer ce fichier avec nano ou vim, dans votre dossier personnel par exemple).

Pour lancer ce script, on va utiliser la commande :

./demo-script.sh

Erreur ! Permission non accordée ...

Pour résoudre ce problème de permission, il faut qu'on ajoute le droit d'exécution sur ce script à notre utilisateur.

On peut le faire avec la commande suivante :

**chmod u+x demo-script.sh**
![[Pasted image 20251211120435.png]]
Permission ? Droit ? C'est quoi cette commande ?

On en reparle un peu plus tard !

Exemple :
![[Pasted image 20251211120609.png]]

Mon suivi:
![[Pasted image 20251211120641.png]]
En général, la plupart des scripts sont conçus pour bash, ou le Bourne shell originel. Ces scripts permettent d'utiliser les structures de contrôle élémentaires rencontrées en programmation :

- boucles (while, for, etc.)
- conditions (if, else, else if)
- opérateurs booléens ET et OU

Hé oui, il va falloir apprendre à programmer

Les scripts bash doivent debuter par la ligne #!/bin/bash, qu'on appelle un **shebang**.

Cette première ligne permet au système de savoir avec quel shell lancer le script.

Un gestionnaire de paquets est un outil qui permet d'automatiser le processus d'installation, de desinstallation et de mise a jour d'un programme/application sur un système informatique.

Pendant l'installation d'un paquet (= d'un logiciel/programme/ application), ce logiciel va également installer toutes les dépendances (les autres paquets) nécessaires au bon

fonctionnement du paquet !

Une dépendance ? C'est quoi ça encore ?

On a pas besoin de gestionnaire de paquets, sur Windows !

On va prendre un exemple pour bien comprendre l'interet d'un gestionnaire de paquet : le logiciel Virtual Box.

Pour installer Virtual Box sur Windows, il faut :

- ouvrir un navigateur web, se rendre sur un moteur de recherche
- chercher "virtual box", se rendre sur le site officiel en evitant les sites type 01Net, clubic, etc.
- télécharger la bonne version de l'installateur, adaptée à notre version de Windows (il faut donc connaître notre version de Windows, y compris l'architecture du processeur potentiellement)
- lancer l'installation et suivre le wizard

Pendant l'installation, vous risquez de tomber là-dessus :
![[Pasted image 20251211120711.png]]
C'est ça, une dépendance : un autre logiciel ou composant logiciel dont a besoin un programme/application/logiciel pour fonctionner.

Avec un gestionnaire de paquet, la procédure est plus simple :

- ouvrir un navigateur web, se rendre sur un moteur de recherche
- chercher "virtual box + le nom de votre distribution", pour trouver le nom du paquet (et verifier qu'il existe !)
- lancer une commande dans le terminal, par exemple sudo apt install virtualbox ou sudo pacman -S virtualbox (selon la distribution)

Et voilà

Et encore, on peut même se passer de la CLI : il existe des interfaces graphiques pour la plupart des gestionnaires de paquets.

------------------------------------

Pour ne pas avoir à passer sur internet, on peut télécharger des paquets sur le terminal :

**apt search** virtualbox (il resort la liste de tous les paquets qu’on cherche)

**apt show** virtualbox (ça permet de regarder les détails d’un paquet

**Sudo apt install** virtualbox

-------------------------------------

#### Exemples de gestionnaires de paquets

- dpkg (Debian, Ubuntu et dérivés)
- RPM (Red Hat, Fedora, openSUSE, etc.)
- Pacman (Arch Linux, Manjaro et derivés)
- ports (FreeBSD, OpenBSD, NetBSD, etc.)
- homebrew ou l'Apple Store sur MacOS
- Microsoft Store sur Windows
- Flatpak sur la plupart des distributions Linux
- snap sur Ubuntu

Le Microsoft Store et l'Apple Store ont un fonctionnement un peu différent des gestionnaires de paquets Unix/Linux. homebrew sur MacOS, Chocolatey ou WAPT sur Windows en sont plus proches !

Et sur GNU/Linux, on ne peut pas installer des logiciels "à la main" comme sur Windows ?

Pas vraiment comme sur Windows, mais si, on peut !

Il existe plusieurs solutions pour installer un logiciel sans passer par le gestionnaire de paquets:

- Applmage, un format d'application portable conçu spécifiquement pour Linux
- les applications Java, qui se lancent directement comme sur Windows
- ou alors ... on peut compiler ses propres logiciels à partir du code source !

## Chargeur d'amorçage

**Bootloader**, en anglais

Le chargeur d'amorçage (bootloader) est un logiciel qui permet de démarrer un ou plusieurs systèmes d'exploitation.

C'est le premier logiciel lancé au démarrage d'une machine !

### BIOS vs. UEFL

Pour rappel, le BIOS (Basic Input Output System) c'est le firmware (micrologiciel) embarqué sur la carte mère.

Sur un PC équipe d'un BIOS, celui-ci va lire au démarrage du système les 512 premiers octets de chaque disque (disque dur, cd/dvd, clé USB) connecte. Ces 512 octets sont appelés le Master Boot Record (MBR). Cette zone contient des informations sur l'emplacement du chargeur d'amorçage sur le disque.

De nos jours, sur la plupart des PC, il y a un micrologiciel supplémentaire : **UEFI**. Il ne remplace pas totalement le BIOS (qui reste présent, mais ne gère plus la partie amorçage).

UEFI est un standard technique, issu d'un consensus entre plusieurs industriels.

Avec UEFI, un PC ne lit plus le MBR d'un disque mais sa GUID Partition Table (GPT). On reparlera de ça un peu plus tard !

### Chargeurs d'amorçage populaires

Il existe de nombreux chargeurs d'amorcage libres sur PC, mais les plus populaires sont :

- **GRUB** : GRand Unified Bootloader, dans sa version 2, est probablement le plus utilisé des bootloaders.
- **rEFInd** : fork de rEFIt, uniquement compatible UEFI
- **systemd-boot** : fait partie du projet systemd, uniquement compatible UEFI

Un comparatif est disponible sur le wiki Arch Linux : [Arch boot process - ArchWiki](https://wiki.archlinux.org/title/Arch_boot_process#Feature_comparison)

### init

Si le chargeur d'amorçage est le premier logiciel lancé ... init est le deuxième !

Sur les systèmes Unix et leurs héritiers/successeurs (BSD, GNU/Linux, MacOS, etc.), init est le **premier programme à démarrer après le chargeur d'amorçage**.

Souvent le chargeur d'amorçage n'est pas vraiment compté, on dit qu'init est le premier programme.

Sur un système Unix, les processus (instance d'un programme en cours d'exécution) se voient attribuer un numéro unique qui permet de les identifier : le **PID** (Process ID).

**init possède le PID 1**.

init n'est pas un programme "unique", c'est en fait un type de programme.

Historiquement, il y avait dejà des differences entre le programme init sur Unix System V et BSD. Sur les distributions GNU/Linux, pendant longtemps le programme utilisé était similaire à celui d'Unix System V.

### systemd

systemd est une suite logicielle qui fournit plusieurs outils, dont une partie permet de gérer l'amorçage du système.

Le projet a été lance par Lennart Poettering en 2010 et est publié sous licence GNU LGPL 2.1.

Depuis un peu plus d'une dizaine d'années, la plupart des distributions GNU/Linux ont remplacé le programme init historique par systemd.

Cette décision et le projet systemd dans son ensemble ont suscité une **grande controverse au sein de la communauté GNU/Linux**.

#### Controverse

Selon la philosophie Unix, un programme doit faire une seule chose (et la faire bien). Les programmes doivent respecter le principe KISS : [Principe KISS — Wikipédia](https://fr.wikipedia.org/wiki/Principe_KISS)

Certains utilisateurs reprochent fréquemment à systemd de ne pas respecter ce principe et la philosophie Unix en général.

Lennart Poettering était connu pour avoir développé PulseAudio, un autre projet déjà peu apprécie par la communauté : [PulseAudio — Wikipédia](https://fr.wikipedia.org/wiki/PulseAudio)

La polémique autour de systemd a été si vive quand plusieurs distributions ont décidé de l'adopter, que Lennart Poettering a même reçu des menaces de mort. Linus Torvalds en personne a dû appeler la communauté à l'apaisement.

Pour certaines distributions ayant adopté systemd, il y a eu un fork :

- Devuan pour Debian
- Artix Linux pour Arch Linux
- et sûrement d'autres !

De nos jours quasiment toutes les distributions utilisent systemd, il est incontournable.

### Gestion des services avec systemd

Quelques commandes usuelles :

- lister les services : systemctl -- type=service
- démarrer un service : systemctl start nom_service
- stopper un service : systemctl stop nom_service
- avoir des infos sur un service : systemctl status nom_service
- lancer automatiquement un service au boot : systemctl enable nom_service (et disable pour le désactiver)

On verra un peu plus tard comment créer nos propres services !

## Éditeur de texte

Faut-il vraiment revenir dessus ?

On en a déjà parlé !

Voici une liste des éditeurs populaires :

- **nano**
- **vi** / **vim** / neovim
- emacs

Vous ~~devez~~ _devriez_ savoir utiliser ceux **en gras** !

La plupart des éditeurs de code avec interface graphique (vscode, pycharm, IntelliJ, IDE arduino, etc.) sont disponibles sur Linux.

--------------------------------

Pour sortir de **Vim :   :q !**

---------------------------------

## Serveur graphique

### X Window System
[X Window System — Wikipédia](https://fr.wikipedia.org/wiki/X_Window_System)
X Window System, souvent abrégé X11, est un protocole client/ serveur qui permet aux développeurs de créer des interfaces graphiques pour leurs applications.

Le protocole est à sa 11ème version (d'où le nom X11) depuis 1987. Il n'a pas évolué depuis !

Ce protocole, comme tout protocole, définit des spécifications que doivent respecter les applications qui implémentent ce protocole.
![[Pasted image 20251211121056.png]]
Sur ce schéma, les blocs X client représentent nos applications avec interface graphique. Ces applications vont communiquer avec un serveur qui implémente ce protocole.

### lmplémentations X11

En général, sur GNU/Linux, il y a toujours des alternatives à une brique du système.

X11 est la seule exception : il n'existe qu'un seul serveur qui implémente de protocole : X.Org

C'est un fork de XFree86 qui date de janvier 2004. Le projet X.Org est géré par la fondation du même nom.

### Wayland

Faute d'avoir une alternative a X.Org, il existe une alternative au protocole X11 : Wayland.

Wayland n'a pas la notion de "serveur" comme X11, le logiciel nécessaire pour utiliser Wayland s'appelle un compositeur (compositor en anglais). Il en existe un grand nombre de compositors : [Wayland - ArchWiki](https://wiki.archlinux.org/title/Wayland#Compositors)

#### Inconvénient de Wayland

Le projet Wayland date de 2008, mais son adoption reste encore timide.

Pourquoi ? Parce que chaque application doit être recodée pour être compatible avec Wayland ! Pour éviter ça, Xwayland (serveur X11 embarqué dans Wayland) a été développé.
![[Pasted image 20251211121145.png]]
--------------------------------
Pour savoir sur lequel est notre ordi, on peut faire cette commande :

`echo $XDG_SESSION_TYPE`
![[Pasted image 20251211121900.png]]
------------------------------
**Rappel** : Protocole Ensemble de règles qui définissent comment des systèmes communiquent entre eux Spécifications théoriques, ce sont juste "des règles" sans aucune implémentation concrète
### X11
Protocole qui définit comment les clients interagissent avec le serveur graphique
- ce que les clients peuvent demander au serveur
- comment le serveur doit répondre

X11 est historique, avec une architecture ancienne et assez complexe, avec un serveur central qui gère tout.
### X.org
Serveur graphique qui implémente le protocole X11 et qui donc applique factuellement le protocole
- Il gère l'écran, le clavier, la souris
- Il dessine les fenêtres, les resize, etc
- C'est lui qui reçoit les requêtes des applications clientes et qui y répond en redessinant les fenêtres, en gérant les périphériques, ..
### Wayland
Autre protocole qui définit les interactions entre un serveur graphique et ses

Clients. On n'utilise plus un serveur mais un compositeur :
- Au lieu de recevoir des requêtes et de tout dessiner comme le fait X.Org, le compositeur reçoit des clients des buffers déjà rendus
- Il a juste à assembler, "composer" ces buffeurs et il affiche le tout

Approche plus moderne : plus sécurise, moins de latence, plus simple. Chaque application gère son rendu et le compositeur organise juste le rendu final.
### Compositeur
Couche logicielle qui implémente le protocole Wayland . Il en existe plusieurs, ils ont le même rôle: fournir une solution qui implémente le protocole Wayland
#### Problème
- Wayland est jeune
- L'informatique ça existe depuis
- Y'a un paquet de logiciels qui n'implémentent pas le protocole Wayland
- Et qui ne peuvent donc pas fonctionner avec ce serveur graphique
### XWayland
XWayland sert à pouvoir gérer les logiciels qui utilisent X11 et pas Wayland.

Il "émule" un serveur X11 qui fait ce que fait X.Org. Puis il donne tout au compositeur dans un  langage qui lui permet maintenant de comprendre ces fenêtres.
## Pilotes de périphériques

Comment ça marche, sous Linux ?

Un pilote de périphérique est un programme qui permet à des applications utilisateur d'accéder à des ressources matérielles.

Avec Linux, ces pilotes doivent être chargés directement dans le noyau. Jusqu'à la version 1.2 du noyau, le code source de tous les pilotes disponibles étaient directement embarqués dans le noyau, que le périphérique soit présent ou pas.

Depuis, les pilotes sont charges de manière dynamique sous la forme de modules du noyau.

### Installer un pilote

Sans trop rentrer dans les détails, ce qui nous intéresse, c'est comment installer des pilotes. Plusieurs cas de figure :

- **le périphérique est une carte graphique** : il faut se reporter à la documentation de votre distribution. Il existe des pilotes propriétaires, fournis par AMD ou Nvidia, et des pilotes libres.

- **le périphérique est relativement ancien et assez populaire/répandu** : il y a de fortes chances qu'il n'y ait rien à installer ! Le pilote est déjà dispo sur le système.
- **le périphérique est très spécifique, ou très récent** : il faudra chercher sur le net avec les références du périphérique + le mot clé "linux". Toujours voir sur reddit et github ou [Les forums - LinuxFr.org](https://linuxfr.org/forums)
## Gestionnaire d'affichage
Display manager, en anglais. Aussi appelé login manager.

C'est le logiciel qui vous permet de vous connecter avec votre nom d'utilisateur et votre mot de passe.

Il en existe plein ! Une liste est dispo sur le wiki Arch Linux: [Display manager - ArchWiki](https://wiki.archlinux.org/title/Display_manager)

Ce composant est optionnel : un utilisateur peut aussi démarrer le serveur X.Org (et donc l'interface graphique) en lançant la commande startx.
## Environnement de bureau
Desktop Environment, en anglais, souvent abrégé DE.

L'environnement de bureau est un ensemble de programmes qui permet d'implémenter la *métaphore du bureau* (un peu de lecture par ici pour les curieux : [Desktop metaphor - Wikipedia](https://en.wikipedia.org/wiki/Desktop_metaphor)).

Un environnement de bureau propose **tous les composants nécessaires pour utiliser un ordinateur via une interface graphique** : un bureau, un explorateur de fichiers, des icônes, un thème / une bibliothèque pour les fenêtres, des applications pour configurer le fond d'écran, l'écran de veille, d'autres paramètres basiques, etc.

Sur Windows ou MacOS, cet environnement ne peut pas être changé, il n'existe pas d'alternative.

Sur GNU/Linux en revanche, il en existe plein ! Les plus connus sont :
- Gnome (bibliothèque GTK)
- KDE Plasma (Qt)
- XFCE (GTK)
- LXDE (GTK)
- LXQt (Qt)
- Mate (GTK)
- Cinnamon (GTK)
- Budgie (GTK)
On peut facilement installer un nouvel environnement de bureau !
Pour l'utiliser, il suffit ensuite de le sélectionner via une liste déroulante sur notre Display Manager.
## Gestionnaire de fenêtre
Window Manager, en anglais, abrégé **WM**.

Le gestionnaire de fenêtre est souvent un composant d'un environnement de bureau (mais pas nécessairement).

C'est le programme qui gère le placement et le comportement des fenêtres des applications graphiques.

Quelques WM connus :
- Compiz
- i3
- dwm
- Awesome
- Fluxbox
- Window Maker
Différence entre un gestionnaire de fenêtres et un serveur graphique :
- **Serveur graphique** (ou _Display Server_) : c’est la **couche de base** qui gère l’affichage à l’écran, les entrées clavier/souris, et la communication avec le matériel graphique. Il fait le lien entre le noyau Linux et les applications graphiques.→ Exemples : **Xorg (X11)**, **Wayland**.
- **Gestionnaire de fenêtres** (_Window Manager_) : il s’exécute **au-dessus** du serveur graphique. Il contrôle **l’apparence et le comportement** des fenêtres : placement, bordures, raccourcis, transitions, etc.→ Exemples : **i3**, **Openbox**, **Hyprland**, **KWin**, **Mutter**.

En résumé :
Le **serveur graphique** fait apparaître l’image à l’écran,  
Le **gestionnaire de fenêtres** organise **comment** ces images (fenêtres) sont disposées et interagissent (comme la distribution de fenêtres sur Win11, elle gère les fenêtres et c’est aussi un client du serveur graphique). Le serveur graphique affiche les fenêtres le gestionnaire contrôle leur apparence et leur organisation.
## Bibliothèque d'interface graphique

GTK (écrit en C) ou Qt (écrit en C++)

GTK et Qt sont deux bibliothèques majeures pour créer des interfaces graphiques (GUI) multiplateformes, mais elles diffèrent sur plusieurs plans : philosophie, langage, performances, intégration, licence, écosystème, etc. Voici les différences principales :

1. Langage et conception
GTK
- Écrit en C, avec une conception orientée objet via GObject.
- Dispose de liaisons pour de nombreux langages : Python (PyGObject), JavaScript (GJS), Rust, Go, etc.
- Style plus « C procédural » même si l'architecture est objet via GObject.

Qt
- Principalement écrit en C++, avec un système objet étendu (métaclasses, signaux et slots).
- Dispose aussi de liaisons pour Python (PySide / PyQt), Rust, Go, etc.
- Conception plus moderne orientée objets.

2. Philosophie et écosystème
GTK
- Projet du monde GNOME et de l'écosystème Li desktop.
- S'intègre naturellement avec les environnements GNOME, XFCE, Cinnamon, etc.

Qt
- Orientée objet, tout-en-un

En résumé :

- **GTK** : plus simple, plus “GNOME”, bon pour projets légers.
- **Qt** : plus complet et puissant, idéal pour applications complexes (KDE, logiciels multiplateformes).
[Qt : un framework dédié au développement multiplateforme](https://www.d-booker.fr/content/43-qt-une-bibliotheque-dediee-au-developement-multiplate-forme)
[List of Qt Examples](https://qt.developpez.com/doc/5.12/all-examples/)
[Qt for Beginners - Qt Wiki](https://wiki.qt.io/Qt_for_Beginners)
## Explorateur de fichiers
On peut les avoir en GUI ou en CLI
### GUI (Graphical User Interface)

interface **graphique** avec fenêtres, boutons, menus et icônes (confort visuel).
- dolphin (KDE)
- konqueror (KDE)
- krusader (KDE)
- nautilus (Gnome)
- gnome commander (Gnome)
- thunar
- nemo
### CLI (Command Line Interface)
interface **en ligne de commande**, où tu tapes des instructions textuelles (puissance et contrôle).

- ranger
- midnight commander
- Yazi : un **gestionnaire de fichiers en ligne de commande** (CLI file manager) écrit en **Rust**, très rapide et moderne.  
    Il ressemble à **ranger** ou **nnn**, mais avec une interface plus fluide et une meilleure intégration avec les terminaux modernes (comme ceux qui supportent les aperçus d’images, vidéos ou PDF).
## Outil de configuration réseau
- doc arch : [Network configuration - ArchWiki](https://wiki.archlinux.org/title/Network_configuration#Network_managers)
- netplan ubuntu : [netplan [Wiki ubuntu-fr]](https://doc.ubuntu-fr.org/netplan)

------------------------------------
**Note** :
![[Pasted image 20251211154917.png]]
----------------------------------
## Serveur & sous-système audio

- doc arch :

[utilisateurs:darkjam:son [Wiki ubuntu-fr]](https://doc.ubuntu-fr.org/utilisateurs/darkjam/son)

Un **serveur audio** est un programme central qui gère, distribue et mélange les flux sonores entre les applications et le matériel audio (ex. : **PulseAudio**, **PipeWire**, **JACK**).  
Un **sous-système audio**, lui, désigne l’ensemble de la **pile logicielle et matérielle** permettant la gestion du son dans un système d’exploitation (ex. : dans Linux, le sous-système audio inclut **ALSA** (commande **alsa mixer**), les pilotes, et parfois le serveur audio au-dessus).

En résumé :

- **Sous-système audio** = fondation (pilotes + API bas niveau).
- **Serveur audio** = couche de gestion au-dessus (mixage, routage, volumes, etc.).

----------------------------------

Un **jeu** en **lignes** de **commandes** : [Terminus](https://luffah.xyz/bidules/Terminus/)

**Jeu** pour apprendre **VIN** [Learn VIM while playing a game - VIM Adventures](https://vim-adventures.com/)

-----------------------------------
