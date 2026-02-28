# 📍⁉️Session A302. Adresses MAC & Protocole ARP

## Notions
- Hub & Switch
- Adresse MAC
- Protocole ARP
- Un peu de sécurité : spoofing & ARP poisoning
## Programme de la journée
- Hub & Switchs (~1h)
- Adresse MAC (~1h)
- Protocol ARP (~1h30)
# Adresse MAC (Media Access Control)

Une adresse MAC (Media Access Control), parfois nommée **adresse physique**, est un identifiant physique stocké dans une carte réseau. Elle est **unique au monde** (en théorie).

**Toutes les cartes réseau**, quel que soit l’appareil dans lequel elles sont installées (PC, smartphone, console de jeu, frigo connecté, etc.), **ont une adresse MAC**.

Une adresse MAC est constituée de **6 octets** (48 bits). Les octets sont représentés sous leur **forme hexadécimale** (0 à F), et les octets sont **séparés par le caractère** :

Par **exemple** : 24 : 4B : FE : DE : 96 : 80
## IEEE
**Les adresses MAC sont attribuées par l’IEEE** (Institute of Electrical and Electronics Engineers), une organisation qui joue un rôle très important dans l’établissement des normes utilisées sur les réseaux informatiques.

[https://fr.wikipedia.org/wiki/Institute_of_Electrical_and_Electronics_Engineers](https://fr.wikipedia.org/wiki/Institute_of_Electrical_and_Electronics_Engineers)

L’IEEE est composées de **différents comités** : IEEE 802 par exemple est le comité qui a décrit les normes relatives aux réseaux locaux (LAN) et métropolitains (MAN). Ce comité a produit de nombreuses normes, comme par exemple **IEEE 802.3**, aussi appelée **Ethernet**.

Dans le cadre des adresses MAC, l’IEEE attribue un **préfixe de 24 bits** (les **3 premiers octets** de l’adresse) à chaque **fabricant** de cartes réseau. Il reste donc 24 bits utilisable pour chaque carte réseau produite par le fabricant, soit environ **16 millions d’adresses MAC disponibles par fabricant**/préfixe.

Ces 3 premiers octets sont appelés **OUI** (Organizationally Unique Identifier).
## Spoofing
L’adresse MAC est liée à la carte réseau, et en théorie, on ne peut pas la modifier.

Dans la pratique il est possible de la modifier logiciellement : on appelle ça le **spoofing d’adresse MAC**.

[https://fr.wikipedia.org/wiki/Institute_of_Electrical_and_Electronics_Engineers](https://fr.wikipedia.org/wiki/Institute_of_Electrical_and_Electronics_Engineers)
## Trame Ethernet
L’adresse MAC est utilisée comme **adresse source** et comme **adresse de destination** dans une trame Ethernet :
![[Pasted image 20251104115938.png]]
Ainsi, la carte réseau d’un appareil peut immédiatement **déterminer si une trame lui est destinée en regardant l’adresse de destination**.

Si la trame ne lui est pas destinée, elle sera ignorée (sauf en **mode Promiscuité**).
## Paquet
Et l’adresse IP, dans tout ça ?

L’adresse IP du destinataire et celle de l’émetteur sont dans la partie Data de la trame, à l’intérieur de ce qu’on appelle un **paquet**.
![[Pasted image 20251104120021.png]]
On dit que le paquet est **encapsulé** dans la trame.

Un **segment TCP** ou un **datagramme UDP** est encapsulé à l’intérieur du paquet, et les données sont encapsulées à l’intérieur du segment ou du datagramme.

TCP, UPD ? Ce sont encore des protocoles, on en parle après.

On nous reparle encore de couches, d’OSI, on voit ça quand ? Bientôt, mais avant, revenons quelques instants sur les Switches.

A **retenir** :
Un switch envoie les données uniquement à l’hôte de destination, grâce à son adresse MAC, contenue dans la trame envoyée par l’hôte source.

Pour reprendre notre analogie : si Alice veut envoyer une lettre à Bob, elle doit écrire l’adresse de Bob sur l’enveloppe.

Imaginons qu’Alice et Bob n’ont jamais communiqué auparavant, ils ne se connaissent même pas (ça arrive souvent, que deux appareils reliés à un réseau n’aient jamais communiqué).

Et bien Alice (plutôt son PC, sa carte réseau) va devoir utiliser le **protocole ARP**.
# ARP (Address Resolution Protocol)
ARP, pour **Address Resolution Protocol** (protocole de résolution d’adresse), est un protocole qui permet à une machine de **déterminer l’adresse MAC d’une autre machine à partir de son adresse IPv4**.

Ce protocole a été défini dans la RFC 826 de l’IETF, en 1982.
Ce protocole est donc **nécessaire au fonctionnement d’IPv4 sur un réseau Ethernet**.

Sans ça, Alice ne peut pas connaître l’adresse de Bob !

En IPv6, ARP n’est plus nécessaire (ses fonctionnalités sont remplacées par le protocole **NDP**).
## Fonctionnement
Reprenons nos personnages Alice et Bob : ce coup-ci, Alice veut récupérer un fichier sur l’ordinateur de Bob avec le protocole FTP.

Bob lui donne l’adresse IP de sa machine, sur un post-it.

Alice met l’IP de la machine de Bob dans son client FTP, et au moment de cliquer sur Connexion.
### Broadcast
La carte réseau d’Alice s’apprête à envoyer une trame Ethernet sur le réseau, mais il manque une dernière info… l’adresse MAC de la machine de Bob.

La carte réseau d’Alice envoie donc au préalable un message de **broadcast** (diffusion à **toutes** les machines du réseau). On appelle ce message une **requête ARP**.

« Quelle est l’adresse MAC correspondant à l’adresse IP 192.168.1.42 ? Repondez-moi sur mon adresse, 192.168.1.13. »

Toutes les machines du réseau reçoivent ce message. Si une carte réseau reconnait son adresse IP (celle de Bob, en l’occurrence), elle va répondre à celle de la machine d’Alice.

C’est moi, 24 :4B/FE/DE/96/80, qui ait l’adresse Ip 192.168.1.42 §

(Ce message est un message unicast, uniquement dédiée à la machine d’Alice).

A la réception de ce message, la machine d’Alice dispose donc maintenant de l’adresse MAC de la machine de Bob.

Les trames Ethernet peuvent maintenant être envoyées, Alice peut récupérer son fichier via **FTP**.

_Ça fait un paquet d’étapes, et beaucoup de bruit sur le réseau !_
### Cache ARP
A la réception de la réponse de Bob, la carte réseau d’Alice va stocker l’adresse MAC de la machine de Bob dans son **cache ARP**.

Si la machine d’Alice doit à nouveau envoyer des trames à celle de Bob, **pas besoin de refaire une requête ARP** !

De la même façon, la machine de Bob a stocké l’adresse MAC de la machine d’Alice lors de la réception de la requête ARP.

La machine de Bob n’a donc pas besoin elle-même de faire une requête ARP pour connaître l’adresse MAC de la machine d’Alice. Il est valable pour quelques secondes.

**Analogie** : Imaginons qu’Alice et Bob travaillent dans le même open-space :

Si Alice veut appeler Bob sur son téléphone fixe mais ne connait pas son numéro, il lui suffit de crier un peu fort « Hé Bob, tu peux m’appeler au 306 ? »

Bob n’a pas besoin de crier en retour, il peut appeler Alice et lui dire ‘Pour la prochaine fois, mon numéro c’est 472 ».

Imaginons que l’open-space est très grand, et qu’Alice n’a pas encore rencontré Bob ni travaillé avec lui. N’importe qui peut se faire passer pour Bob.

A cause de ça on peut avoir un attaque (ARP Poisoning)
## ARP Poisoning
L’attaque ARP Poisoning est une attaque dite Man-in-the-middle : un attaquant, en envoyant des fausses requêtes ARP, peut pousser une machine victime à rediriger tout son trafic à destination d’internet vers la machine de l’attaquant.

[https://fr.wikipedia.org/wiki/ARP_poisoning](https://fr.wikipedia.org/wiki/ARP_poisoning)
**Astuce** **rapide** quand on a un souci de réseau (connexion internet) :
Paramètres Windows> Connexions réseau:

Win > Exécuter (Win + R) et taper « **ncpa.cpl** »
Désactiver la carte réseau (désactiver ce périphérique réseau) et la réactiver
