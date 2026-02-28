# 🛜💡 Session A309. WiFi, vLAN, L3 Switches & IPv6

Notions
- WiFi (normes, sécurité, obligations légales)
- VLANs
- Switches de niveau 3
- Routage inter-vlan
- IPv6

Programme de la journée
- WiFi (1h)
- VLANs (1h)
- Switches L3 & routage inter-vlan (1h)
- IPv6 (2h)
- [[Challenge - VLAN & routage inter-vlan]]
# WiFi (Wireless Fidelity)
Le* WiFi (Wireless Fidelity), aussi orthographié wifi ou Wi-Fi, des fois au féminin la Wifi.

Le WiFi est un ensemble de **protocoles de communication sans fil** permettant de relier par **ondes radios** plusieurs appareils informatiques (ordinateur, routeur, smartphone, etc.) au sein d’un réseau informatique.

Ces protocoles sont définis par les normes du groupe **IEEE 802.11**.

Le terme « Wi-Fi » est en fait une **marque déposée par la Wi-Fi Alliance**, un organisme proposant aux fabricants d’appareils informatiques de **certifier que ces appareils sont conformes** à la norme 802.11, de vendre un **label** « Wi-F1 ».

On nomme souvent « réseau WiFi » un réseau répondant aux normes IEEE 802.11. On peut aussi parler de **réseau WLAN** (Wireless LAN).

Le standard IEEE 82.11 a été initialement publié en 1997.

Il existe à ce jour 5 versions de ce standard : 1997, 1999, 2007, 2012 et 2016 (ce sont les années de publication). Chaque version se base sur/remplace/étoffe la version précédente.

Les versions 2007, 2012 et 2016 incorporent également divers **amendements** (des modifications de morceaux/d’articles du standard ne nécessitant pas une nouvelle version complète).

La première version du standard 802.11 offres des débits maximum de 1 ou 2 Mbit/s.
## Normes Wi-Fi = amendements
Les **amendements** du standard 802.11 sont souvent, par abus de langage, appelées « normes » Wi-Fi.
### 802.11a (appellation commerciale « Wi-Fi 2 »)
Publié en 1999, « haut-débit » de 54Mb/s théoriques (27Mb/s réels) sur la bande de fréquence 5Ghz.
### 802.11b (appellation commerciale « Wi-Fi1 »)
Le plus répandu au début des années 2000. Débit théorique de 11Mb/s (6Mb/s réels) avec une portée théorique de 300 mètres. La plage de fréquence utilisée en celle des 2,4Ghz.
### 802.11f
L’amendement 802.11f propose le protocole _Inter-Access point roaming_ _protocol_, permettant à un utilisateur itinérant de **changer de point d’accès de façon transparente** lors d’un déplacement, quelles que soient les marques des points d’accès présents dans l’infrastructure réseau.

Cette possibilité est appelée **itinérance** (« **roaming** » )
### 802.11g (appellation commerciale « Wi-Fi 3)
Publié en 2003, l’amendement 802.11g offre un **débit plus élevé** (54Mb/s théoriques, 25Mb/s réels) en 2,4 GHz.
**Rétro-compatible** avec l’amendement 802.11b.
### 802.11n (appellation commerciale « Wi-Fi 4)
Ajoute le **MIMO** (Multiple-Input Multiple-Output) et l’**agrégation de porteuses** qui augmentent le débit.

Le 802.11n utilise les bandes de fréquences de **2,4 GHz** et/ou **5GHz**. Cet amendement permet de combiner jusqu’à deux canaux différents, ce qui permet en théorie d’atteindre une capacité totale de **600 Mbit/s** dans la bande des 5 GHz.
### 802.11r (Handover)
L’amendement 802.11r, publié en 2008, vise à améliorer la mobilité entre les points d’accès/bornes d’un réseau Wi-Fi.

Il permet notamment de **réduire le temps d’interruption** d’une communication en cas de « handover » : il permet à un appareil connecté de **basculer plus rapidement** (moins d’une seconde) et de façon plus fluide d’un point d’accès au suivant.
### 802.11s (Mesh)
L’amendement 802.11s vise à implémenter la mobilité sur les réseaux de type Ad-Hoc.

Le débit théorique atteint 10 à 20 Mbit/s. Tout point d’accès qui reçoit le signal est capable de le transmettre.
![[Pasted image 20251117100052.png]]
### 802.11ac (Wi-Fi 5)
IEEE 802.11ac, publié en 2014, permet une connexion Wi-Fi haut débit dans une bande de fréquences communément appelée « bande des 5 GHz ».
Le 802.11ac offre jusqu’à **1 300 Mbit/s de débit théorique**, en utilisant plusieurs canaux.
### 802.11ax (Wi-Fi 6/6E)
IEEE 802.11ax, publiée en 2021, peut fonctionner sur tout le spectre fréquentiel entre 1 et 7,1 GHz, si ces bandes sont disponibles, en complément des bandes 2,4 et 5 GHz déjà utilisées par les versions précédentes.
Les appareils présentés au CES 2018 atteignent une **vitesse maximale de 11 Gbit/s**.
### 802.11be
IEEE 802.11be (Extremely High Throughput: EHT) est l’un des prochains amendements prévus de la norme IEEE 802.11, et sera **probablement désigné sous le nom Wi-Fi7**. Il s’appuie sur la norme 802.11ax, avec des vitesses plus élevées (jusqu’à **46Gb/s**), en utilisant les bandes de fréquences 2,4,5 et 6 GHz.
Le développement de l’amendement 802.11be est en cours, avec pour objectif une version finale et approuvée de la norme prévue pour **décembre 2024**.
## Portée du Wi-Fi
En intérieur, la portée peut atteindre **plusieurs dizaines de mètres** (généralement entre une vingtaine et une cinquantaine de mètres) s’il n’y a aucun obstacle gênant (mur en béton par exemple) entre l’émetteur et l’utilisateur.

