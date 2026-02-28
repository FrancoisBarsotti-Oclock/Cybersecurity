# 🌐 Session A301. Réseaux : Introduction

## Notions
- Canal de communication, diffusion/multi-diffusions
- Serveurs
- Baie de brassage/ baie informatique
- Ethernet
- Câbles à paires torsadée
- Hubs/switchs
## Programme de la journée
- Quelques rappels sur la S01 (~1h)
- Et ensuite, plein de slides, de schémas et d’échanges pour le reste de la journée.
- Challenge : petit exercice pratique sur packet tracer

Ce que nous allons apprendre sur la saison :
- Modèle OSI
- Suite de protocoles TCP/IP
- Adressage IPv4 & IPv6
- Fonctionnement & configuration de switchs et routeurs
- Paramétrage d’un pare-feu
- Gestion du WiFi
- Sécurité réseau
- Interconnexion de sites distants
- Mise en place serveurs VPN
- ….
# Réseaux Notions
## Equipements & Appliances
Comme on va le découvrir pendant cette saison, il existe plein de fabricants d’équipements et d’appliances réseau. De notre côté, on va se concentrer sur :

-les switchs et routeurs **Cisco**
-L’appliance routeur/pare-feu **pfSense**
## Outils utilisés
On va travailler avec un outil de simulation réseau : Cisco Packet Tracer
## Rappels SA1
![[Pasted image 20251103112051.png]]
Avec un masque à taille fixe (à **retenir par cœur**), on « coupe » l’adresse IP pile poil entre 2 octets !
## Exemple, avec **192.168.1.42**

Si on a **un masque /24**, on coupe entre le 3ème et le 4ème octet :
* Partie réseau : 192.168.1
* Partie machine : 42
* Adresse de réseau : 192.168.1.0 (on prend la partie réseau et on met les octets restants à 0)
* Adresse de broadcast : 192.168.1.255 (on prend la partie réseau et on met les octets restants à 255)
* Plage utilisable : 192.168.1.1 > 192.168.1.254
* Nombre de machine : 254 machines max 2^(32 – masque au format CIDR) – 2

Si on a un **maque /16**, on coupe entre le 2ème et le 3ème octet :
* *Partie réseau : 192.168
* *Partie machine : 1.42
* Adresse de réseau : 192.168.0.0 (on prend la partie réseau et on met les octets restants à 0)
* Adresse de broadcast : 192.168.255.255 (on prend la partie réseau et on met les octets restants à 255)
* Plage utilisable : 192.168.0.1 > 192.168.255.254
* Nombre de machine : 2^(32 – masque au format CIDR) – 2 = 65.534 machines

Si on a un **maque /8**, on coupe entre le 1er et le 2ème octet :
* Partie réseau : 192
* Partie machine : 68.1.42
*Adresse de réseau : 192.0.0.0 (on prend la partie réseau et on met les octets restants à 0)
* Adresse de broadcast : 192.255.255.255 (on prend la partie réseau et on met les octets restants à 255)
* Plage utilisable : 192.0.0.1 > 192.255.255.254
* Nombre de machine : 2^(32 – masque au format CIDR) – 2 = 16.777.214 machines

Pour les masques à taille variable (VLSM), pas le choix, il va falloir faire des calculs !

On a vu deux méthodes en SA1 :
- Méthode « **classique** », qui nécessite plein de conversions binaire/décimal
- Méthode du « **nombre magique** », qui ne nécessite presque pas de calculs et pas de conversion !

Quelle que soit la méthode, il faut retenir quelques petites choses par cœur ! (le nombre de masques, taille fixe, où on coupe l’octet… revoir minute 54)

Un masque de sous-réseau ne peut pas être composé de n’importe quelles valeurs, puisque tous les 1 doivent être à gauche et tous les 0 à droite dans sa notation binaire. Aussi car un masque est juste un octet converti en binaire. Chaque bit vaut une puissance de 2.

Les valeurs possibles d'un octet sont:
128 - 64 - 32 - 16 - 8 - 4 - 2 - 1
Alors, 255 en décimal = 1111 1111 en binaire, car 128+64+32+16+8+4+2+1 = 255

