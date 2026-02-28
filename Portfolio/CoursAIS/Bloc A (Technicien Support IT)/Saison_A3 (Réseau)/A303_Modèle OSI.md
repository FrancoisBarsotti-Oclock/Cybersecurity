# 📢 Session A303. Modèle OSI (Open Systems Interconnection)
Le modèle OSI (Open Systems Interconnection) est une **norme de communication, un modèle proposé par l’ISO** (Organisation Internationale de normalisation, en anglais : _International Organisation for Standardization**).**_

Ce modèle a été conçu dans les années 70, et présenté en 1978. La référence de la norme est **ISO 7498**, sa dernière révision date de 1994.
## Objectif
L’objectif de cette norme est de spécifier un **cadre général** pour la création de normes ultérieures cohérentes.

Le modèle OSI lui-même ne définit pas de services particuliers et encore moins de protocoles.
## 7 couches (à retenir par cœur)
A **retenir** !

Le modèle OSI propose une architecture en 7 couches, parfois réparties en deux groupes (couches basses/matérielles et couches hautes).
![[Pasted image 20251104151233.png]]
A chaque couche on va manipuler de différents types de…
![[Pasted image 20251104151259.png]]
On peut reprendre l’**analogie** de la carte postale et de l’enveloppe :
![[Pasted image 20251104151324.png#center]]
C’est exactement le même principe avec le modèle OSI : chaque couche « communique » avec la couche inférieure (ou supérieure). A chaque étape on a une **encapsulation**, ou une **désencapsulation** dans l’autre sens (ou encodage/décodage).

Autre **analogie** :
![[Pasted image 20251104151408.png#center]]
Les protocoles de chaque couche ne se préocuppent pas de ce qu’il se passe aux autres couches dans un même hôte (A, ci-dessous) : ils communiquent avec les couches correspondantes de l’hôte B.
![[Pasted image 20251104151452.png#center]]
## Echec du modèle OSI
A l’annonce de ce modèle, les grands opérateurs Européens l’ont combattu (notamment les PTT Français), lui préférant d’autres normes/standards comme par exemple Transpac (utilisé pour le minitel !). Le projet a donc pris du retard et n’est jamais devenu ce qu’il aurait pu être : le modèle sur lequel se base Internet.

En 1983 le réseau Arpanet remplace le protocole NCP par **la suite de protocoles TCP/IP**.
# TCP/IP
La suite de protocoles qui fait fonctionner Internet.

Regarder l’histoire de ARPANET

Jusqu’au 1er janvier 1983, Arpanet utilisait une suite de protocole appelée **NCP** (Network Control Program).

A cette date, NCP est devenu obsolète : Arpanet a basculé sur la **suite de protocoles TCP/IP**.

**Cette date marque le début de l’Internet moderne** (sans le Web).

**Attention** : **Internet** (la suite créée par le NCP) est différent de **Web** (un service sur internet créé en Suisse)
# Suite de protocoles
Cette suite est un **ensemble de plusieurs protocoles utilisés pour le transfert des données sur un réseau** local et /ou sur Internet.

On l’appelle **suite des protocoles Internet** ou simplement **suite TCP/IP**, du nom de ses deux premiers protocoles : **TCP** et **IP**.

Cette suite de protocoles a été mise au point par **Bob Kahn** et **Vinton Cerf**, chercheurs pour la DARPA.

Ils se sont inspirés des travaux du français **Louis Pouzin** sur le projet Cyclades.

La suite TCP/IP est définie dans la spécification **RFC N°1122**, publiée par l’**IETF** (Internet Engineering Task Force).
# OSI vs TCP/IP
Le modèle OSI peut être utilisé pour décrire la suite de protocoles Internet, **sans y être vraiment adapté** : TCP/IP, sur lequel est basé Internet, est **composé de seulement 4 couches**.

- **Couche Application** : regroupe les 3 couches supérieures du modèle OSI (Application, Présentation et Session). On y retrouve les protocoles http, FTP, DNS, etc.
- **Couche Transport** : équivalent à la couche 4 OSI. On y retrouve les protocoles TCP et UDP.
- **Couche Internet/Réseau** : équivalent à la couche 3 OSI. On y retrouve le protocole IP, ainsi que les protocoles ICMP, IGMP et ARP (discutable pour ces 3 derniers protocoles, plutôt situés entre les couches 2 et 3 pour ARP, et entre les couches 3 et 4 pour ICMP et IGMP).
- **Couche Accès Réseau** : équivalent à la couche 2 OSI + la couche 1 OSI. On y retrouve les protocoles Ethernet, WiFi, Token Ring, RNIS, Bluetooth, ZigBee, irDA, etc.
![[Pasted image 20251105112559.png]]
_On fait abstraction des techniques pour coder/transmettre un signal (radio, laser/optique, ADSL, etc.) dans le modèle TCP/IP_.

Ci-dessous l’évolution du modèle (à **retenir par cœur**) :

[modele_TCPIP_evolution.png (1221×468)](https://reussirsonccna.fr/wp-content/uploads/2014/10/modele_TCPIP_evolution.png)
![[Pasted image 20251105112633.png]]
# TCP/IP : Protocoles
Quelques protocoles qui composent la suite TCP/IP (liste non exhaustive) :
![[Pasted image 20251105112757.png]]
Nous avons déjà parlé de IP, Ethernet et ARP… Le protocole IP, on en a déjà parlé ! (dans sa version 4, on se garde l’IPv6 pour plus tard). Alors, là on s’intéresse aux autres deux protocoles, TCP et UDP.
## TCP (Transmission Control Protocol)
[Transmission Control Protocol — Wikipédia](https://fr.wikipedia.org/wiki/Transmission_Control_Protocol)

Prenons un exemple :

Alice veut envoyer un fichier via le protocole **FTP** à Bob. Voici la topologie du réseau et la liste de protocoles nécessaires :
![[Pasted image 20251105112851.png]]
Et la description du rôle de chaque protocole :
![[Pasted image 20251105112915.png]]
**FTP** demande à **TCP** d’ouvrir une connexion avec la machine de Bob, avec l’envoi d’un segment **SYN** :
![[Pasted image 20251105112942.png]]
* La machine de Bob répond positivement avec un segment **ACK** :
![[Pasted image 20251105113020.png]]
* La machine d’Alice confirme la demande de connexion en envoyant à son tour un segment **ACK:**
![[Pasted image 20251105113051.png]]
* Le protocole **TCP** découpe le fichier à envoyer en plusieurs segments, et commence à les transmettre :
![[Pasted image 20251105113138.png]]
### MTU

La taille des segments, et donc la taille des paquets IP et des trames Ethernet dans lesquels ils seront encapsulés, n’est pas illimitée.

La taille maximale d’un paquet pouvant être transmis en une seule fois, sans fragmentation/segmentation, s’appelle le MTU.

En général, sur le réseau IPv4/Ethernet, le MTU vaudra 1500 octets. Avec les ‘jumbo frames’ (trames géantes) on peut monter jusqu’à 9000 octets.
[Maximum transmission unit — Wikipédia](https://fr.wikipedia.org/wiki/Maximum_transmission_unit)

[Trame géante — Wikipédia](https://fr.wikipedia.org/wiki/Trame_g%C3%A9ante)

* Dès la réception des premiers segments, la machine de Bob envoi des segments pour confirmer leur bonne réception :
![[Pasted image 20251105113255.png]]
_Si certains segments/paquets se sont perdus en chemin, ils seront renvoyés par la machine d’Alice !_

* Une fois les derniers segments envoyés, la machine d’Alice demande la fermeture de la connexion :
![[Pasted image 20251105113319.png]]
* La machine de Bob **confirme** la **fermeture** de la connexion :
![[Pasted image 20251105113352.png]]
* Et pour finir, la machine d’Alice **confirme** elle **aussi** la fermeture de la connexion :
![[Pasted image 20251105113432.png]]
Toutes ces étapes peuvent augmenter la latence, et c’est là qui est intervenu l’UDP.
## UDP (User Datagram Protocol)
**UDP** est un protocole de la **couche Transport**, comme TCP.

Principale différence avec TCP : il n’y a aucun mécanisme de vérification, les données sont transmises directement, et peu importe si elles sont bien reçues ou pas.
[User Datagram Protocol — Wikipédia](https://fr.wikipedia.org/wiki/User_Datagram_Protocol)

Tous les usages pour lesquels une faible **latence** est primordiale !

- Jeux vidéos en réseau (notamment les **FPS** !)
- Voix sur IP
- Visioconférence (webrtc, et donc Slippers, c’est de l’UDP !)
- Streaming

TCP permet uniquement de faire de l’unicast (mono-diffusion) !

UDP est donc utilisé pour émettre des données à plusieurs machines simultanément, multicast (multi-diffusion) ou broadcast (diffusion).

Le protocole **DHCP**, par exemple, utilise UDP.
![[Pasted image 20251105113510.png]]
_Notre ordi reçoit des segments TCP et datagrammes UDP de différents protocoles en même temps, comment fait-il pour correctement réassembler les paquets ?_ Grâce aux **ports** !

N’importe quel segment TCP ou datagrame UDP envoyé par notre carte réseau (après son encapsulation dans une trame Ethernet) est marqué par un numéro du port.
### Un segment TCP :
![[Pasted image 20251105113617.png]]
### Un datagramme UDP :
![[Pasted image 20251105113643.png]]
Quand on va sur [https://www.youtube.com](https://www.youtube.com) avec un navigateur web, on utilise le protocole **HTTPS**, sur le port **443**.

Un site en **HTTP**, sur le port 80.

Si on envoie un fichier via **FTP** en même temps, le port sera probablement le **21**.

Si on administre un serveur via **SSH**, ce sera par défaut via le port **22**, avec Telnet, **23**.

La liste qu’il faudra **retenir** :
![[Pasted image 20251105113708.png]]
Pour plus de détails : [Ports 1 - 1024](https://www.vmaxx.net/techinfo/ports.htm) ou [Liste de ports logiciels — Wikipédia](https://fr.wikipedia.org/wiki/Liste_de_ports_logiciels)
## DHCP (Dynamic Host Configuration Protocol)

DHCP est un protocole (couche 7 du modèle OSI) bien pratique : il permet à une machine de récupérer automatiquement sa configuration réseau (adresse, masque de sous-réseau, serveur DNS, etc.) depuis un serveur DHCP.
[Dynamic Host Configuration Protocol — Wikipédia](https://fr.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)
### Fonctionnement
Alice arrive chez Bob et connecte son PC à sa box.

Dépourvue d’adresse IP, la carte réseau du PC d’Alice va envoyer un **datagramme UDP en broadcast** sur le réseau (toutes les machines vont le recevoir) :

_« il y a un serveur DHCP dans le coin ? »_
On appelle ça une requête DHCP **DISCOVER**.

Tout serveur DHCP recevant ce datagramme, s’il est en mesure de proposer une adresse IP sur le réseau, va envoyer une « offre DHCP » (DHCP **OFFER**) à la machine sur son port 68, en utilisant son adresse MAC.
_« Oui, moi, j’ai l’adresse 192.168.1.1 !_ 
_Tu peux utiliser l’adresse 192.168.1.10 »_

Le client retient la **première offre reçue** (il peut y avoir plusieurs serveurs sur un même réseau !), et diffuse à nouveau un datagramme DHCP **REQUEST** en broadcast, contenant l’adresse IP du serveur et l’adresse IP de l’offre proposée.

Ce datagramme fait office de **confirmation** pour le serveur qui a émis l’offre, qui va **assigner cette adresse à la machine**. Les autres serveurs sont informés que leur offre n’a pas été retenue par ce datagramme.

Le serveur DHCP envoie un dernier datagramme DHCP **ACK** (pour acknowledgement), sorte d’accusé de réception.

Ce datagramme contient à nouveau l’adresse IP mais également le masque de sous-réseau, l’adresse IP du serveur DNS, et l’adresse IP de la **passerelle** (on verra ça bientôt !).

Ce dernier message contient aussi la **durée du bail DHCP** : comme le bail d’un logement, un bail DHCP n’est pas infini, il faut re-signer de temps en temps !

_D’autres infos peuvent aussi être envoyées via DHCP, on y reviendra plus tard._
### CHCPv6
Le protocole DHCP dispose également d’une version compatible IPv6 : DHCPv6. Mais on en reparlera quand on découvrira IPv6 !
Si ça peut aider pour retenir penser à **DORA** (**D**iscover , **O**ffer, **R**equest, **A**cknowledge) .
### APIPA
[Automatic Private Internet Protocol Addressing — Wikipédia](https://fr.wikipedia.org/wiki/Automatic_Private_Internet_Protocol_Addressing)

**Rappel** : Pour que deux machines puissent communiquer, il faut que les deux aient la même adresse de réseau et même adresse de sous-réseau

Alors, deux machines qui soient en APIPA pourront communiquer entre elles, mais jamais avec les autres machines.
# Cisco IOS (Internetwork Operating System)
C’est le **système d’exploitation** produit par le fabricant d’équipements réseau **Cisco Systems**.

Ce système équipe la plupart des équipements fabriqués par Cisco (comme les téléphones IP qu’on a dans la plupart de bureaux, bornes WiFi, des pare-feux).
## IHM
Cisco IOS propose toujours au moins une interface homme-machine : une interface en **ligne de commande**.
Certains équipements peuvent aussi proposer une **interface web**, graphique.

Quand on est un pro de l’informatique, la **ligne de commande** c’est un outil qui nous rend beaucoup plus rapide que l'interface graphique (pour communiquer avec la machine).
Les commandes Cisco sont devenues un peu la norme.
## CLI (Interface en Ligne de Commande)
L’interface en ligne de commande (CLI) est accessible de 3 façons différentes :
- Via une liaison série, en connectant sa machine au port Console de l’équipement.
- A distance, via le protocole Telnet
- À distance, via le protocole SSH
### Par défaut
De base, seule la liaison série est possible : l’accès via SSH ou Telnet est désactivé par défaut
### Liaison série
Sur les équipements Cisco, le protocole utilisé par la liaison série s’appelle RS – 232 (aussi appelé EIA/TIA – 232).
### Câble « console »
Vu que nos PCs n’ont en général pas/plus de port **DB-9** (DE-9), il nous faut également adapteur DB-9 vers USB
![[Pasted image 20251105170839.png#center]]
### Equipements récents
Depuis quelques années, Cisco a remplacé le port console RJ45 par un port USB
![[Pasted image 20251105170921.png#center]]
_Le câble fourni par Cisco reste de couleur bleu ciel._
### Etablir la connexion
Sur notre PC, il nous faut une application capable de communiquer via une connexion série RS-232.

Cisco recommande d’utiliser le logiciel **PuTTY**.
PuTTY, c’est un petit logiciel gratuit qui sert principalement à se connecter en SSH, Telnet ou Serial à des machines distantes (Linux, routeurs Cisco, serveurs, etc.).

En bref :
SSH : pour ouvrir un terminal à distance sur un serveur Linux.

Telnet : ancien protocole, non chiffré.

Serial (COM) : pour se connecter en console à un routeur/switch via un câble console.

PuTTY te permet donc d’envoyer des commandes à une machine distante comme si tu étais devant elle.

### Port COM
Une fois le PC connecté à l’équipement réseau via le câble console, on va devoir identifier le **numéro de port COM** attribué à cette liaison série par Windows. Cette information peut être obtenue dans le Gestionnaire de périphériques, sous la section Ports (COM & LPT).
![[Pasted image 20251105171308.png#center]]
#### PuTTY
Dans PuTTY, cochez la case Serial, puis renseignez le numéro du port **COM** ainsi que la vitesse **9600**.
![[Pasted image 20251105171515.png#center]]
Un terminal va s’ouvrir, vous indiquant d’appuyer sur **Entrée** pour continuer :
![[Pasted image 20251105171556.png#center]]
Et on sera connecté à notre équipement Cisco !
#### Quel intérêt ?
Mais, pourquoi on aurait besoin de se connecter à nos équipements réseau ?

Les switches fonctionnent très bien de base !

Un switch est opérationnel dès le départ, mais si on veut activer/ configurer certaines fonctionnalités avancées, on devra utiliser la CLI ou l’interface web du switch !

Un autre équipement réseau très important devra aussi être configuré de la même façon : le routeur.
#### Fichiers de configuration
La configuration d’un équipement qui utilise le système Cisco IOS est stockée dans deux fichiers :

- Running-config : c’est la configuration actuellement utilisée par l’équipement
- Startup-config : c’est la configuration qui sera chargée au démarrage

La **running-config** est stockée dans la mémoire vive (RAM) de l’appareil : elle sera perdue à l’extiction de l’appareil.
#### Démarrage
Quand on l’allume, un équipement Cisco (switch ou routeur) va faire une copie du fichier de configuration **startup-config** dans le fichier **running-config**.

Le fichier startup-config est stocké dans la mémoire dite non-volatile de l’appareil : elle n’est pas perdue à l’extinction.
#### Modifications
Une fois connecté à l’équipement, les commandes lancées vont modifier le fichier running-config.

IOS a donc la particularité d’appliquer **immédiatement** chaque changement de configuration que nous allons effectuer.
#### Sauvegarde
Les modifications étant effectuées directement sur le fichier running-config, stocké dans la RAM, elles ne seront donc pas conservées au redémarrage.

Pour que les modifications soient « définitives », il faudra penser à faire une copie du fichier running-config vers le fichier startup-config.

On voit comment faire ça juste après !
#### Sur Packet Tracer
On peut tout à fait administrer nos équipements via le port console avec Packet Tracer !

Créez un nouveau projet, ajoutez un switch (2960) et un PC portable, et connectez le port console du switch au port RS232 du PC avec un câble console :
![[Pasted image 20251105171713.png#center]]
#### Moniteur série
Cliquez sur le PC portable, puis dans l’onglet Desktop lancez l’application **Terminal**.

Laissez les paramètres par défaut et cliquez sur **OK**.

On devra arriver sur cet écran :
![[Pasted image 20251105171757.png#center]]
On appuie sur Entrée, et nous allons pouvoir taper nos premières commandes !
#### Nos premières commandes
Première commande que nous allons taper : **?** .

Cette commande ( **?** ) permet de connaître les commandes disponibles dans le **mode** actuel.
#### Modes
Cisco IOS propose différents modes :
- **User** **EXEC** : c’est le mode auquel on va accéder quand on se connecte à l’équipement. Ce mode est très limité, il ne permet pas de modifier la configuration.
- **Privileged EXEC** (aussi appelé mode **Enable**) : on y accède avec la commande **Enable**. Ce mode permet des commandes beaucoup plus puissantes que le mode précédent :
   * Voir, sauvegarder/restaurer et supprimer la configuration de l’appareil
   * Installer une nouvelle image système
   * Redémarrer l’appareil
- **Global configuration** : on accède à ce mode depuis le mode Privileged EXEC avec la commande **configure terminal**. Ce mode dispose de différents ‘sous-modes » permettant de configurer différentes fonctionnalités de l’appareil. C’est le mode le plus « puissant ».
#### Quitter un mode
Les modes Privileged EXEC et Global configuration peuvent être quittés avec la commande **exit**.
![[Pasted image 20251105172026.png]]
#### Attention
En production, assurez vous que la configuration de l’équipement active (fonctionnelle, à priori) a bien été sauvegardée dans la configuration de départ **avant toute modification**.

Pour sauvegarder la configuration actuelle :
![[Pasted image 20251105172118.png#center]]
**copy run sta** (pour la version courte)
Dans le doute, posez la question à l’administrateur réseau !
