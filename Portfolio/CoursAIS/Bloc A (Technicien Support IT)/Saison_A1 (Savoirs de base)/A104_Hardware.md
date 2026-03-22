# Session A104. Composants Hardware.

## Composants Hardware

_Comment monter son propre PC ! (et aussi réparer des PCs !)_

### Carte mère : 

Composant indispensable, elle **permet de connecter tous les autres composants de l’ordi**. C’est un circuit imprimé, dont la taille est **standardisée**. Elle est équipée de nombreux ports et connecteurs. On va y trouver : Connecteurs d’extension PCI, Connecteurs d’entrée-sortie, Support du processeur, chipset, Connecteurs de mémoire vive RAM, Connecteur alimentation, Connecteurs de lecteurs de disques et disquettes, BIOS, Connecteur d’extension AGP, Pile du CMOS.

![01-Carte mère](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_01-Carte%20m%C3%A8re.png)

_Une carte mère AsRock, pas toute jeune !_

![02-Carte mère Asus](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_02-Carte%20m%C3%A8re%20Asus.png)

_Une carte mère Asus, un peu plus récente !_

![03-Éléments carte mère](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_03-%C3%89l%C3%A9ments%20carte%20m%C3%A8re.png)

_Les éléments principaux qu'on rencontre sur une carte mère._


Aujourd’hui la plupart de RAM ont leur propre système de cooling.

Le bus PCI (Peripheral Component Interconnect) est une norme ancienne, peu puissante, utilisée principalement entre 1994 et 1998.

L'AGP (Accelerated Graphics Port), quant à lui, a été conçu pour remplacer le PCI, offrant une vitesse améliorée entre 1998 et 2005.

Les cartes graphiques PCI et AGP sont désormais obsolètes, remplacées par le PCI Express (PCIe).

### Connecteurs électriques 

Ces connecteurs permettent d’acheminer le courant électrique du bloc d’alimentation vers la carte mère. Les cartes-mères modernes en compte deux :
* le connecteur 24 pins (broches) de type ATX : c'est l'alimentation principale de la carte.
* le connecteur 4 ou 8 pins pour CPU : permet d'alimenter le processeur

On les appelle connecteurs PIN.

![04-Connecteurs 8 PIN](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_04-Connecteurs%208%20PIN.png)

_Les deux connecteurs électriques sur une carte mère._

### Socket processeur  

Le processeur doit être installé dans un connecteur spécifique de la carte-mère, appelé **socket** (support processeur en français, mais ce terme est très rarement utilisé).

