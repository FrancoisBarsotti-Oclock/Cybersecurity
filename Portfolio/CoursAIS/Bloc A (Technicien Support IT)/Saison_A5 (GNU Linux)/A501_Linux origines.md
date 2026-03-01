# 🐧 Session A501. Les origines de Linux.

## Linux

C’est un noyau

Système d’exploitation

Une interface entre le software et le hardware

c'est l'application qui gère le matériel

l'OS fait communiquer les composants software et hardware

c'est un logiciel qui met en relation le matériel et les autres applications

Un système d'exploitation (Operating System en anglais, souvent abbrégé OS) est, en quelque sorte, le logiciel "principal" d'un ordinateur.

N'importe quel ordinateur a besoin d'un OS pour fonctionner, que cet ordinateur soit un serveur, un fixe ou portable, et même un ordinateur spécialisé embarqué dans une machine.

Le système d'exploitation gère les ressources matérielles & les périphériques de l'ordinateur.

Ces ressources matérielles sont mises à disposition aux logiciels installés sur l'ordinateur par l'utilisateur.

![[Pasted image 20251210115703.png]]
## Histoire de Linux

Les années 50

_Manque une info sur cette slide_

![[Pasted image 20251210115751.png]]
Les systèmes d'exploitation étaient à l'époque très simples : ils étaient conçus pour fonctionner sur une machine spécifique, et ne pouvaient effectuer qu’un nombre très limité de tâches (des calculs mathématiques).

En réalité, on ne parle même pas de système d'exploitation sur ces premiers ordinateurs ! Les programmes lances sur la machine manipulaient directement les ressources matérielles de la machine, sans passer par un intermédiaire.

Les ordinateurs ne pouvaient pas communiquer entre eux. Une entreprise qui changeait d'ordinateur devait entrer à nouveau toutes les données nécessaires aux calculs dans la nouvelle machine.

Autre problème : les ordinateurs ne pouvaient être utilisés que par

une seule personne à la fois, l'opérateur.

Cette personne (ou équipe de personnes) chargeait des cartes perforées dans un lecteur puis lançait les calculs. Ces cartes contenaient les instructions d'un programme ou des données.

C'est pour résoudre ce problème d'interopérabilité (communication entre deux systèmes) et pour permettre à plusieurs personnes de travailler sur un même ordinateur que le premier projet de création d'un système d'exploitation "moderne" a vu le jour.

## Multics

MULTIplexed Information and Computing Service.

Le projet Multics a été lance en 1964, conjointement par le MIT, les Laboratoires Bell (AT&T) et par General Electric.