En extérieur, l’actuel **record** est détenu par Ernanno Pietrosemoli, président de la Fondation de l’école Latino-américaine de Redes, avec une **distance de 382 km**.

Il y des logiciels qui aident à cartographier la couverture du Wi-Fi (comme [Wi-Fi Heatmap Software - Visualize Coverage and Capacity | Ekahau](https://www.ekahau.com/solutions/wi-fi-heatmaps/)):
![[Pasted image 20251117105612.png]]
## Modes Wi-Fi
Il existe différents « modes » dans lesquels on peut configurer une carte WiFi (dans notre PC ou dans un routeur/point d’accès) :
- Infrastructure
- Client
- Ad-hoc
- Pont
- Répéteur
### Infrastructure
Mode qui permet de connecter les ordinateurs ou appareils équipés d’une carte WiFi à un **point d’accès**.
_Comme votre box, chez vous !_

En entreprise, plusieurs points d’accès (« bornes ») WiFi doivent être déployés pour assurer une couverture suffisante.
### Client
Le mode **client** est utilisé quand la carte WiFi d’un appareil se connecte à une autre carte WiFi en mode **infrastructure** (celle d’un routeur, d’un point d’accès, d’une « box »).
![[Pasted image 20251117105636.png]]
### Ad-hoc
Mode qui permet de **connecter directement les ordinateurs équipés d’une carte Wi-Fi sans utiliser de point d’accès**.

La mise en place d’un tel réseau consiste à configurer les machines en mode « Ad hoc » (au lieu du mode « Infrastructure »), la sélection d’un canal (fréquence), d’un nom de réseau (SSID, voir ci-après) communs à tous et si nécessaire d’une clé de chiffrement.
### Pont
Un point d’accès en mode « Pont » sert à connecter un ou plusieurs points d’accès entre eux pour **étendre un réseau filaire**, par exemple **entre deux bâtiments**.

La connexion se fait au niveau de la couche 2 OSI. Un point d’accès doit fonctionner en mode « Racine » et les autres s’y connectent en mode « Bridge » pour ensuite retransmettre la connexion sur leur interface Ethernet.
### Répéteur
Un point d’accès en mode « Répéteur » permet de répéter un signal Wi-Fi plus loin.

Un répéteur à tendance à **diminuer le débit** de la connexion, son antenne doit en effet recevoir un signal et le retransmettre par la même interface ce qui en théorie divise le débit par deux. Certains répéteurs on deux interfaces WiFi pour palier à ce problème.
## SSID (Service Set Identifier)
Le SSID est le nom d’un réseau sans fil selon la norme IEEE 802.11. Ce nom est constitué par une chaîne de caractères de 0 à 32 octets.

Les « box » des FAI ont un SSID pré-configuré afin de proposer un réseau Wi-Fi automatiquement. Le SSID et la clé d’accès qui y est associée par défaut sont renseignés dans le courrier de bienvenue de l’opérateur et/ou imprimés sur l’étiquette de la box.

La norme 802.11 autorise tout type de contenu pour le SSID, sans spécifier de type d’encodage. Certains anciens terminaux ne gèrent pas correctement les réseaux dont le SSID contient un espace, un caractère spécial, des lettres accentuées ou des caractères étrangers.

**La diffusion du SSID peut être désactivée** afin de masquer à l’utilisateur un réseau sans fil, cette mesure **ne constitue pas une mesure de protection du réseau** contre les intrusions. 
Cela complique un peu la tâche, mais un hacker déterminé va scanner les ondes radio aux alentours et va bien voir qu’il y a un réseau WiFi et va réussir à choper le SSID, s’il veut. Cependant, le fait de désactiver sa diffusion peut aider à tromper l’hacker, dans le cas où notre réseau soit son but.
C’est aussi différent que le **WiFi Pineapple**  (un point d’accès WiFi un peu élaboré, montré dans **Mr. Robot** et dans **How to Sell Drugs Online**) : on remplace avec lui un réseau publique/légitime (on se fait passer par ce réseau) pour que les gens se connectent à nous et nous laissent faire le rôle du « man-in-the-middle » et récupérer tout ce que le gens fait.
[WiFi Pineapple - Hak5](https://shop.hak5.org/products/wifi-pineapple)
## Clé WiFi
Différents « profils de sécurité » sont disponibles sur les points d’accès pour sécurises l’accès à un réseau WiFi.
## WEP (Wired Equivalent Privacy)
Le protocole **WEP** a été proposé en 1999 par le groupe IEEE 802.11 pour **sécuriser l’accès aux réseaux WiFi.**

Le WEP utilise initialement une clé de chiffrement d’une longueur de 64 bits, étendue par la suite à 128 bits. Cette clé (13 caractères ASCII ou 26 caractères hexadécimaux) est saisie par les utilisateurs qui souhaitent ses connecter au réseau.

Plusieurs faiblesses graves ont été identifiées par les cryptologues, et actuellement le WEP est cassable en moins d’une minute (source : ANSSI).

_On le surnomme en conséquence **Weak Encryption Protocol**._
Pour **cracker** le WEP on utilise la suite **Aircrack- NG** (le couteau suisse de l’hacker pour ce qui est du WiFi en WEP et WPA PSK -WPA 1 et 2)
[Aircrack-ng](https://www.aircrack-ng.org/)
Du coup, dès nos jours on ne doit plus utiliser du WEP en entreprise ; alors, il faudra de suite le changer si l’on le voit quelque part.
## WPA (Wi-Fi Protected Access)
**WPA, WPA2** et **WPA3** (Wi-Fi Protected Access) sont des mécanismes utilisés pour sécuriser les réseaux sans fil de type Wi-Fi.

La première version, WPA, a été créée au **début des années 2000** en réponse aux nombreuses et graves faiblesses que des chercheurs ont trouvées dans le mécanisme précédent, le WEP.

**WPA 2, 3, personnal, entreprise?**

Pour avoir un petit aperçu des différentes « normes » WPA, un petit tour sur [Wi-Fi Protected Access — Wikipédia](https://fr.wikipedia.org/wiki/Wi-Fi_Protected_Access)
### WiFi public
_Vous voulez offrir un WiFi gratuit pour les visiteurs de l’entreprise ?_

Il y a des **obligations légales** !
[Fournir un accès internet public : quelles sont vos obligations ? | CNIL](https://www.cnil.fr/fr/fournir-un-acces-internet-public-quelles-obligations)
# VLAN (Virtual LAN)

Un VLAN (Virtual LAN), ou réseau local virtuel, est un réseau informatique indépendant, **cloisonné** (ce réseau ne peut pas communiquer avec d’autres réseaux).
## Cloisonnement
Pour des questions de sécurité, on veut parfois éviter que deux réseaux puissent communiquer.

On peut cloisonner des réseaux (et les machines qui les composent) de **deux façons différentes** :
- Physiquement
- Logiquement
### Cloisonnement physique

Celui-là, vous le connaissez : il suffit de **ne pas relier physiquement, avec un câble**, les réseaux entre eux.
![[Pasted image 20251117121204.png]]
### Cloisonnement logique

Et si on vous disait qu’on pouvait arriver au même résultat mais avec **un seul switch ?**
![[Pasted image 20251117121222.png]]
Comme dans l’exemple précédent, LAN1 et LAN2 sont totalement séparés.

_Mais si on met les machines dans des sous réseaux différents, elle ne peuvent déjà pas communiquer !_
C’est vrai !

On pourrait mettre les machines du LAN 1 sur le sous-réseau 192.168.1.0/24 et les machines du LAN2 sur 172.16.0.0/24 (par exemple), elles ne pourraient pas communiquer.

Et si un utilisateur modifie les paramètres réseau de la machine ?

_Certains vont peut-être me répondre « On peut empêcher un utilisateur de modifier les réglages réseau d’une machine » !_

C’est vrai, mais si l’utilisateur démarre la machine sur un live CD ?

Si l’utilisateur déconnecte la machine et connecte l’une des siennes sur le port RJ45 brassé sur le switch ?

Vous l’aurez compris : segmenter notre réseau en plusieurs sous-réseaux ne suffit pas à réellement cloisonner des machines.

Quand on veut **cloisonner deux réseaux sans passer par un cloisonnement physique**, on va devoir utiliser les **VLANs**§

### Notre premier VLAN

Placez 4 machines (PC fixe, portable ou serveur) autour d’un switch 2960 sir Packet Tracer comme dans l’exemple précédent.
![[Pasted image 20251117121307.png]]
### VLAN par défaut
Connectons-nous à la CLI de notre Switch. Pour afficher les VLANs actuellement configurés, lancez la commande en mode privilégié :
**Show vlan**

On peut voir que par défaut, tous les ports du switch sont sur le **VLAN 1**.
### Création d’un VLAN
Première chose à faire, créer un nouveau VLAN !

Lancez les commandes ci-dessous :
**Conf t**
**Vlan 10**
**Name LAN1**
**exit**

Si on veut **supprimer** ce VLAN, on utilisera l’instruction **no vlan 10**.
### Attribution

Maintenant que les VLANs sont créés, il faut qu’on attribue les ports sur ces VLANs.

Lancez les commandes suivantes pour attribuer les 12 premiers ports au VLAN 10 et les 12 derniers ports au VLAN 20 :
![[Pasted image 20251117121407.png]]
**Pour le test** : Pour TSSR il y a toujours une machine qui est branchée sur un port sur le mauvais VLAN, alors soit il faut brancher la machine sur le bon vlan soit il faut changer la configuration du port.

**Pour AIS**, il faut savoir bien comment ça marche les VLANs, car on peut avoir des questions là-dessus du jury, comme c’est très important pour des questions de sécurité, sur le cloisonnement (que c’est la base, pour après rajouter d’autres sécu derrière). Alors, si jamais on ne sait pas expliquer ce que c’est un VLAN, c’est sûr qu’on n’aura pas le titre. Donc, il faut savoir qu’un **VLAN** c’est simplement **un LAN réseau virtuel, qui nous permet de découper nos switches en plusieurs marceaux** (en principe). Puis, on peut parler de cloisonnement logique (ils nous permettent de cloisonner le réseau sans avoir besoin de cloisonner physiquement avec deux switches séparés pour nos machines : on peut cloisonner logiquement, c’est-à-dire, découper notre switch en plusieurs parties qui ne vont pas pouvoir communiquer et quand même, si besoin, on peut faire communiquer ces différents sous-réseaux avec un routeur ou un Switch L3 en faisant du routage inter-vlan ; et le principe est le même que quand on avait deux LANs connectés à un routeur qui avait une pâte dans chaque LAN), vlan et liens Trunk (car c’est à ça que ça sert). Aussi, que les VLANs permettent d’optimiser le trafic réseau, comme ils segmentent les domaines de diffusion.
#### Tests
Assurez-vous que les machines du LAN1 sont sur les 12 premiers ports du switch et celles du LAN2 sur les 12 derniers.
![[Pasted image 20251117121430.png]]
Les machines du LAN1 devraient se pinger sans problème, idem pour celles du LAN2.

_ET si on change l’adresse de l’un des PC, et qu’on essaye de le passer dans l’autre LAN ?_

Ça ne ping pas !

On a **cloisonné logiquement** nos deux réseaux avec des VLANs.

Pour confirmer, on peut passer le port sur lequel la machine est connectée dans l’autre VLAN :
![[Pasted image 20251117121453.png]]
Essayez de relancer un ping, ça devrait fonctionner.
### Plusieurs Switches ?

_Et si on veut étendre ces VLANs à plusieurs switches sur notre réseau ?_
![[Pasted image 20251117121534.png]]
### Lien marqué
Un « lien marqué » (tagged link) est une interconnexion entre deux switches qui préservent l’appartenance aux VLAN de chaque trame Ethernet.

On va pouvoir, en quelque sorte, faire passer « plusieurs VLANs » dans un seul câble !

Chez certains fabricants (dont Cisco), on appelle cette liaison un lien trunk (tronc).

Attention avec les trunks, qui ne doivent pas être en évidence (un hacker aura accès à tout s’il se connecte physiquement sur un trunk).

Là, pour relier les switches ensemble, on a tout l’intérêt d’utiliser les ports les plus rapides (les deux GigabitEthernet).

Configuration Trunk

Sur les deux switches, lancez les commandes suivantes pour passer le port en mode trunk :
**Conf t**
**Interface gigabitEthernet 0/1**
**Switchport mode trunk**
**Exit**

Une fois ces deux commandes lancées, le ping devrait passer !
### Sécurité
Pour des raisons de sécurité, il est courant de mettre en place deux mesures :

- Changer le vlan de management/d’administration
- Restreindre les vlans autorisés sur un line trunk

### Management VLAN
Par défaut, le VLAN 1 est le VLAN de management.

Créons un nouveau VLAN pour l’administration et ajoutons une adresse IP sur cette interface virtuelle :
**conf t  
vlan 42  
name Management  
exit  
interface vlan 42  
ip address 10.42.0.1 255.255.255.0  
no shutdown  
exit**

_On peut retirer l’adresse IP du VLAN 1._
### Lien Trunk sur Management VLAN
Pour que nos liens trunk soient sur le bon VLAN, on doit lancer les commandes suivantes sur les deux switches :

**conf t  
interface gigabitEthernet 0/1  
switchport trunk native vlan 42  
exit**
### Restreindre les VLANs
Sur le lien Trunk, on peut faire en sorte de n’autoriser spécifiquement que nos VLANs :

**conf t  
interface gigabitEthernet 0/1  
switchport trunk allowed vlan 10,20,42  
exit**
### Intérêt ?
En plus de l’aspect “sécurité » apporté par le cloisonnement logique des VLANs, ils permettent aussi d’**optimiser le trafic réseau**.

## Domaine de diffusion
Certains protocoles nécessitent l’envoie de **trames de diffusion** (broadcast), c’est le cas de DHCP par exemple, ou ARP.

Les trames de diffusion sont **envoyées à tous les équipements du réseau** interconnectés par des switches. On appelle cet ensemble de machines un **domaine de diffusion**.

Quand une trame de diffusion est envoyée sur un domaine de diffusion, chaque machine à l’intérieur de ce domaine doit traiter la trame en question.

_Pas super optimisé !_

Les routeurs peuvent segmenter les domaines de diffusion, mais on ne va pas mettre des routeurs partout !

C’est là que les VLANs peuvent également servir : ils permettent de **segmenter les domaines de diffusion**.

_Et donc, indirectement, d’optimiser le trafic réseau !_
## VoIP & QoS

_Quand on parle d’optimisation…_

## VoIP

La **téléphonie sur IP** (Voice over IP) est très utilisée en entreprise. Les téléphones des collaborateurs, quel que soit leur niveau hiérarchique, sont tous connectés au même réseau que les ordinateurs, serveurs, copieurs et autres équipements réseau.

### Priorité

On voudrait éviter qu’un utilisateur perturbe l’appel important du Directeur en téléchargeant le dernier film à la mode, en occupant au passage toute la bande passante.

Pour ce faire, on va pouvoir prioriser le trafic VoIP par rapport au reste du trafic réseau. On appelle ça de la **QoS** (Quality of Service).

_On a pris le trafic VoIP dans cet exemple car c’est un cas d’école, mais on peut prioriser d’autres types de trafic réseau_.

# Switches de niveau 3

_Niveau dans le sens couche, du modèle OSI !_

## Niveau 2

Les commutateurs, communément appelés switches, sont des équipements qui travaillent à la couche 2 du modèle OSI.

En tout cas, ceux qu’on a utilisé jusque-là. Ils sont capables de diriger les trames Ethernet vers le bon port grâce à l’adresse MAC des machines et à leur table d’adresses MAC.

## Niveau 3

Certains switches sont dits « de niveau 3 » (Layer 3 switches, ou la version raccourcie L3).

Ces switches sont capables de travailler à la couche 3 du modèle OSI, comme les routeurs. Ils peuvent ainsi effectuer certaines opérations de routage basiques !

## L3 Switch vs. Routeur

_Mais du coup, on peut remplacer des routeurs par des switches ?_

Oui.

_Lesquels ?_

Ça dépend !

Souvent, les switches de niveau 3 vont être utilisés pour faire du **routage inter-VLAN**.

Ils sont la réponse à la question :

_Comment faire communiquer deux VLANs sans devoir ajouter un routeur ?_

### En pratique

_Reprenons l’exemple des slides précédentes :_
![[Pasted image 20251117135744.png]]
Si on veut faire communiquer nos deux VLANs, on peut utiliser un switch de niveau 3.
![[Pasted image 20251117135805.png]]
On utilise souvent des switches de niveau 3 pour le cœur de réseau.
Pour que le switch de niveau 3 puisse router les paquets, on a besoin d’une passerelle sur chaque sous-réseau, sur chaque LAN.

Lancez les commandes suivantes sur le switch de niveau 3 :

**conf t  
interface vlan 10  
description Passerelle LAN1  
ip address 192.168.1.254 255.255.255.0  
no shutdown  
exit  
interface vlan 20  
description Passerelle LAN2  
ip address 172.16.255.254 255.255.0.0  
no shutdown  
exit**
### Routage
Et enfin, pour activer le routage, on doit lancer les commandes suivantes :

**Conf t**
**Ip routing**

Les deux LANs peuvent maintenant se pinger !
# IPv6

IPv6 est la **version 6 du protocole IP** (Internet Protocol).

IPv6 est l’aboutissement de travaux menés par l’IETF depuis les années 90, pour succéder à IPv4.

La RFC 2460, qui définit ses spécifications, a été publiée en décembre 1998, puis remplacée par la RFC 8200 en juillet 2014.

Rappeler IPv

Rappeler les modèles OSI & TCP/IP

Lors d’une communication entre deux machines, les segments TCP ou **datagrammes UDP** (niveau 4) sont encapsulés dans des **paquets IP** (niveau 3), qui seront eux-mêmes ensuite encapsulés dans des **trames Ethernet** (niveau 2).

Les paquets IP contiennent des informations primordiales : l’**adresse IP de l’émetteur** et l’**adresse IP de destination** d’un paquet.

Avec la version 4 du protocole IP, une adresse IP était composée de **4 octets**, soit **32 bits** en tout.

Les octets sont séparés par le caractère. (point), et sont affichés en décimal (base 10).

Exemples :

- 8.8.8.8
- 192.168.1.42
- 172.16.0.13
- 51.91.236.192

### Pourquoi une V6 ? Pénurie d’adresses IPv4

On vient de le rappeler, pour communiquer, deux machines doivent avoir une adresse IP différente, unique.

Or avec IPv4, les adresses sont codées sur 32 bits, ce qui ne donne qu’**un peu plus de 4 milliards d’adresses disponibles** : ce n’est pas suffisant pour connecter l’ensemble des ordinateurs, smartphones, et autres objets connectés à Internet !

_On estime qu’en 2025, plus de 75 milliards d’appareils seront connectés à Internet_.

**Note :** Les objets connectés à la maison, il faut les mettre sur un VLAN séparé car on ne sait pas les données qu’ils envoient à leurs fournisseurs.

### NAT

Pour résoudre ce problème de pénurie, on utilise le mécanisme NAT (Network Address Translation) sur les routeurs.

Les machines à l’intérieur d’un réseau LAN disposent d’une **adresse IP privée**, qui leur permet de communiquer avec les autres machines du réseau local.

Si ces machines veulent communiquer avec une machine à l’intérieur du LAN (sur Internet, côté WAN), elles vont « emprunter » l’**adresse IP publique** du routeur.

## IPv6

Mais une autre solution existe : IPv6 !

Avec IPv6, les adresses IP sont codées sur **128 bits** (contre 32 en IPv4), ce qui donne **plus de 340 sextillions (3,4 x 10^38) d’adresses disponibles**.

Plus de problème de pénurie.

IPv6 corrige également d’autres limitations inhérentes à IPv4.

## Disclaimer

Pour bien comprendre le fonctionnement d’IPv6, il faut « oublier » ce que vous savez sur IPv4.

## Adresse IPv6

Une adresse IPv6 est codée sur **128 bits**.

Ces 128 bits sont séparés en **8 hextets**, 8 blocs de 16 bits.

Les hextets sont écrits en **hexadécimal**, sont composés de **4 caractères entre 0 et F**.

Les hextets sont séparés par le caractère :

Exemple :

**2001 : 0DB8 : CAFE : 0001 : 0000 : 0000 : 0000 : 0C30**

## Simplifier une adresse IPv6

En hexadécimal, les 0 au début d’un nombre peuvent être supprimés. On peut donc retirer les 0 qui débutent chaque hextet !

**2001 : 0DB8 : CAFE : 0001 : 0000 : 0000 : 0000 : 0C30**

Devient, après simplification…

**2001 : DB8 : CAFE : 1 : 0 : 0 : 0 : C30**

On peut à nouveau simplifier cette adresse : **les hextets consécutifs qui valent 0 peuvent être remplacés par : : !**

**2001 : DB8 : CAFE : 1 : 0 : 0 : 0 : C30**

Devient, après simplification….

**2001 : DB8 : CAFE : 1 : : C30**

**Attention** : on ne peut pas effectuer cette simplification qu’**une seule fois par adresse** !
![[Pasted image 20251117155314.png]]
### Masque de sous-réseau ?

Avec IPv6, il n’y a **pas de masque de sous-réseau**.

En revanche, les adresses IPv6 sont tout de même suivies par /xx, comme les adresses IPv4 CIDR.

### Partie réseau vs partie hôte

Le principe est le même qu’en IPVv4, une adresse IPv6 est composée de deux parties :

- Partie réseau, aussi appelé « préfixe »
- Partie hôte, aussi appelée « host ID »

Le bloc /xx correspond au nombre de bits du préfixe, de la partie réseau de l’adresse.

#### En image :
![[Pasted image 20251117155753.png]]
On « coupe » l’adresse en deux en fonction du bloc /xx. Pour rappel, chaque hextet fait 16 bits de long.

Plusieurs adresses ?

Avec IPv6, on **doit assigner plusieurs adresses à une même interface réseau**.

### Types d’adresses IPv6

Il existe plusieurs types d’adresses IPv6 :

- Multicast
- Anycast
- Unicast

Il n’existe plus d’adresses de broadcast comme en IPv4 !

#### Multicast
Le multicast en IPv6 permet d’envoyer des paquets vers plusieurs hôtes simultanément.
![[Pasted image 20251117155820.png]]
Une adresse IPv6 multicast commence obligatoirement par **ff** (les premières 8 bits). Toutes les adresses IPv6 multicast sont dans la plage d’adresses **ff00 : : /8**.

La même adresse multicast sera assignée à plusieurs interfaces réseau, sur plusieurs machines.

Adresses multicast : L’IANA réserve un certain nombre d’adresses multicast, par exemple :

- Ff02 : : 1 : tous les hôtes sur un même domaine de collision
- Ff02 : : 2 : tous les routeurs sur un même domaine de collision
- Ff02 : : fb : tous les serveur DNS

#### Anycast
Les adresses anycast sont une nouveauté de l’IPv6 !

Comme pour les adresses multicast, **une même adresse anycast sera assignée à plusieurs interfaces réseau**.

Mais les paquets ne seront envoyés qu’à l’**interface réseau/l’hôte le plus « proche »** (en utilisant des métriques de routage) !
#### Unicast
C’est le type qui va nous intéresser

Les adresses Unicast sont **les plus importantes !** il existe différents « sous-types » d’adresses IPv6 Unicast :

- **Loppback Address** : l’adresse de boucle locale, comme le 127.0.0.1 en IPv4.
- **Unique Local Address (ULA)** : l’équivalent des adresses IPv4 privées, non routable sur Internet mais routable à l’intérieur d’une organisation.
- **Link-local Address** : une adresse IPv6 “locale”, non routable (même en local)
- **Global Unique Address (GUA)** : IPv6 “publique”, routable sur Internet.
#### Loopback Address

L’adresse de boucle locale, qui **représente la machine sur laquelle on se trouve**, sera toujours **: : 1**.

C’est l’équivalent de l’adresse 127.0.0.1  en IPv4 !
#### Unique Local Address (ULA)
Les adresses ULA sont l’équivalent en IPv6 des **adresses IPv4 « privées »** définies dans la **RFC 1918** (pour rappel : 10.0.0.0/8, 192.168.0.0/16 et 172.16.0.0/12).

Ces adresses ne sont **pas routables sur Internet**, mais sont routables au sein du réseau local d’une organisation.

Les adresses ULA sont toutes dans la plage **fc00 : : /7**, c’est-à-dire qu’elles commençent obligatoirement par **fc** ou **fd**.

Les adresses ULA sont documentées par la RFC 4193.
#### Link-local Address
**Cette adresse est obligatoire** ! (toute interface sur laquelle la stack IPv6 est activée en aura forcément une)

Les adresses link-local sont dans la plage **fe80 : : /10**, le premier hextet peut donc aller jusqu’à **febf** (mais tout le monde utilise **fe80**).

**Attention** : **les adresses link-local ne sont pas routable, ni sur Internet ni en local !**

Les adresses link-local ont le même objectif que les adresses APIPA/ IPv4LL (RFC 3927) de la plage 169.254.0.0/16 !

Elles permettent à des machines sur le même domaine de diffusion de **communiquer sans avoir à faire aucune configuration**.

Elles sont **attribuées automatiquement** (on y reviendra) et uniques (sur un même domaine de diffusion)
#### Global Unique Address (GUA)
Contrairement à IPv4, avec IPv6 on dispose de suffisamment d’adresses pour que chaque machine puisse avoir une **adresse IP publique, routable sur Internet**.

Cette adresse s’appelle la Global Unique Adress (GUA).

Les plages d’adresses globales IPv6 sont **allouées/distribuées par l’IANA** (Internet Assigned Numbers Authority).

On peut consulter les plages actuellement allouées sur le site l’IANA. Comme pour l’IPv4, l’IANA délègue la gestion à des organisations régionales, les **RIR** (Regional Internet Reistries).

### Les 5 RIR :
![[Pasted image 20251117155934.png]]
En Europe, on dépend du registre **RIPE NCC**.

Une adresse Global Unique commence obligatoirement par un 2 ou un 3. Toutes les adresses GUA sont dans la plage **2000 : : /3**.

Sauf cas particuliers, toutes les adresses GUA ont une partie host/interface IDE d’une longueur de 64 bits.
![[Pasted image 20251117155959.png]]
La partie « network » de 48 bits est fixe, allouée à notre FAI par un RIPE (RIPE NCC, dans notre cas).

Certains FAI nous laissent la possibilité d’utiliser plusieurs sous-réseaux dans la partie « subnet ID » de 16 bits.

Chez Free, par exemple, on dispose de 8 sous-réseaux en /64 !
D’autres FAI donnent un préfixe en /48, nous laissant totalement libre sur la partie sous-réseau.
![[Pasted image 20251117160145.png]]
On peut dans ce cas créer **216** (soit 65.536) **sous-réseaux**, contenant chacun **264** (soit 18.446.744.073.709.551.616) **machines**.

### Loopback & link-local
Ajoutez un PC, et vérifiez sa configuration réseau IPv6 :
![[Pasted image 20251117160641.png]]
On peut voir que le PC a bien une adresse link-local, obligatoire, qui lui a été automatiquement attribuée (on verra par la suite comment)

-----------------------
18/11/2025 (Partie 2)
Notions

- Attribution IPv6 : SLACC, DHCPv6, adressage,
- Cohabitation IPv4/IPv6
- Sécurité IPv6
- Proxy

## Attribution

Elles viennent d’où, ces adresses IPv6 ?

Comme en IPv4, on peut choisir d’attribuer des **adresses IPv6 statiques** à nos machines, ou alors utiliser un **mécanisme d’attribution automatique** :

- **SLAAC** (sans état, Stateless Automatic Auto Configuration)
- **DHCPv6** (avec état, Stateful)
- Une combinaison des deux !

### SLAAC

SLAAC (Stateless Automatic Auto Configuration) utilise les messages du **protocole NDP** (Neighbor Discovery Protocol) pour déterminer la configuration réseau (adresse IPv6, next hop, etc.) à utiliser.

Avec SLAAC, **pas besoin de serveur** ! SLAAC EST DIT « **sans état** » (stateless), aucun journal ne conserve une trace de l’adresse automatiquement attribuée par une machine.

Le protocole **NDP** utilise des paquets **ICMPv6**, qui est un protocole indispensable avec IPv6.

NDP utilise 5 types de paquets **ICMPv6** :

- **Router Solicitation (RS), type 133** : permet de demander à tous les routeurs présents d’envoyer un **Router Advertisement**.
- **Router Advertisement (RA), type 134** : permet au routeur de donner différentes informations (le préfixe à utiliser, par exemple) aux machines qui lui sont connectés.
- **Neighbor Solicitation, type 135** : remplace ARP (permet de déterminer l’adresse MAC), permet de vérifier si un équipement est en ligne et si une adresse IPv6 est déjà utilisée.
- **Neighbor Advertisement, type 136** : réponse à un paquet **Neighbor Solicitation**.
- **Redirect, type 137** : permet aux routeurs de signaler aux hôtes qu’un meilleur chemin existe pour une destination.

[Internet Control Message Protocol — Wikipédia](https://fr.wikipedia.org/wiki/Internet_Control_Message_Protocol)

### DHCPv6

DHCPv6, qui nécessite un serveur (avec état, stateful), reprend les fonctionnalités du protocole DHCP pour IPv4, et propose également de nouvelles fonctionnalités.

DHCPv6 peut par exemple fournir certaines informations supplémentaires à des machines ayant configuré automatiquement leur adresse par SLAAC.

#### Cas particulier : EUI64

Chez Cisco, par défaut, SLAAC utilise EUI-64 (Extended Unique Identifier) pour auto-attribuer les adresses IPv6 unicast.

Cette méthode se base sur l’adresse MAC de l’interface réseau :

- On part d’une adresse MAC sur 48 bits, exemple FC : 99 : 47 : 75 : CE : E0
- On inverse le 7ème bit : FC = 1111 1100 -> 1111 1110 = FE

L’adresse MAC devient donc FE : 99 : 47 : 75 : CE : E0

- On insère FFFE en plein milieu de l’adresse -> FE : 99 : 47 : FF : FE : 75 : CE : E0
- On utilise l’EUI64 obtenu comme host ID de l’adresse IPv6 -> 2001 : B0B : CAFE : 1 : FE99 : 47FF : FE75 : CEE0

Cette méthode a un inconvénient : elle divulgue l’adresse MAC

### Adressage statique

Comme en IPv4, on peut toujours **choisir nous-même l’adresse IPv6 d’une interface réseau** (sa partie host ID, en tout cas).

De toute façon, on ne peut pas faire n’importe quoi non plus car l**es adresses qui se terminent par 0 sont réservées** : seules les interface des routeurs peuvent déroger à la règle.

En revanche, il n’y a pas d’adresse de broadcast réservée, comme c’était le cas avec IPv4.

**Astuce** : si on utilise IPv4 et IPv6 simultanément (on en parle juste après), on peut utiliser l’adresse IPv4 d’un hôte comme host ID pour construire son adresse IPv6 !

Exemple :

- IPv4 : 192.168.1.10
- IPv6 : 2001 : B0B : CAFE : 1 : 192 : 168 : 1 : 10

## Cohabitation IPv4/v6

Pourquoi cohabiter, pourquoi ne pas passer à 100% sur IPv6 ?

### Compatibilité

Certains services sur Internet ne sont pas (encore) accessibles en IPv6, et **ne sont disponibles qu’en IPv4**.

Sans adresse IPv4, notre machine ne pourrait pas consommer ces services !

Mais pas de panique, tout a été prévu lors de la conception d’IPv6.

### Double stack

La plupart des appareils et systèmes d’exploitation disposent de **deux stacks** (piles) :

- Une stack IPv4
- Une stack IPv6

Chaque interface réseau peut donc avoir une adresse IPv4 et une (ou plusieurs !) adresse(s) IPv6. **L’adresse IPv6 est utilisée en priorité** !

### Tunnel 6to4

Il est possible, dans certains cas, de passer d’un réseau IPv6 à un autre réseau IPv6 en traversant un ou plusieurs réseaux IPv4.

Pour cela, le **paquet IPv6 est encapsulé dans un paquet IPv4**.

### NAT (inutile)

Plus nécessaire dans un monde où tout serait en IPv6, il peut encore servir pour des raisons de compatibilité !

En sortant d’un réseau IPv6, on peut configurer un routeur pour qu’il nous prête une adresse IPv4.

## Sécurité de IPv6

IPv6, moins sécurisé qu’IPv4 ?

Pas de NAT ?! On peut parfois lire ou entendre ce genre d’affirmations :

_IPv6 c’est moins sécurisé, il n’y a pas de NAT, toutes les machines sont routables sur Internet !_

**Cette affirmation est fausse, le NAT n’est en aucun cas un mécanisme de sécurité !**

Effectivement, il n’y a pas de NAT en IPv6.

Pour **rappel**, le NAT permet à une machine n’ayant pas d’adresse IPv4 publique d’accéder à Internet en « empruntant » une adresse IPv4 publique au routeur.

En IPv4, une machine derrière un NAT n’est **en théorie pas joignable si aucune redirection de port n’a été configurée sur le routeur**. Cela dit, des mécanismes comme UPnP peuvent automatiquement créer ces redirections de ports…

Sans oublier qu’il existe d’autres types de NAT que celui généralement rencontré sur nos box.

Qu’on soit en IPv4 ou en IPv6, notre réseau et nos machines doivent être protégées par des pares-feux.

**Un pare-feu au niveau du routeur**, qui filtre toutes les entrées/ sorties pour tout le réseau local, et **un pare-feu sur chaque machine**.

**Le NAT n’est pas un pare-feu**.

Par défaut sur un pare-feu, qu’on soit en IPv4 ou IPv6, **on bloque en général tout le trafic entrant non sollicité**.

On débloque ensuite port par port, en fonction des services que l’on souhaite exposer sur Internet ou sur un réseau local.

Et si on veut se mettre en « mode parano », on peut aussi bloquer tout le trafic sortant (et débloquer selon les besoins, mais c’est très contraignant).

### Scan automatisé

Sur Internet, de nombreux bots passent leur temps à « scanner » des plages d’adresse IP à la recherche de services non sécurisés / non mis-à-jour.

Ces bots utilisent (entre autres) le logiciel **nmap**. Un nmap très basique prend en général à peine plus d’une seconde.

Si on considère que scanner un hôte prend 2s, un bot pourrait scanner 43.200 adresses IP en 24h.

Ces bots font souvent partie de réseau de machines zombies (botnets) qui comptent plusieurs centaines voire milliers de machines.

Pour un botnet de 1000 machines, ça fait donc 4.320.000 adresses scannées en 24h.

L’ensemble des 4 milliards d’adresses IPv4 sur Internet serait scanné en moins de 3 ans.

Pour scanner l’ensemble des adresses IPv6 à cette vitesse, voici le nombre d’années requises :
![[Pasted image 20251118110327.png]]
**Attention**, bien qu’il soit beaucoup moins probable qu’un bot scanne et tente d’attaquer une machine en IPv6, ça reste possible.

### IPsec

IPsec permet de chiffrer le contenu des paquets, il est utilisé avec certain VPN, notamment site-à-site.

IPv6 intègre nativement IPsec, contrairement à IPv4.

# Proxy

Un proxy (« mandataire ») est un composant informatique qui joue le rôle d’intermédiaire en se plaçant entre deux hôtes.
![[Pasted image 20251118110355.png]]
Le proxy est en général un logiciel, qui intervient à la couche 7 (application) du modèle OSI.

La commande traceroute (niveau 3) ne permettra donc pas d’identifier le proxy.

Les serveurs proxy peuvent assurer diverses tâches :

- Accélérer la navigation web (cache, compression)
- Journaliser les requêtes (historique, logs)
- Filtrer (blocage de certains sites, de certains contenus)
- Rédiger le trafic (proxy inverse / réserve proxy)
- Répartir la charge

Nous allons voir les trois premiers. Nous devons installer un proxy transparent.

## Journalisation

Rien ne vaut une démo !

Mettons en place un **proxy** « **transparent** » (il ne nécessite aucune configuration sur les machines clientes) avec **Squid sur pfSense** !

La doc que l’on va suivre : [Proxy transparent : mise en place de Squid sur PfSense | IT-Connect](https://www.it-connect.fr/proxy-transparent-mise-en-place-de-squid-sur-pfsense/)

### Portail captif

Le **portail captif** est une technique consistant à forcer les clients http d’un réseau à afficher une **page web spéciale** (le plus souvent dans un but d’authentification) avant d’accéder à Internet normalement.

Cette technique est généralement mise en œuvre pour les accès Wi-Fi mais peut aussi être utilisée pour l’accès à des réseaux filaires (ex. hôtels, campus, etc.).

--------------------------

**Information à part (culture)** : Un Kernel Panic (ou panique du noyau) est l'arrêt d'urgence du système d'exploitation, déclenché par le noyau (le cœur du système) lorsqu'il détecte une erreur critique qu'il ne peut pas résoudre ou dont il ne peut pas se remettre (écran bleu).

------------------------- 
