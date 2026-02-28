# 4️⃣🤝6️⃣ Session A310. SLACC, DHCPv6 & Cohabitation.

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

![[Pasted image 20251118153736.png]]
**Attention**, bien qu’il soit beaucoup moins probable qu’un bot scanne et tente d’attaquer une machine en IPv6, ça reste possible.
### IPsec
IPsec permet de chiffrer le contenu des paquets, il est utilisé avec certain VPN, notamment site-à-site.

IPv6 intègre nativement IPsec, contrairement à IPv4.
# Proxy
Un proxy (« mandataire ») est un composant informatique qui joue le rôle d’intermédiaire en se plaçant entre deux hôtes.
![[Pasted image 20251118153822.png]]
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
**Note** : On n’a pas eu le temps de parler de **routage dynamique** (protocole RIP et OSPF), il y a plein de vidéos sur YouTube qui en parlent.