1111  1111 = 255 = /32
1111  1110 = 254 (-1) = /31 
1111  1100 = 252 (-2) = /30
1111  1000 = 248 (-4) = /29
1111  0000 = 240 (-8) = /
1110  0000 = 224 (-16)
1100  0000 = 192 (-32)
1000  0000 = 128 (-64)

S’il y a des calcules à faire dans l’examen, il faudra qu’on s’écrive cela dans un bout de papier pour pouvoir résoudre.

A partir de ça, on peut retrouver la correspondance CIDR – notation classique de n’importe quel masque !

Pour rappel, la notation CIDR c’est le nombre de bits à 1 dans le masque de sous-réseau (en notation binaire)

 /32 = 255.255.255.255
**/ 31 =**
**/30 =**
**/29 =**
**…**
/24 = 255.255.255.0
**…**
**/19 =**
**/18 =**
**/17 = =**
/16 = 255.255.0.0
/8 = 255.0.0.0
/0 = 0.0.0.0

![[Pasted image 20251103112541.png#center]]
On peut passer au calcule de notre numéro variable… Alors, rappel de la Méthode du nombre magique :
#### Premier exemple :
Ex : 10.42.153.87 /17

D’abord, on doit déterminer l’octet significatif dans le masque de sous-réseau.

S’il est au format CIDR, il faut le convertir dans son format « classique » en utilisant les infos à **retenir par cœur** ci-dessus.

/17 > 255.255.128.0

L’octet significatif, c’est là où intervient la « coupure » entre partie réseau et partie machine.

Ici, c’est 128.

On détermine ensuite le nombre magique en faisant 256 – octet significatif : 256 -128 = 128

On doit ensuite lister tous les multiples du nombre magique jusqu’à 256 : 0, 128, 256

Pour obtenir l’adresse de réseau, on remplace l’octet significatif dans l’adresse IP par le multiple du nombre magique inférieur ou égal à la valeur de cet octet. Dans notre cas, on remplace donc 153 par 128. On met tous les octets restants (à droite) à 0, pour obtenir l’adresse de réseau.

Adresse de réseau : 10.42.128.0

Pour l’adresse de broadcast, on remplace ce même octet par le multiple suivant – 1 ! Et on met tous les octets restants en 255

Adresse de broadcast : 10. 42.255.255

A partir de ça, on peut déterminer la plage utilisable : 10.42.128.1 > 10.42.255.254

Nombre de machines : 2^(32 – masque CIDR) – 2 : 2^15-2 = 32.766 machines max

#### Deuxième exemple :
Ex : 10.42.153.87 /28

/28 correspond à 255.255.255.240
Nombre magique = 256 -240 = 16
Multiples du nombre magique : 0, 16, 32, 48, 64, 80, 96, 112, … 256
Adresse de réseau : 10.42.153.80
Adresse de broadcast : 10.42.153.95
Plage utilisable : 10.42.153.81 > 10.42.153.94
Nombre de machines : 14

Voir [[Challenge SA03E03 Plan d'adressage]]
## Communication
**Règles de communication**

Quand deux systèmes (deux personnes, ou des machines) veulent échanger des informations, il existe de nombreuses méthodes de communication.

Toutes ces méthodes ont besoin de 3 éléments :
- Source du message (expéditeur)
- Destination du message (destinataire)
- Canal : support qui assure le cheminement du message de la source à la destination

Ces échanges de données, ces communications, sont régies par de règles appelées **protocoles**.

Les protocoles sont spécifiques au type de communication utilisé : les règles sont différentes pour communiquer via téléphone ou via courrier postal.

Avant de communiquer, deux personnes doivent se mettre d’accord sur la façon de communiquer.

Si la communication est vocale, par exemple, les partenaires doivent parler la même langue.

Les phrases utilisées devront être correctement structurées afin d’être comprises par le destinataire.

Pour que les informations soient transmises avec succès, les protocoles doivent tenir compte des exigences suivantes :
- Identification de l’expéditeur et du destinataire
- Utilisation d’une langue et syntaxe commune
- Vitesse et rythme d’élocution adapté
- Demande de confirmation / accusé de réception

Les protocoles utilisés dans les réseaux informatiques partagent ces caractéristiques fondamentales.

En plus d’identifier la source et la destination, les protocoles informatiques et réseau définissent la manière dont un message est transmis sur un réseau :
- Codage/encodage des messages
- Format et encapsulation des messages
- Taille du message
- Synchronisation des messages
- Options de remise des messages

Pour envoyer un message, il faut d’abord l’encoder.

L’encodage est le processus de **conversion des informations vers un autre format à des fins de transmission**. Le décodage est le processus inverse, permettant de récupérer le message d’origine.

Analogie : imaginez qu’Alice appelle Bob pour lui parler d’un beau coucher de soleil.

Pour faire passer ce message à Bob, elle convertit ses pensées dans un langage convenu au préalable, prononce des mots au moyen de sons, qui vont véhiculer le message. Bob écoute ces sons, les décode/sons, qui vont véhiculer le message. Bob écoute ce son, les décode/interprète pour comprendre le message reçu.

C’est la même chose en informatique !

Les informations sont encodées sous formes de bits (0 ou 1), puis ces bits sont envoyés sur un média (fil de cuivre, fibre optique, ondes, radio, etc .). L’hôte de destination reçoit le signal via ce média, et décode les bits reçus.

Autre analogie : l’envoie d’une lettre.

Alice souhaite envoyer une carte postale à Bob avec un petit message personnalisé. Alice écrit son message sur la carte postale, la met dans une enveloppe et ajoute l’adresse postale de Bob. Le processus de placer la carte dans l’enveloppe s’appelle l’**encapsulation**.

A l’arrivée, Bob devra ouvrir l’enveloppe pour récupérer la carte postale et lire le message d’Alice. C’est la **dés-encapsulation**.
![[Pasted image 20251103112641.png]]
En informatique, c’est pareil !

Les messages sont encapsulés dans un format spécifique, appelé trame, avant d’être transmis sur le réseau. La trame, qui fait office d’enveloppe, fournit l’adresse de destination souhaitée et l’adresse source, de l’expéditeur.
![[Pasted image 20251103112709.png]]
Le format et le contenu de la trame est déterminé par le type de message envoyé et par le canal de communication sur lequel ce dernier est transmis.

Les messages qui ne sont pas correctement formatés ne sont **ni livrés ni traités** par l’hôte de destination.

Quand Alice et Bob discutent, ils respectent une autre règle : la longueur des messages. Les phrases utilisées ne doivent pas être trop longues.

C’est pareil en informatique !  un message trop long sera décomposé en plusieurs messages, en plusieurs trames.

Les règles qui régissent la taille des trames transmises sont très strictes : une trame trop longue ou trop courte ne sera pas transmise. Ces restrictions peuvent différer selon le canal utilisé.

Les trames contiennent des informations permettant à l’hôte de destination de recomposer le message complet.

La synchronisation des messages est également très importante, et comprend les éléments suivants :
- **Contrôle de flux** : définit la quantité et la vitesse de transmission des données.
- **Délai de réponse** : faute de réponse dans un délai acceptable, on suppose qu’aucune réponse n’a été donnée et on réagit en conséquence.
- **Méthode d’accès** : définit le moment où un hôte peut envoyer un message (pour pas envoyer en même temps !)

**Destinataire(s)**, au pluriel, ou au singulier ?

Dans certains types de communication, on peut s’adresser à une ou plusieurs personnes en même temps.
![[Pasted image 20251103112734.png]]
C’est pareil en informatique !
![[Pasted image 20251103112918.png]]
A **retenir** :
- **Unicast** :  un seul destinataire sur le réseau
- **Multicast** : plusieurs destinataires sur le réseau
- **Broadcast** : message adressé à tout le monde

**Bi-directionnel ?** Ou monodirectionnel ?

En télécommunications, les communications ne sont pas toujours bi-directionnelles.

Exemple : la TV hertzienne ou la radio FM.

Un émetteur envoie des données, notre antenne TV les réceptionne. Notre TV n’a aucun moyen d’envoyer des informations à l’émetteur du signal.

On dit que c’est un **canal de communication simplex**.

Quand on peut communiquer dans les deux sens, on dit que c’est un **canal de communication duplex**.

Mails il n’est pas toujours possible de communiquer dans les deux sens **simultanément** ! Exemple : le walkie-talkie.

On parle dans ce cas -là d’un **canal de communication half-duplex**.

Si on peut communiquer **simultanément** dans les deux sens, comme par exemple par téléphone, on parle de **canal de communication full-dupplex**.

A **retenir** !
![[Pasted image 20251103112956.png]]
On est sur de Full-duplex la plupart de temps

## Architectures
A **retenir !**

Quand deux machines communiquent à travers un réseau, il y a deux architectures possibles :
- Client/serveur
- Pair-à-pair (peer to peer)

### Client/Serveur
Dans une architecture client/serveur, **le serveur fournit un service**. Par exemple, un site web, que **le client va pouvoir consommer**.

On peut faire l’anlalogie avec une terrace de café : le client demande au serveur de lui apporter une boisson, et le serveur répond à sa demande.
![[Pasted image 20251103113025.png#center]]
### Peer 2 peer
Dans une architecture peer to peer (paire-à-paire), les deux machines qui communiquent jouent à la fois le rôle de client et de serveur.

Exemple : le partage de fichiers ou d’imprimantes, le téléchargement via torrents, les logiciels comme eMule, etc.

L’incovénient du pair-à-paire ? Il n’ya a pas de question centralisée.
![[Pasted image 20251103113056.png#center]]
## C’est quoi, un serveur ?
En informatique, un serveur, ça peut désigner deux choses différentes :
- Un ordinateur (hardware)
- Un logiciel (software)
### Partie hardware
Quand on parle de serveur « hardware », ça ressemble à ça :
![[Pasted image 20251103113150.png]]
**Celui-ci, c’est un Dell PowerEdge r6615**
### Format
Le serveur ci-dessous a un format spécial, standardisé : c’est un **serveur « rack » de taille 1U**.

Ces serveurs »rackable » peuvent être installés dans des « racks », dans des sortes d’armoires :
![[Pasted image 20251103113225.png#center]]
### Armoires « rack »
Les dimensions de ces armoires « rack » sont normées. La largeur fait toujours **19 pouces**, d’où le nom « rack 19 pouces ».

Les hauteurs des équipements installés dans ces racks sont également standardisées : chaque équipent a une hauteur qui est un **multiple d’une longueur U** (pour unité de rack). 1U vaut 1,75 pouces (4,5 cm).

#### 1U, 2U, 3U, 4U

On trouve des serveurs et équipements réseau de différentes tailles, toujours multiple de la longueur U.
![[Pasted image 20251103113305.png]]
### Baie de brassage
Le même format de rack (19 pouces) est utilisé pour les **baies de brassages**. On en reparle un peu plus tard.
![[Pasted image 20251103113337.png#center]]

![[Pasted image 20251103113721.png#center]]

![[Pasted image 20251103114022.png]]

![[Pasted image 20251103114039.png#center]]
![[Pasted image 20251103114109.png]]

### A l’intérieur
Un serveur, c’est **comme un ordinateur classique** : on y retrouve une (ou plusieurs) alimentations, un (ou plusieurs) processeurs, des disques durs, de la RAM, des cartes d’extension, etc.

![[Pasted image 20251103114310.png]]

## Salle Serveur
Dans les grandes entreprises ou les PME, il y a en général une (ou plusieurs) « salles serveurs ».
![[Pasted image 20251103114338.png]]
On y retrouve un ou plusieurs racks 19 pouces, avec le cœur du réseau (on y reviendra) et les serveurs de l’entreprise.

![[Pasted image 20251103114404.png]]
### Onduleur
Un onduleur (ou **UPS**, _Uninterruptible Power Supply_) est un appareil qui fournit une **alimentation électrique de secours** à un ordinateur ou un équipement en cas de coupure ou de variation de courant.

Il sert à :
- **Éviter les coupures brutales** qui peuvent endommager le matériel ou les données ;
- **Stabiliser la tension** si le courant est instable ;
- **Permettre d’éteindre proprement** les systèmes pendant une panne.

Il contient généralement une **batterie** et un **convertisseur** qui transforme le courant continu (DC) de la batterie en courant alternatif (AC) utilisable.

Les serveurs doivent fonctionner 24h/24, 7jours/7. Il faut donc garantir leur alimentation électrique en permanence !
![[Pasted image 20251103121325.png#center]]
Pour éviter les coupures, on retrouve en général des **onduleurs**, qui peuvent aussi être au format rack 19 pouces, dans les salles serveurs.
![[Pasted image 20251103114601.png]]
#### Cables connexion d’Onduleur
![[Pasted image 20251103114625.png#center]]

#### Rallonges pour onduleur
![[Pasted image 20251103114707.png]]

### Control de serveur
![[Pasted image 20251103114828.png]]
Ces salles sont en général **climatisées**, et doivent pour des raisons de sécurité n’être **accessibles que par des personnels autorisés**.

Dans certaines entreprises, l’accès est restreint par digicode, lecteur d’empreinte digitale, la salle peut-être sous vidéo-surveillance, etc.

Les salles serveurs sont équipées de **systèmes anti-incendie**, parfois même avec extinction automatique (suppression de l’oxygène de la pièce).
## Différents formats
Il existe également des serveurs au format « tour » (comme l’unité centrales d’un ordinateur de bureau) :
![[Pasted image 20251103121429.png#center]]
On rencontre en général ces serveurs dans les TPE ou PME qui n’ont pas de salle serveur / pas de rack 19.
## Partie Software
Petit rappel, un « serveur », ça peut aussi désigner un logiciel !
### Serveur logiciel
Un logiciel « serveur », comme un logiciel classique, est installé sur un ordinateur.

Ce logiciel n’a pas forcément d’interface graphique : son but est de **rendre un service**, par exemple servir des pages web, pour un serveur web.
### Types de serveurs
Il existe autant de types de serveurs qu’il y a de protocoles client/serveur :
- Serveur web (protocole http) : publication de pages web, de sites internet
- Serveur de fichiers (protocole FTP, NFS, ou autre) : partage de dossiers et fichiers
- Serveur d’impression (protocole SMB) : partage d’imprimantes
- Serveur d’annuaire (protocole LDAP) : gestion de ressources (utilisateurs, groupes d’utilisateurs, machines)
- Etc.

On verra comment mettre en place tout un tas de logiciels serveurs pendant la formation.

En attendant, il faut déjà qu’on mette des machines en réseau !
## Ethernet
On fait comment, si on veut faire communiquer deux machines ensemble ?

On les relie avec un câble « ethernet », non ?
Ethernet est un **protocole de communication.**

[https://en.wikipedia.org/wiki/Communication_protocol](https://en.wikipedia.org/wiki/Communication_protocol)
### Normes Ethernet
Il existe plusieurs normes, plusieurs « versions » de ce protocole :
- 10BASE-T : débit maximum de 10Mbits/s
- 100BASE-T : max 100Mbits/s, aussi appelé **Fast Ethernet** (on ne le rencontre plus en entreprise car trop lent)
- 1000BASE-T : max 1Gbit/s, aussi appelé **Gigabit Ethernet (**en général la plus part de réseaux d’aujourd’hui y sont)
- 10GBASE-T : max 10Gbit/s, aussi appelé **10 Gigagit Ethernet**

Cette liste est loin d’être exhaustive, comme on peut le voir sur la page wikipédia ([Ethernet - Wikipedia](https://en.wikipedia.org/wiki/Ethernet))
**Modèle** **OSI** ? Sur la page wikipédia Ethernet on nous parle de couches, de modèle OSI.. Quesako ?
#### Câble & Connecteur

Ethernet, on vient de le dire, c’est un protocole. Quand on parle de « câble Ethernet », c’est en fait un abus de langage !
### Paires torsadées
Plutôt que de parler de « câble Ethernet », on devrait parler de **câble de paries torsadées**.

[https://en.wikipedia.org/wiki/Paire_torsad%C3%A9e](https://en.wikipedia.org/wiki/Paire_torsad%C3%A9e)

Bon en vrai, si vous appelez ça un câble Ethernet, ce n’est pas bien grave.
![[Pasted image 20251103145545.png#center]]
Les câbles de paires torsadées utilisés dans nos réseaux informatiques sont composées de **4 paires de 2 fils de cuivre**.
En entreprise, nous pourrions être emmenés à créer ses câbles.
### Types de câbles
Il existe différents types de câbles de paires torsadées, définis dans différentes normes (dont la norme ISO 11801).
![[Pasted image 20251103145643.png#center]]
- **U/UTP** (Unshielded/ Unshieleded Twisted Pair): Le moins cher, aucun blindage.
- **F/UTP** (Foiled / Unshielded Twinsted Pair): un peu plus cher, un blindage unique autour des 4 paires.
- **U/FTP** (Unshielded / Foiled Twinted Pair): blindage autour de chaque paire, pas de blindage autour des 4 paires.
- **F/FTP** (Foiled / Foiled Twinsted Pair): blindage autour de chaque paire, blindage autour des 4 paires.
- **S/FTP** (Screened / Foiled Twisted Pair): blindage autour de chaque paire, écran maillé autour des 4 paires.
- **SF/UTP** (Screened Foiled / Unshielded Twisted Pair): pas de blindage autour de chaque paire, blindage et écran maillé autour de 4 paires.
- **SF/FTP** (Screened Foiled / Foiled Twinsted Pair): blindage autour de chaque paire, écran maillé et blindage autour des 4 paires.
### Catégories de câbles
Il existe plusieurs catégories de câbles de paires torsadées que l’on peut utiliser sur un réseau informatique, les plus courants :

- Catégorie 5 : 100Mbits/ max, obsolète !
- Cat5e : 1Gbit/s, pas cher, il y en a partout. (Plus pop dans entreprises)
- Cat6 : 1Gbit/s, un peu plus cher !

Et pour la liste plus ou moins complète :
- Catégorie 3 : très vieux, plus vraiment utilisé sauf pour la téléphonie.
- Catégorie 4 : 10Mbit/s en Ethernet (16Mbit/s en Token Ring) (obsolète)
- Catégorie 5 : 100Mbit/s (obsolète)
- Catégorie 5e : 1Gbit/s, pas cher, rencontré fréquemment !
- Catégorie 6 : 1Gbit/s, un peu plus cher, intéressant pour la PoE
- Catégorie 6a : 10Gbit/s
- Catégorie 7 : 10Gbit/s + possibilité de faire passer un signal TV (TNT par exemple, vu que jusqu’à 600MHz). **Attention**, pas compatible avec RJ45, obligé d’utiliser un connecteur spécifique comme GG45 ou TERA.
- Catégorie 7a : 50Gbit/s sur 50m et 100Gbit/s sur 15m
- Catégorie 8 : connecteur RJ45 sur la version 8.1 (8.2 en TERA ou GG45), jusqu’à 40Gbit/s sur 30m.
### Connecteurs
![[Pasted image 20251103145801.png]]
### Grades
Dans les installations domestiques modernes, on peut rencontrer des câbles de paires torsadées qui n’appartiennent pas à une catégorie mais à un **grade** !

C’est une norme **UTE** (Union Technique de l’électricité), les câbles en « grade » sont imposées en France par la NF C 15-100 (norme élec des bâtiments).
- Grade 1 & 2 : 100Mbit/s max
- Grade 3 : 1Gbit/s + passage d’un signal TV TNT (900 MHz)
- Grade 3 S ou grade 3 TV : 10Gbit/s + passage d’un signal TV satellite (2200 MHz)
![[Pasted image 20251103145839.png]]
### Fabriquer un câble
T-568ª vs. T-568B
![[Pasted image 20251103145915.png]]
## Sertissage
Avec une pince pas chère, un manchon et un connecteur RJ45 (avec ou sans peigne) !
![[Pasted image 20251103145951.png#center]]
Acheter **avec peigne** c’est mieux
![[Pasted image 20251103150030.png]]
#### Pince à sertir
![[Pasted image 20251103150057.png#center]]

![[Pasted image 20251103150127.png]]
### Câble droit vs. Croisé
- Un câble droit, c’est le **mêle norme de connecteur RJ45 des deux côtés** !
- Un câble croisé, c’est **une norme différente de connecteur RJ45 de chaque côté** !
![[Pasted image 20251103150208.png]]

![[Pasted image 20251103150227.png]]

![[Pasted image 20251103150255.png]]
![[Pasted image 20251103150314.png]]
### Auto MDI-X
Les câbles croisés étaient en général **gris**, avec des **manchons de connecteurs RJ45 verts** !

Etaient ?
Avec l’Auto MDI-X, droit ou croisé, c’est de l’histoire ancienne.
Droit ou croisé, la plupart des carte réseaux modernes peuvent automatiquement s’adapter.

Ce qu’il faut **retenir** :

 Quand on veut câbler un connecteur RJ45, on a le choix entre deux normes :
- T-568A
- T-568B

-En Europe et en France, on utilise T-568B !
-Un **câble droit**, c’est la même norme des deux côtés, par exemple T-568B sur les deux connecteurs.
-Un **câble croisé**, c’est une norme différente des deux côtés (T-568A sur le premier connecteur et T-568B sur le deuxième.

Ok, on connecte deux machines avec un câble ~~Ethernet, RJ45~~, machins torsadés, et c’est bon, elles peuvent communiquer ?

Et l’adresse IP ?
On en a déjà un peu parlé : **IP est un protocole**, comme Ethernet !

**Toutes les machines qui communiquent sur les réseaux informatiques modernes on une adresse IP**.
Sur le [[Challenge SA03E01- Communication machines]], on peut voir comment lier deux plages informatiques et la vérification de la communication entre machines (à travers l'interface "Packet Tracer")
# Hubs (Concentrateurs) & Switches (Commutateurs)
(Début cours du 04/11)
![[Pasted image 20251104102508.png#center]]
Pour interconnecter plusieurs machines dans un réseau informatique, on va utiliser un appareil « central » : un **concentrateur (hub)** ou un **commutateur (switch)** !
![[Pasted image 20251104102548.png#center]]
# La différence entre les deux

# Le Hub (Concentrateur)

Le hub est juste une sorte de « multiprise » réseau : les données reçues sur un port du hub sont réexpédiées à tous les autres ports.

C’est totalement obsolète et il ne faut plus s’en servir en entreprise.
![[Pasted image 20251104102643.png]]
## Inconvénient du Hub
Les données sont transmises à tous les ports, donc à toutes les machines du réseau, que ces données leurs soient destinées ou non.

Le réseau est donc inutilement surchargé, les temps de réponse peuvent être plus longs.

Les hubs sont obsolètes depuis plus d’une dizaine ou vingtaine d’années.
# Le Switch (Commutateur)
Un switch, contrairement au hub, ne vas pas transmettre les données bêtement à tous les ports/toutes les machines !

**Il va uniquement transmettre les données au port sur lequel est connectée la machine destinataire**.
![[Pasted image 20251104102723.png]]
Ci-dessous un switch qu’on peut trouver dans nos maisons (non administrable, on s’y branche et c’est tout)
![[Pasted image 20251104105734.png]]
Il fait comment le switch pour savoir sur quel port est connectée la machine de destination ? Si je change de port ça fonctionne toujours !
## Tableau d’adresses
Le switch retient l’adresse de chaque machine, mais pas l’adresse IP, l’adresse **MAC** !

Pour l’instant il faut juste retenir que le switch retient l’adresse de chaque machine dans sa **table d’adresses MAC**. Quand il ne connait pas l’adresse, il envoie les données à tous les ports une seule fois pour déterminer sur quel port est connectée la machine de destination.
## Types de Switches
Il existe différents types de switch :
- Administrables ou non
- De niveau 2 ou de niveau 3 (on en parle après)
