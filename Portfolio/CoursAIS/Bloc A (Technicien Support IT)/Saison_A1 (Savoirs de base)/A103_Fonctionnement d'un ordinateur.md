# 🖥️ Session A103. Fonctionnement d'un ordinateur

### Comment fonctionne un ordinateur ?
Un ordinateur (computer, en anglais) est un **système programmable de traitement de l'information**, qui fonctionne par la lecture séquentielle d'un ensemble d'instructions (programme informatique), qui lui font exécuter des **opérations logiques et arithmétiques**.

La notion d'ordinateur a été formalisée mathématiquement par **Alan Turing** en 1931. Les premières implémentations commencent à émerger dans les années 40, suite notamment aux travaux de **John von Neumann**.

Le mot "ordinateur" a été introduit en France par IBM dans les années 50. La traduction littérale de computer, calculateur ou calculatrice, est plutôt réservé aux machines scientifiques.

## Types d’ordinateurs
Les ordinateurs modernes que nous utilisons s'appellent des **ordinateurs personnels** (PC, _Personnal Computer_, en anglais).

On rencontrait parfois avant le terme de **micro-ordinateur** ou d'**ordinateur individuel**, mais ces termes sont tombés en désuétude.

Il existe différents "types" d'ordinateurs personnels :
* Ordinateur de bureau, ou ordinateur fixe
* Ordinateur portable
* Ordinateur à carte unique

### Ordinateur fixe 🖥️​
Un **ordinateur de bureau** ou **ordinateur fixe** (_desktop computer_, en anglais) est un ordinateur destiné à être utilisé sur un bureau ou tout autre endroit "fixe", à cause de ses dimensions qui le rendent difficilement transportable.

Il est composé (en général) d'une **unité centrale**, d'un (ou plusieurs) écran(s), ainsi que de **périphériques** **d'entrées**/**sorties** (clavier, souris, haut-parleurs, imprimante, etc.).

Plus d'infos par [Wikipedia, ici](https://fr.wikipedia.org/wiki/Ordinateur_de_bureau).

![01-Desktop]()

_On distingue sur cette photo l'unité centrale à gauche, un écran ainsi qu'un clavier et une souris._

Pour plus d'information, [Wikipedia en a bien plus à nous dire](https://fr.wikipedia.org/wiki/Ordinateur_de_bureau)

### Ordinateur portable 💻​

Un **ordinateur portable** (_laptop_, en anglais), est un ordinateur personnel dont le poids et les dimensions limitées permettent un transport facile.

Un ordinateur portable est muni des mêmes types de composants qu'un ordinateur de bureau classique : un écran, un clavier, un périphérique de pointage (trackpoint et/ou trackpad), de haut-parleurs, etc.

![02-laptop]()

_Un laptop_