⚠️​ Attention, tous les processeurs ne sont pas compatibles avec toutes les cartes mères : il existe différents sockets (il faut donc s'assurer que la carte mère et le processeur soient compatibles).

Il faut que ton processeur soit compatible avec le processeur et la carte-mère. On ne peut pas changer le socket de carte-mère. On achète l’un en fonction de l’autre. Il faut lire sur la fiche technique de chacun. Avec les configurateurs sur les sites comme top achat ldlc, sa devient automatique. Généralement quand tu montes ta config (en étant débutant) tu utilises un configurateur, et ce configurateur te propose des éléments uniquement compatibles ensemble.

![05-Installantion d'un CPU](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_05-Installantion%20d'un%20CPU.png)

_Installation d'un CPU dans le socket de la carte-mère._

![06-Différents sockets par fabricant de processeur](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_06-Diff%C3%A9rents%20sockets%20par%20fabricant%20de%20processeur.png)

_Les différents sockets, par fabricant de processeur._

### Connecteurs mémoire  

ils sont appelés des **slots**. Ils ont un nombre de deux, quatre, et parfois six ou huit (mais c’est rare), et permettent de connecter un composant indispensable : les barrettes mémoire (barrettes de RAM). Ils se distinguent des autres connecteurs par la présence d’ergots de sécurité à leurs extrémités et d’un détrompeur, évitant d’insérer la barrette mémoire à l’envers. 

⚠️​ Attention, toutes les barrettes mémoire ne sont pas compatibles avec toutes les cartes mères !
Attention à ne pas confondre DDR (RAM dédié à la carte-mère) et GDDR (RAM dédié à la carte graphique).

On n’est pas sur les mêmes versions actuellement (DDR5 et GDDR6).

![07-Connecteurs mémoire](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_07-Connecteurs%20m%C3%A9moire.png)

### Connecteurs cartes d’extension  

Les cartes mères proposent en général plusieurs connecteurs (appelés **slots**, eux-aussi), qui permettent de connecter diverses cartes d'extension. ça peut être la carte graphique ou d’autres choses (Wi-Fi, USB, carte son, carte Ethernet, GPU) si besoin.

Le débit de chaque ligne est limité à 250 Mo/s. Il existe des connecteurs et des câbles pour les versions 1x, 4x, 8x et 16x du bus. Une évolution vers des lignes à 500 Mo/s (comme le PCIe 2.0) est prévue mais sans date annoncée.

Il en existe plusieurs types :

* **ISA** (Industry Standard Architecture) : premier slot d'extension interne créé par IBM, disparu depuis les années 90.
* **PCI** (Peripheral Component Interconnect) : apparu en 1994, descendant du slot ISA. Toujours présent aujourd'hui dans sa version plus rapide, PCI Express.
* **AGP** (Accelerated Graphic Port) : lancé en 1997 par Intel, ce slot était réservé à la connexion de cartes graphiques. Disparu au milieu des années 2000.
* **PCI Express** (souvent abrégé PCIe) : lancé en 2004 par Intel, il est plus rapide et apte à supporter des cartes graphiques (mais il supporte aussi d'autres types de cartes).

![08-Différents PCI](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_08-Diff%C3%A9rents%20PCI.png)

_De haut en bas : PCIe x4, PCIe x16, PCIe x1, PCIe x16 et PCI traditionnel._

### Connecteurs de stockage 
On retrouve également des connecteurs dédiés aux périphériques de stockage de masse (disque dur, lecteur de disque optique, disque SSD).

Il en existe 5 types :

* **Connecteur Floppy** : permet de connecter un lecteur de disquettes, obsolète
* **Connecteur IDE** : aussi appelé PATA, _Parallel_ ATA, plus long que le Floppy, permet de connecter des disques durs et lecteurs/graveurs de disques optiques. Remplacé par le SATA de nos jours, mais encore présent sur certaines cartes mères.
* **Connecteur SATA** : pour _Serial ATA_, actuellement en version 3, permet de connecter des lecteurs/graveurs de disques optiques et des disques durs et SSD.
* Connecteur **mSATA** : pour _mini SATA_, version miniature de connecteur SATA destinée aux ordinateurs portables.
* **Connecteur M.2** : permet de connecter des disques SSD ou des cartes d’extension (WiFi, bluetooth…), c’est la révolution du moment.

![09-Connecteur SATA](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_09-Connecteur%20SATA.png)

_Le connecteur **SATA**, fréquemment utilisé pour connecter disques durs et lecteurs optiques._

![10-Deux connecteurs IDE](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_10-Deux%20connecteurs%20IDE.png)

_Deux connecteurs IDE et un connecteur Floppy, obsolètes de nos jours._

![11-mSTA vs M.2](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_11-mSTA%20vs%20M.2.png)

_Sur la gauche, un SSD avec un connecteur mSATA. Sur la droite, un SSD avec un connecteur M.2. Attention à ne pas les confondre !_

![12-Types de connecteurs M.2](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_12-Types%20de%20connecteurs%20M.2.png)

⚠️​ _Attention, il existe différents types de connecteurs M.2 !_

### BIOS

**Basic Input Output System (BIOS)**, est un **logiciel directement installé sur la carte mère**. On parle parfois de micrologiciel, mais le terme anglais de **firmware** est plus utilisé.

Le BIOS offre différentes fonctions :
* Identification et initialisation des périphériques et composants à la mise sous tension
* Amorçage du système d’exploitation
* Configuration de certaines options de la carte mère

On peut accéder au BIOS en pressant une touche (qui varie d’un fabricant à l’autre) **avant le démarrage du système d’exploitation** :

* Acer : F2 ou Suppr
* Asrock & Dell : F2
* Asus : F2, Suppr ou F9
* Compaq : F10
* Gigabyte & MSI : Suppr
* HP : F10, Echap ou F1
* Lenovo : F1 ou F2
* Packard Bell : F1 ou Suppr
* Samsung : F2 ou F10
* Sony : F1, F2, F3, Echap ou Assist

On peut chercher sur Google/YouTube notre `BIOS + modèle de l’ordinateur` et on y pourra trouver une procédure.

![13-ancien BIOS](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_13-ancien%20BIOS.png)

_Un aperçu d'un BIOS, plutôt ancien (1999)_

_En comparaison, un BIOS moderne sur une carte mère MSI :_

![14-BIOS moderne](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_14-BIOS%20moderne.png)

On reparle du BIOS plus en détail en Saison 2 !

### Chipset

C’est une puce électronique soudée à la carte mère. Il était divisé en deux parties (ce n’est plus le cas sur les cartes mères modernes) :

* Northbridge (pont nord) : gestion des communications entre le processeur, la RAM, et certain bus haute vitesse.
* Southbridge : gestion communication avec les autres périphériques et composants (slots PCI, ports USB, SATA, etc.)

💡​ A savoir que le northbridge est maintenant directement intégré au processeur.

![15-Northbridge vs Southbridge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_15-Northbridge%20vs%20Southbridge.png)

_Relation entre processeur, northbridge et southbridge sur les anciennes cartes mères._

Le chipset détermine les fonctions proposées par la carte mère, et les composants avec lesquels elle est compatible, notamment le processeur ou la RAM.

C’est une puce intégrée à la CM et elle chauffe beaucoup. C’est comme l’assistant du CPU, soudée à la CM.

![16-Chipset](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_16-Chipset.png)

### Panneau d’entrées/sorties
Chaque carte mère dispose d’un panneau d’entrées/sorties (Inpunt/output Panel), sur lequel on trouve la **connectique pour les périphériques de notre ordi**.

![17-Panneau d'entrées et sorties](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_17-Panneau%20d'entr%C3%A9es%20et%20sorties.png)

_Pouvez-vous nommer tous les ports présents ci-dessus ?_

Solution, de gauche à droite et de haut en bas :

* deux ports USB (USB2.0 probablement, on y reviendra)
* un port PS/2 combo (clavier/souris)
* un port VGA
* un port DVI (DVI-D dual link plus exactement, on y reviendra)
* un connecteur optique TOSLINK (signal audio numérique)
* un port HDMI (sortie uniquement, nécessite un chipset graphique dans le CPU)
* un port DisplayPort (sortie uniquement)
* deux ports USB
* un port FireWire 400 (presque obsolète, de nos jours)
* un port eSATAp (combinaison du port eSATA et d'un port USB)
* un port 8P8C (usuellement appelé RJ45, même si c'est inexact)
* deux ports USB (3.0, probablement)
* 6 ports pour l'audio (seul le vert pour les haut-parleurs et le rouge pour le micro sont utilisés, en général).

### Carte multiprocesseur : 

Avant que les processeur multi-cœurs deviennent la norme, on pouvait parfois rencontrer des cartes mères multiprocesseurs.

![18-Carte multiporcesseur](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_18-Carte%20multiporcesseur.png)

💡​ De nos jours, on rencontre uniquement ce genre de cartes mères sur les serveurs.

### Formats de carte mère :

* AT (1984) : 305mm x 279-330mm
    * Baby-AT (1985) : 216mm x 254-330mm

Ce format n'est plus utilisé de nos jours, il a été largement supplanté par ATX depuis le début des années 2000.

* ATX (1995) : 305mm × 244mm
    * microATX (1996) : 244mm × 244mm
    * FlexATX (1999) : 228,6mm × 190,5mm
    * E-ATX : 305mm × 330mm
    * Mini-ATX (2005) : 150mm × 150mm
* ITX (2001) : 215 mm × 191 mm
    * Mini-ITX (2001) : 170mm × 170mm max
    * Nano-ITX (2003) : 120mm × 120mm
    * Pico-ITX (2007) : 100mm × 72mm max
* BTX (2004) : 325mm × 267mm max
    * MicroBTX (2004) : 264mm × 267mm max
    * PicoBTX (2004) : 203mm × 267mm max
* DTX (2007) : 203 mm × 244 mm max
    * Mini-DTX (2007) : 170 mm × 203 mm max

Il faut s’assurer que le boîtier dans lequel vous comptez installer la CM soit compatible avec son format. 

💡​ Les formats les plus courants sont **ATX** et **micro-ATX**.

## Processeur (CPU)

Souvent appelé **CPU** (Central Processing Unit) est l’un des composants primordiaux, avec la mémoire vive, d’un ordinateur. C’est le composant qui effectue les calculs dans notre ordinateur.

![19-Processeur](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_19-Processeur.png)

### Classification des processeurs

Un processeur est défini par :
* Son **jeu d’instructions** (x86, AMD64, ARM, etc.)
* Ses **caractéristiques** (finesse de gravure, cadence en Ghz, nombre de cœurs, etc.)

### Jeu d’instructions

Ce sont les instructions-machine qu’un processeur peut exécuter. Ce sont les opérations de base, mathématiques (addition, soustraction, multiplication, etc.) et logiques (ET, OU, etc.) qui servent à programmer des applications/logiciels.

Les jeux d’instructions les plus répandus de nos jours :
* **x86** (16 puis 32 bits, on y reviendra), obsolète depuis quelques années
* **AMD64** (souvent appelé **x86_64**, 64 bits)
* **ARM** (16, 32 et 64 bits de nos jours)

La plupart des processeurs modernes utilisent le jeu d’instruction **AMD64/x86_64**, d’architecture **CISC**, à l’exception des processeurs Appel **M1** et **M2**, qui sont des processeurs **ARM** (d’architecture **RISC**).

---

(Parenthèse : l’**assembleur en code** c’est un code bas niveau, le plus proche de la machine qui donne directement les instructions à la procédure. C’est du binaire.  Comparer au langage **COBOL** qui est l’un de plus gros langage utilisé dans le Main Frame, avec le souci d’être trop ancien, alors on rechercher bcp des gens qui savent s’en servir)

---

### Finesse de gravure

Il est composé de plusieurs milliards de transistors. Quand on parle de **finesse de gravure** (mesurée en nanomètres) d’un processeur, on parle en réalité de la **taille d’un transistor** dans ce dernier. 

Plus un processeur contient de transistors, plus il sera capable de réaliser d’opérations par seconde.

## Loi de Moore

Gordon E. Moore a prophétisé [plusieurs lois](https://fr.wikipedia.org/wiki/Loi_de_Moore) qui prédisent l’augmentation du nombre de transistors par processeur.

## Cadence d’un processeur 

Les processeurs sont équipés d’une horloge interne, dont on exprime la cadence/ fréquence avec l'unité **Hertz** (Hz), **Mégahertz** (MHz) ou le plus souvent en **Gigahertz** (GHz).

1 Hertz = une opération par seconde.
3 Ghz = 3 milliards d'opérations par seconde.

Ces opérations sont appelées des **cycles** du processeur. Chaque cycle correspond à l’ouverture et la fermeture de milliards de transistors !

A l’époque des processeurs mono-cœurs, la cadence/fréquence du processeur était l’unique critère permettant de connaître sa vitesse d’exécution. De nos jours, d’autres paramètres rentrent en compte, comme la **mémoire cache** (aussi appelée antémémoire, qui est un type de mémoire très rapide utilisée pour améliorer les performances du processeur, des petits slots de mémoire). Pour [en savoir plus](https://fr.wikipedia.org/wiki/Cache_de_processeur) 👈​

### Gammes de processeurs
Les deux fabricants sur le marché des ordis de bureau sont Intel et AMD. Ils proposent différents modèles, plus ou moins performants.

- Chez **Intel** :
    - Xeon (gamme serveur)
    - Core i9 (haut de gamme)
    - Core i7
    - Core i5
    - Core i3 (entrée de gamme)

- Chez **AMD** :
    - EPYC/Threadripper (gamme serveur)
    - Ryzen 9 (haut de gamme)
    - Ryzen 7
    - Ryzen 5
    - Ryzen 3 (entrée de gamme)

>On cherche « PC » / Afficher le nom de votre PC. Ça ouvre les infos systèmes et on a les specs de l’ordi (proc/Ram…). Ci-après un laptop  Intel® Core(TM) i7-12650H.

![20-Exemple processeur](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_20-Exemple%20processeur.png)

### Comparer les processeurs

Le plus simple, c’est d’utiliser un comparateur tel que [cpubenchmark](https://www.cpubenchmark.net/).

​💡​ Plus les processeurs sont récents, plus ils ont tendance à être performants (à gamme équivalente), et moins énergivores.

💡 Facteur qui peut être important : certains processeurs sont équipés d’un chipset graphique, évitant la nécessité d’avoir une carte graphique (mais souvent au prix de performances graphiques moindres). Cherchez les gammes « G » chez Intel et AMD.

## Refroidissement

Un processeur, ça chauffe 🥵​

C’est un composant qui doit, dans la majeure partie des cas, être **refroidit activement** (par opposition à passivement, à température ambiante, sans ventilation).

On utilise en général une combinaison appelée **ventirad** : un radiateur en métal et un ventilateur.

_Un ventirad (sur la gauche, juste le radiateur), à fixer sur le processeur et connecter à la carte mère._

![21-Vintarad](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_21-Vintarad.png)

_Il existe plein de types de ventirad, et chacun a sa procédure de montage. Lisez la notice (ou cherchez sur YouTube/Google) !_

![22-Différents ventirad](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_22-Diff%C3%A9rents%20ventirad.png)

Sous le radiateur, qui sera en contact direct avec le boîtier en métal du processeur, on applique de la **pâte thermique** pour améliorer l'échange thermique entre le CPU et le radiateur.

Certains radiateurs sont pré-équipés de pads thermiques, collés directement sous le radiateur (pas besoin de pâte thermique).

_Application de pâte thermique sur un CPU. Pas besoin d’en mettre beaucoup (un gros grain de riz au centre suffit)._

![23-Application de pâte thermique](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_23-Application%20de%20p%C3%A2te%20thermique.png)

Il existe également des systèmes de refroidissement liquide :
* **watercooling "AIO"** (All-In-One), prêt à être installé sur un CPU - souvent plus silencieux et performant qu'un ventirad, simple à installer
* watercooling "DIY"/"custom", avec pompe, réservoir, tuyaux, etc. à monter soi-même - très performant, mais aussi très coûteux et complexe
* **azote liquide** [🎥​ versé directement sur le processeur](https://www.youtube.com/watch?v=_NxOZyfDYPs&t=154s) (voir vidéo ) - réservé en général aux concours d'overclocking (augmentation de la fréquence d'un processeur)
* **bain d'huile**, carte mère et composants complètement immergés dans un liquide non conducteur - parfois utilisé en datacenter, peu pratique dans la plupart des cas.

## Mémoire vive (RAM)

Autre composant indispensable, la **mémoire vive**, souvent appelée **RAM** (Random Access Memory) permet de stocker des **données de travail**, pendant que l’ordinateur est allumé.

Par exemple, les données (images, texte, etc.) des pages web ouvertes sur votre navigateur sont stockées dans la mémoire vive.

![24-barettes de RAM](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_24-barettes%20de%20RAM.png)

_En haut, une barette de RAM pour PC �xe (taille **DIMM**)._

_En bas, une barette de RAM pour PC portable (taille **SO-DIMM**)._


Le critère le plus important c’est sa capacité, que l’on mesure en octets (o)- ou plutôt, de nos jours, en **Giga-octets** (Go).

Autre critère c’est la **fréquence** (en Mégahertz- MHz) de la RAM. Plus elle est élevée, plus la RAM sera rapide.

⚠️​ Pour ces deux critères, faites attention à la compatibilité avec le processeur et la carte mère (indiquée sur les fiches techniques) !

Les systèmes d’exploitation modernes (sauf pour GNU/Linux) requièrent en général un **minimum de 4 Go de mémoire vive**, et il faut idéalement en avoir 8.

Certains besoins, tels que la virtualisation, le gaming, ou certaines applications professionnelles peuvent demander beaucoup plus de RAM.

Aujourd'hui en environnement pro, même pour de la bureautique on propose 32 Go. La différence de tarif entre 16 et 32 est beaucoup moins significative

64 Go c’est nécessaire uniquement pour la virtualisation et la gaming.

### Types de mémoire RAM

Il existe différents types/technologie de mémoire RAM, mais de nos jours les machines modernes utilisent de la mémoire **DDR SDRAM** (Double Data Rate Synchronous Dynamic Random Access Memory).

Il existe différentes générations de DDR, de DDR1 à DDR5 (la plus récente). ⚠️​ **Attention** à la compatibilité avec votre carte mère /processeur là-aussi !

![25-Types de mémoire](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_25-Types%20de%20m%C3%A9moire.png)

## Dual channel

La plupart des cartes mère et processeurs modernes permettent d’utiliser un double canal de communication, permettant de doubler la bande passante effective (ça améliore drastiquement la performance). 

Il est nécessaire d’utiliser des barettes de RAM identiques connectés sur des slots spécifiques, se référer au manuel de votre carte mère : [En savoir plus](https://fr.wikipedia.org/wiki/Architecture_de_m%C3%A9moire_%C3%A0_multiples_canaux).

## Disque dur

Un disque dur (HDD, Hard Drive Disk) est une mémore de masse magnétique, à disques tournants.

Inventé en 1956, il a énormément évolué en capacité et diminué drastiquement en encombrement au fil des années.

💡​Contrairement à la mémoire vive, un disque dur conserve les données même sans alimentation électrique : [📸​ Voici quelques photos](https://fr.wikipedia.org/wiki/Disque_dur).

### Capacité

C’est la quantité de données que l’on peut stocker dans le disque dur, mesurée en **Giga-octets** (Go) out Tera-octets (To).

1 To = 1 000 Go = 1 000 000 Mo = 1 000 000 000 Ko = 1 000 000 000 000 octets

On les trouve sur le marché de 250 Go à 24To (peut-être même plus).

### Performance

La performance d'un disque dur, étant un appareil mécanique, dépend de plusieurs facteurs : temps de latence (liée à la vitesse de rotation des plateaux), temps de positionnement de la tête de lecture, temps de transfert.

En règle générale, on peut s'attendre à des vitesses de transfert oscillant **entre 30 et 150 Mo/s** sur un disque dur.

### Format

En général, on trouve deux dimensions de disques durs :

* 2.5" (pouces), utilisé pour les PC portables
* 3.5", dans les PC fixes

![26-Disque dur de 2.5puces](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_26-Disque%20dur%20de%202.5puces.png)

### Connectique 

Il existe plusieurs connectiques différentes pour les disques durs :
* sur les ordinateurs de bureau :
    * **IDE**, aussi appelé **PATA** (de l'anglais Parallel ATA, Advanced Technology Attachment), obsolète de nos jours
    * **SATA** (de l'anglais Serial ATA), en version 2 ou 3 (la version 3 offre un meilleur débit), fréquemment rencontré. 

Récupérer de l’information, le HDD est plus fiable

![27-Disque dur IDE et SATA](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_27-Disque%20dur%20IDE%20et%20SATA.png)

_Sur la gauche, un disque dur connecté en IDE/PATA (avec une nappe IDE), et un connecteur MOLEX 4 broches pour l'alimentation._

_Sur la droite, un disque dur connecté en SATA (avec un câble SATA), et un connecteur SATA spécifique pour l'alimentation._


* sur les serveurs / stations de travail :
    * **Parallel SCSI** (de l'anglais Small Computer System Interface), différents connecteurs plus ou moins obsolètes
    * **SAS** (de l'anglais Serial Attached SCSI), différentes versions comme SATA, plus ou moins performantes.

💡​ Les disques durs SATA sont en général compatibles avec des connecteurs SAS, l'inverse n'est pas vrai.

![28-Connecteurs Parallel](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_28-Connecteurs%20Parallel.png)

_Deux disques durs avec des connecteurs Parallel SCSI, uniquement rencontré sur du matériel relativement ancien._

### Disque SSD

Les disques SSD (de l'anglais Solid State Drive) sont des mémoires de masse qui permettent de stocker des données sur de la **mémoire flash**.

Leur principal avantage par rapport aux disques durs mécaniques : ils sont **beaucoup plus performants** (meilleures vitesses de transfert). Ils sont également **plus résistants aux chocs**, n’ayant pas de pièces en mouvement.

### Mémoire Flash

C’est un composant électronique qui est un type de ROM (Read-Only Memory) : on parle d’**EEPROM** (Electrically-Erasable Programmable Read-Only Memory).

Une mémoire flash fonctionne comme une barrette de RAM, **mais les données ne disparaissent pas lors de la mise hors tension**.

Les clés USB, cartes SD ou Compact Flash et les disques SSD reposent tous sur ces mémoires flash.

### Types de disques SSD 

* format disque dur 2,5’’, connectique SATA 3
* format carte d’extension PCI Express (très performant)
* format mSATA (spécifications proches du format disque 2,5’’)
* format M.2, avec ou sans NVMe (très performant en NVMe/PCIe)

![29-SSD 2.5 puces](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_29-SSD%202.5%20puces.png)

_Un SSD au format 2.5" monté dans un adaptateur 3.5" pour installation dans un PC fixe._

![30-Différents SSDs](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_30-Diff%C3%A9rents%20SSDs.png)

_Différents types de SSDs, plus ou moins performants._

⚠️ **Attention**, comme pour les autres composants, Il faut s’assurer que le disque SSD que l’on souhaite installer est **compatible avec votre carte mère** (connecteur disponible, NVMe disponible ou pas, etc.).

Ces informations sont disponibles dans les fiches techniques des cartes mères, sur le site du fabricant ou sur les sites d'e-commerce.

# Exercise : compatible ou pas ?

Pour toutes les cartes mère ci-dessous, essayez de déterminer s’il serait possible de connecter et d’exploiter à son plein potentiel un disque **Samsung 990 EVO** d’une capacité de 1 To.
* **Asus Prime A520 M-K** – Faux
* **ASRock P43 Pro/USB3** – Faux
* **MSI Z87-G45 Gaming** – Faux
* **Asus ProArt B650 Creator** – Vrai car PCI 5

Cherchez cette information dans les fiches techniques !

### Solution

Le disque Samsung 990 EVO est un disque SSD, avec un connecteur M.2 NVMe/PCI Express, compatible avec la génération PCIe 5.0 (mais rétrocompatible).

* **Asus Prime A520M-K** : la carte dispose d'un connecteur M.2 PCIe 3.0, on pourra
donc connecter le disque mais ses performances seront limitées par la génération
PCIe (3.0 alors que le disque peut aller jusqu'à la génération 5.0 !)
* **ASRock P43 Pro/USB3** : pas de port M.2/NVMe.
* **MSI Z87-G45 Gaming** : seulement un port mSATA, non compatible !
* **Asus ProArt B650 Creator** : pleinement compatible.

## Cartes d’extension

Il existe de nombreuses cartes d'extension que l'on peut connecter à la carte mère d'un ordinateur. Elles permettent de rajouter des fonctionnalités, par exemple :

* carte graphique
* carte son
* carte réseau
* carte d'acquisition

Ces cartes sont à connecter sur les slots PCI ou plus récemment PCI Express des cartes mères.

De nos jours, la plupart des fonctionnalités offertes auparavant par des cartes d’extension sont directement intégrées à la carte mère, rendant ces cartes caduques dans de nombreux cas. Seule exception : les cartes graphiques et les cartes d’acquisition. Elles restent néanmoins souvent utilisées sur les serveurs et certaines stations de travail ayant des besoins spécifiques.

## Carte graphique (CG - GPU)

**GPU** (Graphical Processing Unit) est une carte d’extension dédiée à l’affichage sur un (ou plusieurs) écrans.

Elle embarque un processeur et de la mémoire vive, et est conçue spécifiquement pour résoudre certains calculs (par exemple, le rendu 3D).

### Fabricants

De nos jours, il existe 3 grands fabricants de carte graphique, qui proposent différentes gammes :

* Nvidia
* AMD
* Intel

​💡​ Intel est arrivé récemment sur le marché des GPU, on en rencontre assez peu pour l’instant.

### Critères de Performance d’une GPU

La performance d'une carte graphique est liée à plusieurs critères :
* la cadence de son processeur interne, en Gigahertz (GHz)
* la quantité de mémoire vive qu’elle embarque, en Giga-octets (Go)
* la génération de la mémoire, on parle de [GDDR](https://en.wikipedia.org/wiki/GDDR_SDRAM)

💡​ Pour pouvoir comparer entre-elles les GPU, on peut se référer au nombre d’opérations à virgule flottante par seconde qu’elles peuvent exécuter, mesuré en FLOP (Floating-Point Operations per Second).

### Connectique d’un GPU

Les cartes graphiques se connectent via un **slot PCI Express 16x**.

Il faut s’assurer de la compatibilité de la GPU avec la CM (vitesse du bus PCI Express, notamment). 

💡​ **Attention**, Certaines cartes mères en possèdent plusieurs (comme sur la photo de la slide suivante), il faut en priorité utilisé le plus proche du processeur, qui dispose en général de la meilleure bande passante.

![31-Slots PCI Express](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_31-Slots%20PCI%20Express.png)

_Deux connecteurs (slots) PCI Express 16x, celui du haut est renforcé pour accueillir des cartes graphiques de grande taille._

Comme d'habitude, il faut s'assurer de la **compatibilité de la carte graphique avec votre carte mère** (vitesse du bus PCI Express, notamment).

⚠️ **Attention** également : certaines cartes graphiques doivent être directement alimentées par le bloc d'alimentation, il faut donc que le bloc d'alim dispose des connecteurs adaptés (c'est indiqué sur la fiche technique).

![32-Carte graphique avec alimentation directe](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_32-Carte%20graphique%20avec%20alimentation%20directe.png)

_Une carte graphique nécessitant une alimentation directe._

## Alimentation

Le bloc d’alimentation (**_power supply_ unit ou PSU**), est le composant chargé de convertir la tension électrique du secteur (220) en différentes **basses tensions continues** (12V, 5V et 3.3V).

Le bloc d'alimentation sera **connecté à la carte mère et à certains composants internes** (disques durs, lecteurs de disques optique, carte graphique, etc.).

### Puissance délivrée

Quand on choisit un bloc d’alim, le critère principal est la puissance maximale délivrée, **mesurée en Watts** (W).

Cette puissance devra être adaptée aux composants internes de l’ordinateur, quelques exemples :
* PC bureautique basique : 300/400 Watts
* PC milieu de gamme, carte graphique / plusieurs disques durs : 550/600 Watts
* PC "gamer"/CAO, carte graphique puissante : 750/850 Watts
* station de travail très puissante : + de 1000 Watts

Il n'est pas toujours évident d'estimer précisément la consommation électrique des différents composants. Certaines fiches techniques l'indiquent, mais souvent il faudra chercher sur Internet.

**Qui peut le plus, peut le moins !** Si votre bloc d'alim délivre 1000W au maximum, ça ne veut pas dire que votre PC consommera en permanence 1000W. **Les composants consomment ce dont ils ont besoin !**

​⚠️​ Attention, Il faut **se laisser un peu de marge**, ce qui permettra également d’upgrader certains composants sans devoir changer l’alim.

### Certification

Les blocs d’alim sont parfois **certifiés 80 PLUS**. Cette certification garantie un certain **rendement** entre la puissance consommée sur le secteur, en 220V, et la puissance délivrée en basse tensions.

Exemple : un PC qui consomme 500W en 220V mais ne délivre qu'un total de 400W aux composants a un rendement de 80%.

Les 20% restants sont transformés en chaleur émise par l'alim.

_Les logos des certifications 80 PLUS, visibles sur la fiche technique et parfois directement sur l'alim._

![33-Certifictations alim](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_33-Certifictations%20alim.png)

💡​ Pour ne pas "gaspiller" d'énergie, on va donc chercher un rendement le plus proche des 100%.

La certification 80+ propose plusieurs labels, qui garantissent (en théorie) un certain rendement:

| **Label** | **Rendement à 20% de charge** | **Rendement à 50% de charge** | **Rendement à 100% de charge** |
| :--: | :--: | :--: | :--: |
| 80 PLUS | 82% | 85% | 82% |
| 80 PLUS Bronze | 85% | 88% | 85% |
| 80 PLUS Silver | 87% | 90% | 87% |
| 80 PLUS Gold | 90% | 92% | 89% |
| 80 PLUS Platinum | 92% | 94% | 90% |
| 80 PLUS Titanium | 94% | 96% | 94% |

⚠️​ Attention, les fabricants peuvent mentir sur cette certification. Choisissez un fabricant et un vendeur réputés.

Pas besoin de chercher forcément une alimentation certifiée 80+ Titanium ou Platinum, Bronze c’est déjà très bien. 

Recommandation d’essayer de miser sur au moins le Gold.

## Boîtier

Et pour finir ... il faut bien assembler tous ces composants dans un boîtier !

Il existe de nombreux boîtiers conçus pour assembler des PC fixes, dans des tailles et des styles différents.

Quelques exemples par [ici](https://www.ldlc.pro/pieces/boitier-pc/c5418/)

### Dimensions du boîtier

Il existe différentes tailles de boîtiers

![34-Dimensions boîtier](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_34-Dimensions%20bo%C3%AEtier.png)

### Choix du boîtier

Il faut s’assurer que le boîtier soit compatible avec format de la carte mère (ATX, micro-ATX, mini-ITX, etc.) choisie.

⚠️​ Attention également à la taille de la carte graphique !

# Montage d’un PC

_Quelques conseils pour assembler/réparer des PCs de bureau !_

## Ordre d’assemblage

Peut varier en fonction du boîtier/des composants, mais en général :

1.	Installation de l'alimentation dans le boîtier
2.	Installation de la backplate de la carte mère
3.	Installation de la carte mère dans le boîtier et connexion du bloc d'alim à la CM
4.	Installation du processeur
5.	Application de pâte thermique
6.	Installation et connexion du ventirad
7.	Installation des barettes de RAM
8.	Installation des cartes d'extensions et autres composants (disques durs, lecteurs optiques, etc.)
9.	Connexion des composants à la carte mère et au bloc d'alim
10.	Connexion du "front panel" à la carte mère (voir ci-après)
11.	(optionnel) Installation de ventilateurs

![35-Montage PC](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_35-Montage%20PC.png)

## Front Panel 

En façade du boîtier, on retrouve plusieurs éléments : des ports USB, un bouton de mise sous tension, des LEDs, parfois un bouton pour réinitialiser (redémarrer) la machine, un connecteur pour micro/casque, etc.

On appelle ces éléments le **front panel** (on pourrait traduire ça en "panneau de façade", ou "panneau frontal").

Tous ces éléments vont devoir être connectés à la carte mère !

![36-Front Panel](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_36-Front%20Panel.png)

_Ça ressemble à ça, le front panel ! (sur la gauche, les câbles à connecter à la carte mère)_

![37-Connexions sur carte mère](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_37-Connexions%20sur%20carte%20m%C3%A8re.png)

_Là où il faut connecter les éléments sur la carte mère ! (cette position peut varier un peu d'une carte mère à l'autre)_

### Front panel : boutons et LEDs

C’est l’une des tâches les plus compliquées lors de l’assemblage d’un PC ! il faut se référer à la notice de la carte mère pour connecter correctement les LEDs et boutons (et parfois, le haut-parleur interne, mais ce haut-parleur a disparu de nos jours).

💡​ Les boutons n’ont pas de polarité : on peut les connecter dans un sens ou dans l’autre. Ce n’est pas le cas des LEDs. Si une LED ne s'allume pas, c'est qu'elle est connectée à l'envers.

![38-Boutons led](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_38-Boutons%20led.png)

## Cable management

Quand on assemble un PC, il est courant de faire attention au **cable management** (terme anglais qu'on pourrait traduire par "rangement du câblage").

💡​ Un boîtier bien câblé, c'est un boîtier qui est **mieux ventilé**, et donc **des composants mieux refroidis** !

Un bon câble management facilite également la maintenance !

![39-Mauvais cablement](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_39-Mauvais%20cablement.png)

_Un mauvais câble management !_ ❌​

![40-bon câble management](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_40-bon%20c%C3%A2ble%20management.png)

_Un bon câble management !_ ✅

_Aucun risque qu'un câble se coince dans un ventilateur, meilleure ventilation et maintenance facilitée._

Faire un bon câble management demande d'avoir un bon boîtier !

Les boîtiers modernes de bonne facture sont en général pensés pour faciliter le câble management (avec de l'espace prévu derrière la carte mère et des trous pour passer les câbles).

![41-câbles bien rangés](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_41-c%C3%A2bles%20bien%20rang%C3%A9s.png)

## Electricité statique

⚠️​ Attention : les composants électriques d’un PC sont sensibles à l'électricité statique !

Évitez de porter des vêtements en matière synthétique, privilégiez les vêtements en coton. Vous pouvez aussi toucher un élément relié à la terre (radiateur, tige de terre dans une prise 220V, etc.) pour vous "décharger".

![42-bracelet antistatique](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A1%20(Savoirs%20de%20base)/images%20A1/images%20A104/A104_42-bracelet%20antistatique.png)

_Il existe également des bracelets de mise à la terre, pour être sûr de ne pas détruire son matériel à cause de l'électricité statique._

## Aller plus loin

Il existe de nombreux tutoriels vidéo sur YouTube, n'hésitez pas à y jeter un oeil 🎥​👀​

---