Ce projet a apporté de nombreux concepts novateurs :
* système de fichiers hiérarchique
* temps partagé ([**https://fr.wikipedia.org/wiki/Temps_partag%C3%A9**](https://fr.wikipedia.org/wiki/Temps_partag%C3%A9))
* multitâche préemptif ([**https://fr.wikipedia.org/wiki/Multit%C3%A2che_pr%C3%A9emptif**](https://fr.wikipedia.org/wiki/Multit%C3%A2che_pr%C3%A9emptif))
* multi-utilisateurs
* sécurité

Ces concepts ont été repris dans la plupart des systèmes d'exploitation modernes, **Multics à influence tous les systèmes qui lui ont succédé**.

Multics a été développé sur et pour des machines **GE** (General Electric) **645**, mainframe 36-bits qui a été conçu spécialement pour ce système.
![[Pasted image 20251210115829.png]]
Le premier GE 645 a été livré au MIT en janvier 1967. Plus d’infos par ici : [**http://web.mit.edu/Saltzer/www/multics.html**](http://web.mit.edu/Saltzer/www/multics.html)

Malgré les nombreuses innovations de Multics, il avait également des défauts : sa **taille** et sa **complexité** ne lui permettait que de fonctionner sur des machines très couteuses et imposantes.

Plusieurs chercheurs des laboratoires Bell, frustrés par ces défauts, ont commencé à quitter le projet.

Les **laboratoires Bell** ont fini par se retirer du projet en 1969, lassés par le temps que prenait le projet à être réalisé.

Peu de temps après, en 1970, **General Electric** décide de fermer et revendre leur division informatique à **Honeywell**.

Honeywell a continué à développer Multics et l'ont commercialisé jusqu'en 1985.

Plus de 80 mainframe coutant plusieurs millions de dollars ont été installés chez des industriels, universités (dont certaines universités françaises !) et gouvernements dans ces années-là.

Honeywell fut rachète par Bull à la fin des années 80, qui continuèrent à distribuer Multics jusqu'à l'an 2000 (année au cours de laquelle le dernier système Multics en production fut éteint

définitivement).

## Unix

Unics = Uniplexed Information and Computing Service

Et personne ne se souvient de comment le nom Unics est devenu Unix !

Les derniers chercheurs des Laboratoires Bell a quitter le projet Multics furent Ken Thompson, Dennis Ritchie, Douglas Mcllroy et Joe Ossanna.

Ils décidèrent de mettre à profit l'expérience acquises sur le projet Multics sur un plus petit projet, sans nom et sans soutien financier.

Le nom Unics fut proposé en 1970 par **Brian Kernighan**, un autre chercheur des laboratoires Bell.

La même année, le projet reçu un financement par le biais de la mise à disposition d'une machine **PDP-11/45** (jusque-là, Unix était développé sur **PDP-7**).

Entre 1972 et 1973, **Dennis Ritchie** développa le langage C, conçu pour construire des programmes/utilitaires pour le système Unix.

En 1973, la version 4 d'Unix fut réécrite en C. La version 5, sortie également en 1973, fut la première à être commercialisée à des universités.

Unix fut commercialisé pour la première fois à des entreprises à partir de sa version 6, sortie en 1975.

En 1975, **Ken Thompson** pris un congé sabbatique, et fut invite comme professeur à l'université de Berkeley. Il aida l'université à installer la version 6 d'Unix sur leur machine PDP-11 et travailla sur une implémentation du langage Pascal pour Unix.

Deux étudiant de l'université de Berkeley, **Chuck Haley et Bill** **Joy**, ont par la suite améliore l'implémentation Pascal de Ken Thompson. Ils ont également ajouté au système un éditeur de texte écrit par Bill Joy, appelé ex.

En 1977, Bill Joy commença à compiler cette version enrichie d'Unix, d'autres universités ayant montré un intérêt pour ce projet.

La première version de BSD (Berkeley Software Distribution) fut publiée le 9 mars 1978.

Ce n'était qu'un "add-on" pour Unix version 6, à l'époque.

La famille BSD était née !

D'autres versions d'Unix et de BSD ont suivi, mais on ne va pas y passer 3 jours donc une image fera l'affaire :
![[Pasted image 20251210120055.png]]
Les descendants des premiers systèmes Unix sont toujours utilisés de nos jours :

* MacOS, base sur BSD/FreeBSD.
* FreeBSD, un descendant open-source de BSD, utilisé sur certains équipements réseau (pfSense est base sur FreeBSD !)
* NetBSD, OpenBSD, DragonflyBSD, relativement peu utilisés mais encore maintenus !
* Oracle Solaris
* HP-UX
* IBM AIX
* etc.
## Philosophie UNIX
Les développeurs qui travaillent sur les systèmes UNIX suivent certains principes :

- Écrivez des programmes qui effectuent une seule chose, et qui le font bien.
- Écrivez des programmes qui collaborent.
- Écrivez des programmes pour gérer des flux de texte, car c'est une interface universelle.

On ajoute parfois d'autres principes, comme le principe KISS : Keep It Simple, Stupid (à comprendre dans le sens "ne pas compliquer inutilement les choses").
## POSIX
POSIX est une famille de normes techniques, définie par l'IEEE (groupe IEEE 1003). Ces normes définissent des standards et spécifications pour les éléments de base d'un système d'exploitation :
- ligne de commande
- nombreux utilitaires/programmes de base
- entrées/sorties
- gestion du réseau
- gestion des threads
- etc.

C'est grâce au respect de ces normes que les commandes utilisées sur Unix, Linux, BSD ou MacOS ne différent pas (ou peu), par exemple.
## Projet GNU
GNU is not Unix

Tout a commencé avec une imprimante laser Xerox 9700 :
![[Pasted image 20251210121322.png]]
En 1980, **Richard Matthew Stallman** (souvent appelé **rms**) était développeur au AI Lab du MIT.
![[Pasted image 20251210121349.png]]
Dans ce laboratoire, il y avait une imprimante laser qui occupait un étage entier, loin de là où travaillaient les développeurs.

Cette imprimante était utilisée par plusieurs personnes, qui ne savaient pas si leur document était sorti de la file d'attente ou s'il y avait eu un bourrage papier avant de se déplacer.

C'était une perte de temps importante de devoir retourner à son poste relancer l'impression après un échec.

Pour résoudre ce problème, Richard Stallman a modifié le pilote de l'imprimante pour qu'un message soit envoyé à l'utilisateur une fois son document imprime, ou qu'un message soit envoyé à tous les utilisateurs dans la file d'attente en cas de bourrage papier.

L'histoire aurait pu s'arrêter là, mais le MIT a décidé de remplacer cette imprimante par une Xerox 9700.

Richard Stallman, voulant implémenter cette fonctionnalité sur la nouvelle imprimante, a demandé à Xerox le code source du pilote.

Xerox a refusé.

Cette expérience a convaincu rms de l'importance pour les utilisateurs de pouvoir **modifier/enrichir librement les logiciels dont ils ont besoin** pour travailler.

En septembre 1983, rms annonce qu'il commence a travailler sur un système d'exploitation compatible avec Unix, très populaire à l'époque mais qui avait l'inconvénient d'être **propriétaire**.

On dit d'un logiciel qu'il est "propriétaire" (proprietary en anglais) quand son contrat de licence stipule que seul l'auteur du logiciel a le droit d'y apporter des modifications et de le distribuer.

La "compatibilité" avec Unix était nécessaire pour que les utilisateurs de ce dernier puissent facilement basculer sur le nouveau système développé par Richard Stallman.

Le projet GNU était né !
![[Pasted image 20251210121416.png]]
Le nom **GNU**, acronyme récursif de **GNU is Not Unix**, a été choisi pour symboliser le fait que GNU était comme Unix, sans être Unix.

Contrairement à Unix, le système d'exploitation GNU devait être complètement **libre** (free, en anglais).

Richard Stallman a quitté son travail au MIT (craignant que le MIT s'attribue son travail) et a fondé en 1985 la **Free Software Foundation** (FSF).

**Attention**, le terme **free** ne doit pas être traduit en Français par gratuit, mais par **libre**.

La FSF indique d’ailleurs: Free as in free speech and not free beer.
## Logiciel libre
La notion de logiciel libre était née !

Pour encadrer légalement ces logiciels, rms et la FSF ont créé en 1989 une licence spécifique, intitulée **GNU General Public Licence**, abrégée GNU **GPL**. La dernière version de cette licence est la v3.
![[Pasted image 20251210121450.png#center]]
### Copyleft
On désigne cette licence (et d'autres licences équivalentes) comme une licence copyleft, par opposition à copyright.
![[Pasted image 20251210121529.png#center]]
Le principe ? Vous êtes libres de modifier, partager, redistribuer un logiciel sous licence libre. La seule contrainte est que vos modifications doivent être partagées avec les mêmes droits.

Revenons au projet GNU !

Unix étant un système modulaire, rms et la FSF ont pu développer les composants du système GNU brique par brique.

La première brique que rms a développé a été le compilateur pour langage C, **GNU GCC** (GNU Compiler Collection), disponible dès juin 1984. Il a ensuite rapidement porté son éditeur de texte Emacs, qui n'était jusque-là pas compatible Unix.

Les briques principales du système d'exploitation GNU sont les suivantes :
- GNU GCC, le compilateur
- GNU C libraby (glibc)
- GNU Core Utilities (coreutils)
- GNU Debugger (GDB)
- GNU Binary Utilities (binutils)
- GNU Bash

Toutes ces briques (on reviendra en détail sur certaines) étaient prêtes au tout début des années 90.

Il ne manquait qu'une chose : le noyau.
## GNU Hurd
La production d'un noyau GNU nomme Hurd est lancée en **1990**.

La **dernière version de ce noyau est la 0.9**, sortie en décembre 2016. Il n'est toujours pas considéré comme étant terminé et complètement opérationnel.

Au début des années 90, le système d'exploitation GNU est donc loin d'être utilisable, faute de noyau.

Mais un autre projet open-source va proposer une solution à ce problème : **Linux** !
# Linux
![[Pasted image 20251210121611.png#center]]
Au début des années 90, un étudiant Finlandais du nom de **Linus Torvalds** a acheté un micro-ordinateur de type PC, équipé d'un microprocesseur intel 386, pour se former à cette plateforme.
![[Pasted image 20251210121643.png]]
Ce PC était livre avec MS-DOS, sur lequel Linus a joué quelques jours à Prince of Persia.

Habitué aux systèmes Unix, Linus a rapidement remplacé MS-DOS par le système d'exploitation **Minix** ([**https://fr.wikipedia.org/wiki/Minix**](https://fr.wikipedia.org/wiki/Minix)), créé par **Andrew S. Tanenbaum** pour des fins pédagogiques.

Minix était conçu autour d'un **micro-noyau**, volontairement simple pour que son architecture soit comprise facilement par les étudiants d'Andrew Tanenbaum.

Linus préférait le système Unix qu'il utilisait à l'université, mais ne pouvait pas se permettre de l'acheter pour sa machine personnelle.

Andrew Tanenbaum ne souhaitait pas faire évoluer Minix. Linus a donc décidé de créer **son propre noyau de système d'exploitation**.
![[Pasted image 20251210121718.png]]
_Il manque un commentaire du slide_

Message original :
![[Pasted image 20251210121747.png]]
Ce qui allait devenir le noyau Linux avait une architecture différente de Minix : un noyau monolithique pour Linux, un micro-noyau pour Minix.

Plus d'infos sur les différents types de noyaux par ici : [**https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d%27exploitation**](https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d%27exploitation)

[Voici le PC « parfait » pour Linux selon son créateur — Frandroid](https://www.frandroid.com/produits-android/ordinateurs/pc-fixes/2894615_voici-le-pc-parfait-pour-linux-selon-son-createur)

Pour l'anecdote, Andrew S. Tanenbaum aurait répondu a Linus avec ce message :
![[Pasted image 20251210121813.png#center]]
Un **noyau monolithique** (ou monolithic kernel) est un type d'architecture de noyau où l'intégralité du système d'exploitation (y compris la gestion de la mémoire, les systèmes de fichiers, les pilotes de périphériques, et le réseau) s'exécute dans un seul grand espace d'adressage (l'espace noyau).

Opposé : **Micro-noyau**  
L'architecture opposée est le micro-noyau, où seules les fonctions les plus essentielles (communication, gestion de la mémoire de base) restent dans l'espace noyau. Les autres services (pilotes, systèmes de fichiers) s'exécutent dans l'espace utilisateur, séparément, ce qui améliore la stabilité.

La première version complète fut la **v0.02**, publiée le **5 octobre 1991**. Cette version embarquait déjà plusieurs outils GNU (bash et GCC, par exemple).

En 1992, Linus décide de publier son noyau sous licence GNU GPL, décision qui a permis à la communauté GNU de s'associer à la communauté naissante autour de ce nouveau noyau.

Cette collaboration a donnée naissance au système d'exploitation GNU/Linux.

Encore une anecdote : Linus voulait appeler son noyau **Freax** (contraction des mots free, freak et x pour Unix).

**Ari Lemmke**, qui gérait le serveur FTP sur lequel Linus uploadait son travail, a décidé de renommer le noyau pour l'appeler Linux sans consulter Linus (ce dernier trouvait le nom Linux trop égocentrique).
## Tux
Le manchot (pas un pingouin !) **Tux**, dessine par Larry Ewing, devient la mascotte du projet Linux en **1996**.
![[Pasted image 20251210121843.png]]
La mise à disposition du code du noyau Linux a **suscité beaucoup d'intérêt** auprès de la communauté de Minix et des informaticiens en général.

Des **milliers de développeurs bénévoles** à travers le monde ont contribué et contribuent encore à son développement.

De nombreuses entreprises (Intel, IBM, Valve, Novell, etc.) collaborent aujourd'hui au côté des développeurs bénévoles, et Linus Torvalds est toujours le coordonnateur du projet (il se qualifie lui-même de "dictateur bienveillant").

Le développement de Linux est effectué **exclusivement par Internet** : notamment par des échanges de mails et grâce à l'utilisation de **Git**, développé également par Linus Torvalds.

Le modèle de développement de Linux est considéré comme un **exemple de réussite pour un projet open-source**.

En 2024, le noyau Linux est toujours en développement actif, de nouvelles fonctionnalités et des nouveaux pilotes de périphériques sont implémentés pour suivre l'évolution de l'informatique.

Au moment où j'écris ces lignes, la dernière version du noyau est la 6.10, publiée le **14 juillet 2024**. La version stable est la **6.9.9** (publiée 3 jours avant).

Une particularité des systèmes GNU/Linux : il n'en existe pas qu'un seul !

Il existe de nombreuses distributions GNU/Linux, on voit ça juste après
## Les distributions GNU/Linux

[https://upload.wikimedia.org/wikipedia/commons/9/96/Liste_des_distributions_Linux.svg](https://upload.wikimedia.org/wikipedia/commons/9/96/Liste_des_distributions_Linux.svg)

Pour rappel : **Linux, c'est un noyau**.

Pour disposer d'un système d'exploitation complet, on doit ajouter des éléments à ce noyau.

Ces éléments à ajouter sont notamment les outils GNU !
![[Pasted image 20251210121910.png]]
On pourrait **choisir nous-même chacun de ces éléments** à ajouter un-par-un, mais ce serait une tache très fastidieuse.

Pour nous éviter ce choix, et faciliter l'installation d'un système GNU/Linux, il existe des **distributions**.
### Définition

Une distribution GNU/Linux, c'est un ensemble de logiciels (souvent libres) assemblés autour du **noyau Linux**.

Cet ensemble forme un **système d'exploitation pleinement opérationnel**.

Le terme "distribution" est employé car le principe est de **distribuer** un ensemble de logiciels présélectionnés par les mainteneurs de la distribution.

---------------------------------

**Astuce** : Pour ceux qui ne peuvent pas (ou ne veulent pas) passé à Windows 11, beaucoup sont passé à ZorinOS en distrib linux qui ressemble à Win10

[https://blackarch.org/](https://blackarch.org/) => tellement jolie, la meilleurs distrib pour **Pentesting**. Mais tellement difficile à utilisé

Chercher sur **hyprland**

---------------------------------

Il existe des centaines de distributions GNU/Linux !

Les premières "grandes" distributions publiées sont :
- Slackware (à partir de juillet 1993)
- Debian (à partir d'août 1993)
- Red Hat (à partir de 1994)

Les distributions GNU/Linux sont très souvent publiées avec des licences libres (GNU GPL, en général).

Un logiciel libre pouvant être modifié et redistribué librement, de nombreuses personnes et entreprises ont créé puis distribué leur propre distribution, basée sur une distribution existante.

On se retrouve donc avec des centaines de distributions, plus ou moins pertinentes ...

Pour visualiser les différentes distributions et leurs ancêtres, on peut jeter un œil à la **Linux Distributions Timeline** : [Linux Distributions Timeline](https://upload.wikimedia.org/wikipedia/commons/9/96/Liste_des_distributions_Linux.svg)

-------------------------------

**Note** à part : à savoir sur Kali [Un peu d’histoire sur Kali Linux | IT-Connect](https://www.it-connect.fr/chapitres/un-peu-dhistoire-sur-kali-linux/)

------------------------------

**Grandes familles**
![[Pasted image 20251210121955.png]]
Ce nombre de possibilités entraine une certaine **fragmentation** de l'écosystème Linux.

Selon Linus Torvalds (source), c'est le frein principal à l'adoption de Linux sur les ordinateurs de bureau (seulement environ 3% de parts de marché).

Pour comprendre pourquoi il y a tant de distributions, il faut comprendre ce qui les différencie !

### Différences entre distributions

La différence principale entre les grandes familles de distributions c'est le gestionnaire de paquets (nous y reviendrons) :
- **apt/dpkg** sur Debian, Ubuntu et leurs dérivés
- **pacman** sur Arch et ses dérivés
- **RPM** sur Red Hat, Fedora et dérivés

Une distribution, c'est aussi un certain nombre de programmes préinstallés, comme par exemple l'environnement de bureau, le navigateur web, etc.

On peut, en général, **installer n'importe quel logiciel sur n'importe quelle distribution**. La différence majeure est donc bien le gestionnaire de paquets.

Certaines distributions sont néanmoins un peu plus compliquées à prendre ne main que d'autres. Il y a même des distributions dont l'objectif est justement la simplicité d'utilisation (c'est le cas d'Ubuntu ou de Linux Mint, par exemple).
![[Pasted image 20251210122021.png]]
_Certaines distributions sont plus compliquées que d’autres !_
# Linux : notions de base

Au programme :
- interface en lignes de commande
    * rompt
    * commandes usuelles
    * arguments
    * raccourcis et caractères spéciaux
- système de fichiers
    * arborescence et dossiers usuels
    * chemin relatif ou absolu
- entrées, sorties, redirections
- manipulation de fichiers texte
## CLI (Command Line Interface)

### IHM : CLL vs. GUL
Avant l'apogée des interfaces graphiques (GUI - Graphical User Interface) avec l'arrivée de la souris, l'interface homme-machine (IHM) d'un système informatique était la ligne de commande (CLI - Commande Line Interface).

Sur Windows et MacOS, la CLI est encore présente, mais n'est généralement pas ou peu utilisée par la plupart des utilisateurs.

Sur GNU/Linux, ce n'est pas le cas ! On peut parfois s'en passer dans certains environnements de bureau, mais ce n'est pas tout le temps judicieux : la CLI est souvent **plus efficace** qu'une interface graphique, pas besoin de chercher où cliquer parmi des dizaines de menus et boutons !

La CLI impose en revanche de lire la documentation (RTFM !), ce qui n'est pas forcément le cas pour les GUI.

Une interface graphique est plus intuitive, mais pas nécessairement plus simple que copier/coller une ligne de commande.
## Prompt
Invite de commande, en français
![[Pasted image 20251210154858.png]]
pwd : Découvre le parcours complet vers notre annuaire de travail actuel. Il est facile de perdre de vue où nous en sommes exactement dans le système de fichiers, c'est pourquoi je veux introduire « pwd ».

### Commandes usuelles
Voici quelques commandes fréquemment utilisées (page 1/2) :

- clear ou Ctrl + L : nettoyer les prompts _(commande base)_
- Is (LiSt files) : liste les fichiers et dossiers présents dans un dossier
- pwd (Print Workin Directory) : affiche le chemin absolu du dossier courant
- cd (Change Directory) : changer le dossier courant _(commande base)_
- cp (Copy Directory) : _(commande base)_
- mv (MoVe) : déplacer un ou plusieurs fichiers/dossiers (ou renommer le fichier)
- rm (ReMove) : supprimer un ou plusieurs fichiers _(commande base)_
- rmdir (ReMove DIRectory) : supprimer un ou plusieurs dossiers
- mkdir (MaKe DIRectory) : créer un dossier _(commande base)_
- touch : créer un fichier
- sudo (Super User DO) : lancer une commande en tant que super-utilisateur (root)
- man (**read MANual**) : affiche la documentation (appelée manpage) d'une commande _(commande base)_
- cat : afficher le contenu d'un fichier dans la sortie standard
- less : lire un fichier page par page (alternative : more)
- head : afficher la "tete" d'un fichier (les premières lignes)
- tail : afficher la "queue" d'un fichier (les dernières lignes)
- In (LiNk) : créer un lien vers un fichier
- find : rechercher un ou plusieurs fichiers/dossiers (alternative : locate)
- grep (Global Regular Expression Print) : recherche une chaîne de caractères dans des fichiers ou depuis l'entrée standard, souvent utilisée pour "filter" la sortie d'une autre commande
- shutdown : éteindre l'ordinateur
- reboot : redémarrer l'ordinateur
- df (Disk Free) : afficher l'espace disque libre/utilisé
- free : affiche la mémoire vive (RAM) libre/utilisée
- uptime : affiche la durée de fonctionnement de la machine (depuis le boot)
- uname (Unix NAME) : affiche des infos sur le système
- file : permet d'identifier un fichier à partir de son type MIME

Il faut les apprendre par cœur, toutes ces commandes ?

- pas forcément ! La seule à savoir par cœur c’est **man**.

Un administrateur système GNU/Linux ou Unix connaîtra probablement ces commandes usuelles par cœur.

Mais pas besoin de se forcer : vous vous en souviendrez **naturellement à force de les utiliser** ! En attendant, si vous ne vous souvenez plus d'une commande ou que vous ne savez pas comment faire quelque chose, le plus simple c'est de **chercher sur Internet**.
### Arguments
Les commandes vues précédemment (et la plupart des autres commandes !) peuvent être utilisées avec des arguments.

Ces arguments vont permettre de changer le comportement des commandes, spécifier des paramètres, donner des informations, etc.

Les arguments acceptés sont propres à chaque commande ! Mais certains sont fréquemment implémentés, comme par exemple -h ou - - help pour afficher un message d'aide à l'utilisation de la commande.

Prenons pour exemple la commande ls :

on lui ajoute fréquemment les arguments -a, -l et -h !

*  **-a** permet de lister tous les fichiers et dossiers, y compris ceux cachés (dont le nom commence par un *.* )
* **-l** permet d'afficher plus d'infos sur les fichiers et dossiers (les droits d'accès, notamment)
* **-h** permet d'afficher certaines des infos dans un format facilement compréhensible par l'homme (Human readable)

**Attention**, pour rappel, ces arguments sont uniquement valables pour la commande ls !

Voici la même commande (**ls**), sans et avec l’argument -l (pour changer le comportement de la commande et voir des détails différents)
![[Pasted image 20251210155025.png]]
![[Pasted image 20251210155037.png]]
Les arguments sont renseignés directement après la commande, avant d'appuyer sur entrée pour la lancer. La commande et ses différents arguments sont **séparés par des espaces** (bon, ça dépend).

Pour connaître les arguments disponibles pour une commande, pas le choix, il faut **lire son manuel** (ou une doc en ligne, peu importe).

**Attention** : majuscule et minuscule pourra correspondre à deux choses différentes.

### Raccourcis et caractères spéciaux

On peut utiliser différents caractères spéciaux dans nos commandes :

* **~** (tilde) : correspond au chemin vers le dossier de l'utilisateur courant (par exemple, /home/bob si l'utilisateur s'appelle bob)
*  **.** (point) : correspond au dossier courant (dans lequel on se trouve)
* **..** (deux points) : correspond au dossier parent
* **!!** (deux exclamations): commande précédemment lancée (très utile si vous avez oublié sudo sur une commande : il suffit de lancer sudo ! ! )
* * (wildcard) : permet de remplacer un ou plusieurs caractères
* **{,}** : sélecteur multiple, voir exemple dans la slide suivante

#### Quelques exemples :

- pour lister uniquement les fichiers avec l’extension. txt dans un dossier : **ls *. txt /chemin/vers/dossier**
- pour lister les fichiers dans le dossier parent : **ls .. /**
- pour lister les fichiers avec l’extension. txt ou . docx : **ls { *. txt, *. docx}**

## Système de fichiers
**Notion primordiale** : sous Linux, tout est fichier !

- les périphériques (disques dur, clavier, souris, périphériques USB, etc.) sont des fichiers !
- les processus (programmes en cours d'exécution) sont des fichiers !
- les dossiers/répertoires sont en fait des fichiers aussi !
- et ... les fichiers sont des fichiers, ça on s'en doutait.

Ces différents fichiers sont "rangés" dans des répertoires spécifiques sur le système de fichier de la machine : [Système de fichiers — Wikipédia](https://fr.wikipedia.org/wiki/Syst%C3%A8me_de_fichiers)

### Arborescence et dossier usuels
Un aperçu du système de fichier d'un système GNU/Linux :
![[Pasted image 20251210155110.png]]
![[Pasted image 20251210155121.png]]
**Retenir** : bin, dev, home, media et root.

--------------------------------------------------
La commande `sudo rm -fr /*` cassera la machine de suite : pour la tester, il faudra bien avoir une snapshot de la VM pour récupérer.

----------------------------------------
## Chemin relatif ou absolu
Quand on manipule des fichiers dans l'arborescence d'un système d'exploitation (GNU/Linux ou autre !), on peut cibler un fichier de deux façons différentes :

- par son **chemin absolu** : c'est le chemin vers le fichier **depuis la racine du disque**.
- par un **chemin relatif** : c'est un chemin vers le fichier qui est **relatif au dossier courant**, dans lequel on se trouve actuellement.

Revoir notion

**Note** : **pwd** va toujours nous aider à savoir où on est

**Exemple** de **chemin absolu** :

Pour cibler le fichier test . txt dans le dossier personnel de l'utilisateur Bob, on peut utiliser son **chemin absolu, depuis la racine du système** :

* /home/bob/text.txt (sous GNU/Linux)
* /Users/bob/text. txt (sous MacOS)
* C:\Users\bob\text. txt (sous Windows)

**Attention**, sous GNU/Linux et MacOS, la racine du système de fichier c'est /. Ce n'est pas le cas sur Windows, sur lequel chaque lecteur de disque se voit attribuer une lettre (C : , D : , E : , etc.)

**Attention** **bis** : sous Windows, on utilise das anti-slashs (\) dans les chemins de nos fichiers.

**Exemple** de **chemin relatif** :

Pour cibler le fichier text. txt dans le dossier Documents lui-même dans le dossier personnel de l'utilisateur, alors qu'on se trouve dans le dossier Images, le chemin relatif sera :

* .. /Documents/test. txt (sur GNU/Linux ou MacOS)
* .. \Documents\text. txt (sur Windows)

On remonte d'un cran dans l'arborescence avec **.** **.** !

Chaque commande/programme/exécutable/processus possède une **entrée standard (stdin)** et une **sortie standard (stdout)**.

Il y a aussi une **"sortie" dédiée aux erreurs (stderr).**
![[Pasted image 20251210155227.png]]
La sortie standard et la sortie dédiée aux erreurs peuvent être **redirigées** vers la console, vers un fichier texte ou même vers une autre commande :
![[Pasted image 20251210155251.png]]
### Redirection vers un fichier
On utilise > ou >> pour rediriger la sortie standard d'une commande vers un fichier texte :
![[Pasted image 20251210155318.png]]
### Redirection des erreurs
On peut aussi rediriger la sortie dédiée aux erreurs dans un fichier spécifique (avec 2>), ou dans le même fichier que la sortie standard (avec 2>&1).
![[Pasted image 20251210155346.png]]
C’est très pratique pour débugger !

### Redirection vers stdin
On peut rediriger un fichier vers l'entrée standard (stdin) d'une commande avec < (exemple : cat < file. txt).

Pour lire ligne par ligne une saisie clavier jusqu'au mot FIN, on utilise << FIN (exemple : sort -n << FIN).
![[Pasted image 20251210155413.png]]
### Chaînage de commandes

On peut "chaîner" des commandes, c'est à dire rediriger la sortie d'une première commande vers l'entrée d'une deuxième.

Pour cela, on sépare les commandes par un pipe | (barre verticale, AltGr+6 sur clavier azerty).
![[Pasted image 20251210155438.png]]
Quelques exemples de chaînage de commandes :

- si on veut compter le nombre de fichiers dans un dossier :
     * ls permet de lister les fichiers/dossiers
     * wc (Word Count) permet de compter le nombre de mots (ou de lignes, avec l'argument **- l**)
     * on peut donc utiliser la combinaison de ces deux commandes : ls | wc -l

- si on veut afficher une ligne dans un fichier qui contient un certain motif :
    * **cat** fichier. txt permet d'afficher le contenu d'un fichier
    * **grep** permet de faire une recherche en fonction d'un motif
    * on peut donc utiliser la combinaison de ces deux commandes : cat /etc/sysctl.conf | grep "ip_forward"
### Manipulation de fichiers texte

Créer des fichiers, les copier, les déplacer, les supprimer, c'est bien ! Pouvoir écrire dedans, c'est mieux.

Un peu d'histoire

En 1966, l'un des premiers éditeurs de texte a vu le jour : QED. C'était un éditeur de texte ligne par ligne, l'utilisateur devait taper des commandes pour modifier individuellement chaque ligne d'un fichier.

QED a inspiré Ken Thomson, qui a créé l'éditeur de texte Ed pour Unix en 1971.

**GNU Ed** a été implanté par la suite pour le projet GNU, et est **toujours utilisable sur les systèmes GNU/Linux de nos jours**.

Vous voulez le tester ? Bon courage !

Ed ne sert plus vraiment en tant qu'**éditeur de texte interactif** depuis les années 80, mais peut encore être utilisé dans des scripts shell (de façon non-interactive / automatique).

### Grep, sed & awk
L'éditeur Ed a inspiré la création de plusieurs utilitaires de manipulation de texte :
- **grep** (Global REgular Expression) : crée par Ken Thomson pour Unix en 1974, cet utilitaire permet de faire des recherches de motifs/caractères dans du texte. Son implémentation la plus répandue est GNU grep.
- sed (Stream EDitor) : créé par Lee McMahon en 1974, sed permet d'appliquer des transformations prédéfinies (dans un langage propre, appelé script sed) sur des lignes d'un fichier texte. Lui aussi a été réimplémenté dans le projet GNU.
- **awk** (initiales de ses créateurs Alfred Aho, Peter Weinberg et Brian Kernighan) : un utilitaire et un langage de traitement de ligne de texte, qui permet de faire des opérations de recherche et de transformation complexes.
### ex/vi
En 1976, Bill Joy développe l'éditeur ex sur l'une des premières versions de BSD.

ex & vi (prononcer "vi aïe") ne sont en fait qu'un seul et même éditeur de texte :

·        ex est le mode ligne-par-ligne de l'éditeur

·        vi est le mode visuel de l'éditeur

vi est un éditeur **modal** : il propose **différents modes** (principalement **insertion** et **commande**) ! En fonction du mode sélectionné les touches du clavier ont des fonctions différentes.

vi est devenu l'**éditeur de texte standard d'Unix**, et a été l'éditeur de texte favori de nombreux informaticiens jusqu'à l'arrivée d'**Emacs** en 1984.

vi est encore très utilisé de nos jours : la plupart des distributions GNU/Linux l'embarquent. S'il ne doit y avoir qu'un seul éditeur sur un système, ce sera dans 99% des cas vi. Il est donc très utile de savoir s'en servir !

De nos jours, vi n'est **plus maintenu** (la dernière version date de 2005). De nombreux administrateurs systèmes utilisent donc plutôt Vim (Vi iMproved), qui s'est très largement inspiré de vi en rajoutant des fonctionnalités.
![[Pasted image 20251210155522.png]]
Neovim est également de plus en plus utilisé, c'est un fork (basé sur le même code-source) de Vim.

![[Pasted image 20251210155545.png]]
### Emacs
Emacs est un éditeur de texte dont la première version a été développée par Richard M. Stallman en 1976 pour ajouter des macros à l'éditeur de texte TECO (créé lui-même en 1962 au MIT).

Son nom vient de l'anglais "Editing MACroS".

RMS l'a réimplémente dans le cadre du projet GNU en 1984.
![[Pasted image 20251210155610.png]]
GNU Emacs utilise le langage d'extension **Emacs Lisp**, qui permet la réalisation de **tâches avancées** :

* compilation de programmes
* lecture/écriture de courriers électroniques ou de forums
* navigation sur le web
* etc.

### Guerre des éditeurs
Depuis les années 70, il y a une tradition chez les informaticiens : défendre son éditeur de texte favori avec une passion proche du fanatisme religieux.

Les deux belligérants de cette guerre sont Emacs et Vi.

Cette rivalité est prise au sérieux par certains, tournée en dérision par d'autres.

RMS a par exemple inventé une parodie de religion autour d’Emacs, s’autoproclamant _Saint iGNUcius of the church of Emacs_.
![[Pasted image 20251210155632.png]]
Quelques expressions humoristiques autour de cette guerre des éditeurs :
* L'utilisation d'une implémentation libre de vi n'est pas un péché, mais une pénitence. (RMS)
* Emacs est un très bon système d'exploitation auquel il ne manque qu'un bon éditeur de texte.
* "VI VI VI, le chiffre de la bête !" disent les emacsiens de l'église d'Emacs.
* Vim est un éditeur, il ne cherche pas à inclure "tout sauf l'évier de la cuisine". Mais vous pouvez nettoyer le vôtre avec Vim.

### Nano

**GNU Nano** est un éditeur de texte créé en **1999** par Chris Allegretta.

Il est basé sur la bibliothèque **ncurses**, qui permet de réaliser des pseudo-interfaces graphiques en ligne de commande.

C'est un clone libre de l'éditeur **Pico**, dont il s'efforce de reproduire les fonctionnalités et la simplicité.

Il est fréquemment utilisé par les débutants (et pas que !) et présent par défaut sur de nombreuses distributions GNU/Linux **pour sa simplicité d'utilisation**. Il est beaucoup plus limité que Vim ou Emacs.

-------------------------------

Un jeu en lignes de commandes :  [Terminus](https://luffah.xyz/bidules/Terminus/)

------------------------------ 