Ce que [Wikipedia peut nous dire sur l'ordi portable](https://fr.wikipedia.org/wiki/Ordinateur_portable)

Il existe différents types d'ordinateurs portables :
* ***tablettes** : ordinateur portable ultraplat sans clavier, composé uniquement d'un écran tactile.
* **netbook** (_miniportable_, en français) : ordinateur portable de très petite taille (écran de 10"), moins performant et plus abordable - désuet de nos jours !
* **ultrabook** (_ultraportable_, en français) : ordinateur portable de petite taille (écran de 12, 13 ou 14"), aussi performant qu'un portable classique
* ordinateur portable de taille "classique" (écran de 15" ou 17")

### Ordinateur à carte unique ​🍒​
Un ordinateur à carte unique (parfois abrégé SBC, de l'anglais _Single-Board Computer_) est un ordinateur complet construit sur un seul circuit imprimé.

Rarement utilisés comme ordinateurs personnels, ils servent en général plutôt pour de **l'informatique** **embarquée** (à l'intérieur d'une machine ou d'un appareil).

![03-raspberry]()

_Plusieurs modèles de Raspberry Pi, un ordinateur à carte unique très populaire._

### Autres types d'ordinateurs
* Serveur
* Ordinateur embarqué / ordinateur de bord
* Ordinateur quantique
* Super-calculateur

On ne peut pas vraiment parler d'ordinateurs personnels pour ceux-là ! On reparlera des serveurs un peu plus tard.

## Hardware vs. Software
Un ordinateur, pour fonctionner, a besoin de deux éléments :

* **partie matérielle, hardware** en anglais : les différents composants et périphériques qui constituent l'ordinateur.
* **partie logicielle, software** en anglais : les programmes, applications, le système d'exploitation qui permettent d'utiliser le hardware pour réaliser les tâches que nous souhaitons réaliser avec l'ordinateur.

## Composants d'un ordinateur 🧩

Tout ordinateur, quel que soit son type, a besoin de certains composants pour fonctionner :

* le **processeur** (**CPU**, Central Processing Unit en anglais) : le cerveau de l'ordinateur, il traite les instructions des programmes et effectue les calculs
* la **mémoire morte** (**ROM**, Read-Only Memory en anglais) : là où sont stockées les données et les programmes !
* la **mémoire vive** (**RAM**, Random-Access Memory en anglais) : la mémoire de "travail", ne conserve que les données quand l'ordinateur est allumé.

D'autres composants peuvent être ajoutés, on en reparle très bientôt.

## Périphériques
Un périphérique informatique est un **dispositif connecté à un ordinateur**, permettant de lui ajouter des fonctionnalités.

### Types de périphériques

Il existe plusieurs types de périphériques !

* **périphériques d'entrée** : servent à transmettre des informations à un ordinateur.
* **périphériques de sortie** : servent à faire sortir des informations d'un ordinateur.
* **périphériques d'entrée-sortie** : vous vous en doutez, ils permettent de faire les deux !

### Périphériques d'entrée 

On trouve différents types de périphériques d'entrée :

* **dispositifs de saisie** : clavier, pavé numérique
* **dispositifs de lecture** : lecteur de disque optique (CD, DVD, etc.), lecteur de carte à puce, lecteur de code-barres, etc.
* **dispositifs de pointage** : souris, trackpad (pavé tactile en français), trackpoint, tablette graphique
* **contrôleurs de jeu** : manettes ou joysticks, pour les jeux-vidéos.
* **dispositifs d'acquisition d'image** : scanner, webcam
* **dispositifs d'acquisition sonore** : microphone

### Périphériques de sortie

Un peu plus simple pour les périphériques de sortie, quelques exemples :
* écran
* imprimante
* haut-parleurs

### Périphériques d'entrée-sortie

Là encore, différents types de périphériques d'entrée-sortie :

* **mémoire de masse** : lecteur de bande magnétique, lecteur de disquette, disque dur, graveur de disques optiques, clé USB
* **équipements réseau** : modem, par exemple (on parle du réseau plus tard !)
* **périphériques d'interaction homme-machine** : écran tactile, casque de réalité virtuelle
* **périphériques multifonctions** : les imprimantes qui font également scanner, par exemple!

## SOFTWARE

## Systèmes d’exploitation (OS : Microsoft Windows, Apple MacOS, GNU/Linux) 🪟​🍏​🐧​

Un système d'exploitation (souvent appelé **OS** — _de l'anglais operating system_), est le logiciel principal d'un ordinateur.

Il offre une suite de service permettant la création de logiciels applicatifs, et sert d'intermédiaire entre ces logiciels et le matériel. Il offre une suite de service permettant la communication entre le matériel physique de l’ordi (à travers les drivers) et les logiciels/applications interne (Chrome, Microsoft…)

![04-OS]()

Il existe plusieurs systèmes d'exploitation, les plus courament rencontrés sur les ordinateurs modernes sont : Microsoft Windows 🪟, Apple MacOS ​🍏 et GNU/Linux 🐧​.

### Smatphones 📳​

ils ont aussi un système d’exploitation (Android, iOS, Windows Phone, ancien Blackberry OS)

## Composants d’un OS :

### Noyau

Le Noyau (kernel) : est le composant principal d’un OS (c’est le chef d’orchestra). Il offre les fonctionnalités suivantes :

* exécution et ordonnancement des programmes
* utilisation et gestion des ressources de l'ordinateur, comme la mémoire
* gestion des périphériques
* manipulation des systèmes de fichiers (on en parle après)
* gestion et communication via le réseau

### Interfaces 

Pour exploiter les fonctionnalités du noyau, on peut passer par différentes interfaces :

* **interface graphique** (GUI, Graphical User Interface, en anglais)
* **interface en ligne de commande** (CLI, Command Line Interface, en anglais)
* **interface de programmation** (API, Application Programming Interface, en anglais)

L'interface graphique et l'interface en ligne de commande sont appelées des **interfaces homme-machine**, souvent abrégé **IHM**.

Elles permettent à un homme (dans le sens "être humain") d'interagir avec la machine.

![05-Interfaces]()

#### Interface Graphique 
Ce sont les différents menus proposés par le OS pour le configurer et réaliser des tâches courantes. Par exemple, pour éteindre votre ordinateur, vous cliquez sur le

menu démarrer puis sur le bouton d'extinction.

#### Interface en ligne de commande 
Moins connues du grand public, les interfaces en ligne de commande permettent aux utilisateur avancés (nous !) d’interagir avec le OS en lançant des commandes, instructions spécifiques, propres au système de OS utilisé. Par ex, pour éteindre l’ordi, on peut lancer la commande shutdown.

Sur Windows, elle est accessible via un programme préinstallé qui s’appelle l’**invite de** **commande**. On appelle parfois cette interface le **terminal**.

![06-Terminal]()

💡​ **À retenir** _Raccourci clavier_ : `Ctrl + R` ou `Win + R` puis taper `cmd`.

#### Interface de programmation 

Les interfaces de programmation ne sont pas directement utilisées par des humains, mais utilisées par des programmes informatiques (codés par des humains !) pour accéder à des fonctionnalités du OS.

## Démarrage d’un OS

### Procédure de démarrage

1. on appuie sur le bouton "power", d'allumage
2. le BIOS initialise tous les composants de la carte mère et certains périphériques
3. le BIOS identifie les périphériques internes et externes
4. le BIOS démarre le système d'exploitation
5. une fois le système d'exploitation démarré, on peut lancer nos applications !

Pour plus d'information sur [BIOS, c'est ici](https://fr.wikipedia.org/wiki/BIOS_\(informatique\))

En anglais, on dit qu'un ordinateur "**boot**" pour indiquer qu'il démarre. Ce terme est souvent utilisé en Français également. Redémarrer un ordi = **reboot** !

### Processus : la RAM

Un processus (_process_, en anglais) est un programme en cours d'exécution sur un ordinateur.

L'exécution d'un processus dure un certain temps, avec un début et parfois une fin. Un processus peut être démarré par l'utilisateur ou par un autre processus.

Les applications, logiciels, que nous utilisons tous les jours, sont souvent composées d'un ensemble de processus.

### Système multitâche

Un système d'exploitation est multitâche s'il permet d'exécuter, de façon apparemment simultanée, plusieurs programmes.

Un cœur de processeur ne peut exécuter qu'une seule tâche à la fois : pour pouvoir lancer plusieurs programmes simultanément, le système d'exploitation va passer de l'exécution d'un processus à un autre très rapidement.

Sur Windows, on peut voir (et stopper !) les processus et programmes en cours d'exécution depuis le **Gestionnaire des tâches**

![07-Gestionnaire de tâches]()

### Pilote de Périphérique (Driver)

C’est un Driver ou programme qui permet au OS et aux autres logiciels d’interagir avec un périphérique connecté à l’ordinateur. Chaque périphérique a son propre pilote. Sans pilote, le périphérique ne peut pas être utilisé.

**Composants internes**

Les composants matériels internes à l’ordi, comme la carte graphique, peuvent également nécessiter un pilote

#### Pilotes génériques

Les systèmes d’exploitation proposent leurs propres pilotes génériques censés fonctionner avec la plupart des périphériques. Ils proposent en général moins de fonctionnalités que les pilotes fournis par le constructeur du périphérique.

#### Plug-and-play

Certains périphériques sont dits ‘plug and play », pour sous-entendre qu’il suffit de les connecter et qu’ils sont directement utilisables. Le OS détermine automatiquement le pilote à utiliser sans nécessiter d’action de l’utilisateur.

#### Installation de pilotes, quand ce n’est pas du plug and play

Si le OS ne propose pas de pilote générique pour notre périphérique, pas le choix, il faudra l’installer nous-même ! Conseillé de passer directement par le constructeur.

Les pilotes sont en général téléchargeables depuis le site officiel du fabricant du périphérique. Ou moins par des sources sûres. Dans le logiciel d'installation via les tiers il inclut d'autre logiciel et les gens acceptent sans regarder ce qu'il accepte en pensant que c'est les conditions d'utilisation (on peut même télécharger un virus).

Les pilotes doivent être conçus pour le système d’exploitation installé sur l’ordinateur (un pilote Windows ne fonctionnera pas sous MacOS !). Or, si j’ai un vieil ordi portable qui ne supporte plus Win 10/11, on peut lui installer Linux

Sur Windows, on peut gérer les périphériques et leurs pilotes depuis le **Gestionnaire de Périphériques** : `Win + x` et ensuite `G`.

![08-Gestionnaire de périphériques]()

### Fichiers 📂​

Un fichier est une collection d’informations portant un **nom, enregistré sur un média** tel qu’un disque dur, une bande magnétique, un disque optique ou une mémoire flash. Le OS s’occupe de créer, modifier et détruire les fichiers et les répertoires (dossiers).

Le SO permet également de modifier les attributs des fichiers: leur nom, date de création/modif, type de contenu, taille et emplacement. Aussi, il gère des permissions : autorisations qui indiquent si un utilisateur spécifique peut lire, écrire (modifier) ou exécuter le fichier.

### Installer un OS

Normalement un OS est préinstallé. Mais il est tout à fait possible de l’installer nous-même !

Pourquoi faire ?

* Installer un système différent de celui fourni
* installer une nouvelle version du sytème (pour passer à Windows 11, par exemple)
* résoudre un bug, ou en cas de présence d’un virus ; 
* réinstaller le système pour le configurer différemment.

En entreprise, on réinstalle en général systématiquement un OS sur les ordis neufs, car la marque qui vend l’ordi aura toujours des partenariats qui polluent l’ordi. Ça va virer les apps inutiles. Si ça vient d’un autre pays, par exemple, mieux réinstaller du neuf pour ne pas se retrouver espionnés.

### CD/DVD d’installation

Plus d’actualité, mais on a besoin d’un **média d’installation**. Pendant de nombreuses années, les systèmes d'exploitation étaient installés à partir d'un DVD ou d'un CD-ROM fourni par l'éditeur.

De nos jours, avec la disparition des lecteurs de disques optiques, on utilise plutôt une clé USB.

### Image ISO

La clé USB d’installation n’est en général pas fournie par l’éditeur du OS. Nous allons devoir la créer nous-mêmes depuis un autre ordi, avec un utilitaire prévu à cet effet et en utilisant une image disque (aussi appelé image ISO). 

Une image disque est une copie conforme (une « photo ») du contenu d’un disque optique ou magnétique. Il existe différents formats d’images disque, mais le format ISO est le plus utilisé.

### Booter sur le média d’installation

Pour installer un OS sur un ordi, il va falloir **faire en sorte que l’ordi démarre un OS « léger » stocké sur le média d’installation** (que ce soit une clé USB ou un DVD). Il faudra pour cela modifier l’ordre de démarrage dans le BIOS de l’ordi.

Ce système léger va nous permettre d’effectuer l’installation, en nous posant différentes questions.

Une fois le nouveau OS installé, il suffit de redémarrer l’ordi et de retirer le média d’installation (ou modifier à nouveau l’ordre de démarrage dans le BIOS), pour que l’ordi démarre sur le nouveau système installé sur l’un des disques durs.

💡​ Il faut une clé USB spéciale pour les BIOS.
On rentre la clé bootable à l’ordi, et il va dire sur quelle touche appuyer pour booter le BIOS, suivre les instructions.

---

### Bagage culturel

Il faut apprendre les dates de l’évolution de l’informatique : métier Jacquard (1801), Ada Lovelace, L’ancêtre du système binaire est la case de cartes perforées (Métier Jacquard)…

---

## L'informatique et son histoire

Selon Wikipédia, l'Informatique c'est un domaine d'activité scientifique, technique et industriel concernant le **traitement automatique de l'information** numérique par l'**exécution de programmes informatiques** hébergés par des dispositifs électriques- électroniques : des systèmes embarqués, des ordinateurs, des robots, des automates, etc.

Pour en savoir plus: [Informatique](https://fr.wikipedia.org/wiki/Informatique)

## Les débuts

Le terme "informatique" n'est apparu que dans les années 50-60. Mais en réalité, l'informatique remonte à plus longtemps que ça !

### Ada Lovelace

![09-Ada Lovelace]()

Date : environ entre 1830 & 1852

[Ada Lovelace](https://fr.wikipedia.org/wiki/Ada_Lovelace) est une pionnière de l'informatique. D'après Wikipédia : Elle est principalement connue pour avoir réalisé le premier véritable programme informatique, lors de son travail sur un ancêtre de l'ordinateur : _la [machine analytique de Charles Babbage](https://fr.wikipedia.org/wiki/Machine_analytique)_

![10-Machine analytique]()

### Métier Jacquard

Date : 1801

D'après Wikipédia : _Le métier Jacquard est un métier à tisser mis au point par le Lyonnais Joseph Marie Jacquard avec l'aide du menuisier Jean-Antoine Breton en 1801, **premier système mécanique programmable avec cartes perforées**._

## Al-Khwârizmî

![11-Al-Khwârizmî]()
Date : environ entre 800 et 850

Le mathématicien [Al-Khwârizmî — Wikipédia](https://fr.wikipedia.org/wiki/Al-Khw%C3%A2rizm%C3%AE) a également indirectement contribué à l'informatique moderne ! D'après Wikipédia : 
>_Son nom latinisé est à l’origine du mot algorithme et le titre de l'un de ses ouvrages (Abrégé du calcul par la restauration et la comparaison) est à l'origine du mot algèbre. Al-Khwârizmî a classifié les algorithmes existants, en particulier selon leurs critères de terminaison, mais ne les a pas inventés._

### Euclide

![12-Euclide]()

Date : vers 300 avant JC

_En mathématiques, l'[Algorithme d'Euclide — Wikipédia](https://fr.wikipedia.org/wiki/Algorithme_d%27Euclide) est un algorithme qui calcule le plus grand commun diviseur (PGCD) de deux entiers, c'est-à-dire le plus grand entier qui divise les deux entiers, en laissant un reste nul._

L'algorithme d'Euclide serait l'un des plus anciens algorithmes jamais conçus.

L'ère moderne : Retour au 20ème siècle !

## Alan Turing

![13-Alan Turing]()

Date : entre 1936 et 1940 environ

Mathématicien et cryptologue, Alan Turing est l'auteur de travaux qui fondent scientifiquement l'informatique, notamment la [Machine de Turing](https://fr.wikipedia.org/wiki/Machine_de_Turing). Il est aussi l'un des pionniers de l'intelligence artificielle.

_Durant la Seconde Guerre mondiale, il joue un rôle majeur dans la cryptanalyse de la machine Enigma utilisée par les armées allemandes._


### Machines à calculer

Datealculer électromécaniques voient le jour :

* [IBM AS CC / Harvard Mark I](https://fr.wikipedia.org/wiki/Harvard_Mark_I) : créée par Howard Aiken & Thomas J. Watson + IBM entre 1939 et 1943, puis donnée à l'université Harvard en 1944 

* [Zuse 3](https://fr.wikipedia.org/wiki/Zuse_3) : développée en secret par Konrad Zuse entre 1939 et 1941, détruit pendant la Seconde Guerre mondiale. 

![14-Machine à calcular]()

## Premier "ordinateur"

Date : entre 1937 et 1946

Le premier "ordinateur" ne fait pas consensus : d'un ouvrage à l'autre, 6 machines sont qualifiées comme étant le premier ordinateur :

* 1937 : [Atanasoff–Berry Computer — Wikipédia](https://fr.wikipedia.org/wiki/Atanasoff%E2%80%93Berry_Computer)
* 1939 : le [Complex Number Calculator — Wikipédia](https://fr.wikipedia.org/wiki/Complex_Number_Calculator) de George Stibitz
* 1941 : le Zuse 3
* 1943 : le [Colossus (ordinateur) — Wikipédia](https://fr.wikipedia.org/wiki/Colossus_\(ordinateur\))
* 1944 : l'ASCC/Mark I d'IBM
* 1946 : l'[ENIAC — Wikipédia](https://fr.wikipedia.org/wiki/ENIAC)

## John Von Neumann

![15-Neumann]()

Date : 1945

Mathématicien, [John von Neumann — Wikipédia](https://fr.wikipedia.org/wiki/John_von_Neumann), a travaillé sur la machine EDVAC (slide suivante), et est notamment connu pour son rapport sur cette machine publié en 1945.

Il a donné son nom à l' [Architecture de von Neumann — Wikipédia](https://fr.wikipedia.org/wiki/Architecture_de_von_Neumann), utilisée dans la quasi-totalité des ordinateurs modernes.

## EDVAC

Date : 1946 (lancement du projet) - 1951 (mise en service)

L'[Electronic Discrete Variable Automatic Computer (EDVAC)](https://fr.wikipedia.org/wiki/Electronic_Discrete_Variable_Automatic_Computer) est l'un des tous premiers ordinateurs électroniques. Il opère en binaire, contrairement à l'ENIAC qui opère en décimal.

Il permet de faire des opérations mathématiques (addition, soustraction, multiplication & division), consomme 56kW (56 000 watts), comporte près de 6 000 tubes à vides et 12 000 diodes, pèse 7 tonnes, occupe plus de 45m2 et demande 2 équipes de 30 personnes qui se succèdent pour le faire fonctionner en continu.

## Premiers ordinateurs commercialisés

![16-Premiers ordi]()

* 1949 : Le [BINAC](https://fr.wikipedia.org/wiki/BINAC), produit par l'entreprise EMCC (première entreprise fabriquant des ordinateurs, premier ordinateur commercialisé)
* 1951 : Le [Ferranti Mark I](https://fr.wikipedia.org/wiki/Ferranti_Mark_I), premier ordinateur électronique "généraliste" commercialisé
* 1951 : L'[UNIVAC I](https://fr.wikipedia.org/wiki/UNIVAC_I), premier ordinateur commercial réalisé aux États-Unis.

## La révolution, une découverte a tout changé

### Le Transistor

![17-Transistor]()

Date : 1947

Suite aux travaux sur les [Semi-conducteurs](https://fr.wikipedia.org/wiki/Semi-conducteur), le premier transistor a été réalisé dans les [Laboratoires Bell](https://fr.wikipedia.org/wiki/Laboratoires_Bell). Les chercheurs à l'origine de cette "découverte" ont reçu le prix Nobel de physique en 1956.

Le transistor est considéré comme un énorme progrès face au tube électronique : beaucoup plus petit, plus léger et plus robuste, fonctionnant avec des tensions faibles, autorisant une alimentation par piles, il fonctionne presque instantanément une fois mis sous tension, contrairement aux tubes électroniques qui demandaient une dizaine de secondes de chauffage, généraient une consommation importante et nécessitaient une source de tension élevée (plusieurs centaines de volts).

## Premiers ordinateurs à transistor

* Bell Labs [TRADIC](https://fr.wikipedia.org/wiki/TRADIC) (1954)
* Lincoln Laboratory [TX-0](https://fr.wikipedia.org/wiki/TX-0) (1956)

### Disque dur

![18-Disque dur]()

Premier disque dur produit vers 1956/57 par **IBM**, le **RAMAC** **350**.

50 disques de 61cm à 1200 tours/minute - pour une capacité de ... 3.75Mo

[The First Disk Drive: RAMAC 350 - CHM Revolution](https://www.computerhistory.org/revolution/memory-storage/8/233)

### Circuit intégré

![19-Circuit intégré]()

Invention en 1958 par Jack Kilby du première [Circuit intégré](https://fr.wikipedia.org/wiki/Circuit_int%C3%A9gr%C3%A9).

Le circuit intégré, aussi appelé puce électronique (chip ou IC en anglais), est un composant électronique reproduisant **une ou plusieurs fonctions électroniques** plus ou moins complexes, **intégrant souvent plusieurs types de composants** électroniques de base dans un volume réduit, rendant le circuit facile à mettre en œuvre.

## PDP-8

![20-PDP8]()

Le [PDP-8](https://fr.wikipedia.org/wiki/PDP-8), sorti en mars 1965, est qualifié de "Fort T" de l'informatique : simple, rustique, abordable (moins de 10 000$), et produit en masse.

50 000 exemplaires produits entre 1965 et 1984. 

Sa mémoire ? À peine plus de 6 octets (4096 mots de 12 bits).

### Micro-processeur

![21-Micro-processeur]()

En 1969, le [Microprocesseur](https://fr.wikipedia.org/wiki/Microprocesseur) est inventé par un physicien d'Intel.

Un petit tour sur la page Wikipédia s'impose !

(Photo : un Intel 4004, le premier processeur !)

## Loi de Moore

![22-Loi de Moore]()

En 1965, Gordon E. Moore postule que les semi-conducteurs vont devenir deux fois plus complexes tous les ans à un coût constant.

10 ans plus tard, il ajuste sa prédiction en ce qui deviendra la loi de Moore : "le nombre de transistors sur un microprocesseur va doubler tous les deux ans".

## L’ordinateur personnel

### Mini vs. Micro vs. Mainframe

* **mainframe** : ordinateur central, utilisation partagée via des terminaux.

* **mini-ordinateur** : usage exclusif d'un processeur par un utilisateur (exemple : PDP-8)

* **micro-ordinateur** : équipés d'un microprocesseur, ils sont plus petits et moins chers ! (exemple : Altair 8800)

### Projet Simon

Date : octobre 1950

Projet publié dans 13 articles du magazine Radio-Electronics : plans permettant de fabriquer un ordinateur pour environ 600 $.

### IBM 610

Ordinateur "automatique personnel" vendu en 1957.

prix : 55 000 $ (180 vendus)

### Olivetti Programma 101

![23-Olivetti Programma 101]()

Parfois présenté comme premier ordinateur personnel, ou ordinateur de bureau (en réalité plutôt une calculatrice programmable), sorti en 1965.

Plus de 44 000 unités vendues à 3200$.

### MIR

Série d’ordinateur soviétiques, entre 1965-1969

### Kenbak-1

Date : 1971

"Premier ordinateur personnel du monde" selon le Computer Museum de Boston.

750$, 40 machines construites et vendues.

Très limité (256 octets de mémoire, entrées/sorties via lumières & boutons uniquement), mais permettait d'apprendre les bases de la programmation !

### Datapoint 2200

![24-Datapoint 2200]()

Première machine ressemblant à un ordi moderne (écran, clavier, stockage) !

Sorti en 1970, avec un processeur Intel 8008 (successeur du 4004).

### Micral

![25-Micral]()

Développé en 1972 et commercialisé dès 1973 par R2E, un bureau d'études Français.

Il a été utilisé en majorité pour les barrières de péages des autoroutes.

Basé sur processeur Intel 8008, 2Ko de RAM.

On le programme en utilisant un [Téléscripteur](https://fr.wikipedia.org/wiki/T%C3%A9l%C3%A9scripteur).

R2E a été absorbé par le groupe Bull en 1978.

### Téléscripteur

![26-Téléscripteur]()

Bel exemple d'une [page Wikipédia](https://fr.wikipedia.org/wiki/T%C3%A9l%C3%A9scripteur) beaucoup plus fournie en anglais qu'en français.

### Xerox Alto & Star (1973/1981)

[Xerox Alto](https://fr.wikipedia.org/wiki/Xerox_Alto) : première souris et première interface graphique ! Juste un prototype, mais premier ordi personnel complet au sens moderne.

[Xerox Star](https://fr.wikipedia.org/wiki/Xerox_Star) : version basée sur le proto, premier vrai ordi personnel complet commercialisé.

### IBM 5100 (1975-1981) :

premier ordi transportable.

![27-IBM 5100]()

1978 : IBM 5110 , plus large public, 5120 écran plus grand [IBM 5100 et 5110](https://fr.wikipedia.org/wiki/IBM_5100_et_5110)

1981 : 5150 successeurs du 5110, architecture différente, qui deviendra le fameux IBM PC

### Altair 8800 (1975)

![28-Altair 8800]()

Considéré comme l'un des premiers micro- ordinateurs vendus aux particuliers : l'Atlair 8800 est sorti en 1975, vendu sous forme de kit à 400$.

Bill Gates, Paul Allen, Steve Wozniak et Steve Jobs ont tous fait leurs débuts sur cette machine. Le premier logiciel développé par Microsoft était Altair Basic (un langage de programmation).

[Altair 8800](https://fr.wikipedia.org/wiki/Altair_8800)

Si jamais on a regardé Malcom, c'est possible qu'on ait vu passer l'Altair 8800 dans une épisode

![29-Malcom with an Altair 8800]()

### Homebrew Computer Club (Apple)

![30-Apple Homebrew]()

Club d'amateurs d'informatique qui se réunissent dans la Silicon Valley.

Plusieurs machines conçues par des membres du club ont été commercialisées, comme le Sol-20 ou ... l'Apple I, fabriqué artisanalement par Steve Wozniak.

### Appel II (1977/1993)

![31-Apple II]()

Après une commande de 100 Apple I, Woz & Steve Jobs fondent Apple Computer.

Quelques dizaines de commandes plus tard et ils annoncent l'Apple II, un ordinateur complet, qui sort en 1977.

4 millions d'Apple II vendus ! (jusqu'à la fin de sa production en 1993)

### Commodore PET (1977)

![32-Comodore PET]()

Sorti en 1977, un peu moins d’un million d'unités vendues.

Pour savoir plus de [Commodore PET](https://fr.wikipedia.org/wiki/Commodore_PET) 👈​

### TRS-80 (1977)

![33-TRS 80]()

Sorti en 1977, 1,5 millions d'unités vendues.

Pour plus d'information sur [TRS-80](https://fr.wikipedia.org/wiki/TRS-80) 👈​

### TL-99 (1979)

![34-TL 99]()

Sorti fin 1979, 2,8 millions vendus.

Pour savoir plus du [TI-99/4A](https://fr.wikipedia.org/wiki/TI-99/4A) 👈​

###  Atari 8-bits (1979)

![35-Atari 8 bits]()

Sorti en novembre 1979.

Pour plus d'information sur l'[Atari 8-bits](https://fr.wikipedia.org/wiki/Atari_8-bits) 👈​

### Sinclair (1970/1980)

![36-Sinclair]()

Date : fin des années 70 / début 80

Pour plus d'information du [Sinclair Research](https://fr.wikipedia.org/wiki/Sinclair_Research) 👈​

### Commodore VIC-20 & 64 (1980/1982)

![37-Comodore VIC 20]()

1980 & 1982

25 millions d'unités vendues pour le Commodore 64 !

Pour plus de détails sur le [Commodore 64](https://fr.wikipedia.org/wiki/Commodore_64) 👈​

### Oric-1 & Atmos (1982/83)

![38-Oric 1 & Atmos]()

L'ordinateur [Oric-1](https://fr.wikipedia.org/wiki/Oric#Oric-1), sorti en 1982/83, est le premier ordi à se répandre dans les foyers français & anglais (UK). 

[Oric Atmos](https://fr.wikipedia.org/wiki/Oric#Oric_Atmos): sorti en 1985. 

### Thomson TO7 (1982) & MO5 (1984)

![39-Thomson T07]()

1982 : [Thomson TO7](https://fr.wikipedia.org/wiki/Thomson_TO7)

1984 : [Thomson MO5](https://fr.wikipedia.org/wiki/Thomson_MO5)

### Amstrad CPC 464 (1984)

![40-Amstrad CPC 464]()

Sorti en 1984, pensé pour une utilisation familiale (il ne coûtait "que" 3500 francs).

Plus d’un million d'exemplaires vendus !

### IBM PC (1981/83)

![41-IBM PC]()

1981 : [IBM 5150](https://fr.wikipedia.org/wiki/IBM_PC), à base de processeur Intel 8088 

1983 : [IBM PC XT](https://fr.wikipedia.org/wiki/IBM_PC/XT)

#### Lisa & Apple Macintosh (1983/84)

![42-Lisa & Apple Macintosh]()

[Apple Lisa](https://fr.wikipedia.org/wiki/Apple_Lisa) 1983 (avec interface graphique) Motorola 68000 1Mo RAM, DD 5Mo 10k$ / 70k francs : 

[Macintosh 1984](https://fr.wikipedia.org/wiki/Macintosh_128K), premier succès d'interface graphique WIMP (Windows, Icons, Menus & Pointing device) similaire au Lisa mais à 2495€ 128ko de RAM, pas de disque interne.

### IBM PS/1 & PS/2 (1987/90)

![43-IBM PS1]()

1987 : [IBM Personal System/2](https://fr.wikipedia.org/wiki/IBM_Personal_System/2)

1990 : [IBM Personal System/1](https://en.wikipedia.org/wiki/IBM_PS/1)

### IBM compatible

![44-IBM compatible]()

Pour en savoir un peu [Compaq](https://fr.wikipedia.org/wiki/Compaq) 👈​

#### Fin des années 90, 2000 & de nos jours, tout commence à exploser en évolution, niveau hardware et software.

[Compatible PC](https://fr.wikipedia.org/wiki/Compatible_PC), informatique embarquée & miniaturisation…

Mais jusqu’à présent nous n’avons traité que du hardware… Et les systèmes d’exploitation & logiciels dans tout ça ? – On parlera de l’histoire des systèmes d’exploitation en saison A5, quand on parlera de **GNU/Linux**.

---