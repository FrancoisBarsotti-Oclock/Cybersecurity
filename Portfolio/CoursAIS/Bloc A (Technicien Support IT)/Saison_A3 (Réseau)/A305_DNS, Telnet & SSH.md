# 🌎🚩 Session A305. DNS, Telnet & SSH

Notions
- Commande tracroute / tracert
- Relai DHCP
- Protocole DNS
- Telnet & SSH

Programme de la journée
- Commande traceroute, relai DHCP et protocole DNS 
- SSH & Telnet

**Commande traceroute :** Pour montrer la trajectoire d’un point A à un Point B (à travers les routeurs, il ne montre jamais les Switches). C’est moins utile que la commande Ping.**  
  - sur MacOS/Linux : traceroute  
- sur Windows : tracert**
# Protocole DNS (Domain Name Service)
On peut créer un enregistrement de type A (Google.com = 1.2.3.4)
## Types d'enregistrements DNS :
- type A : faire matcher un nom de domaine avec une adresse IPv4
- type AAAA : faire matcher un nom de domaine avec une adresse IPv6
- type CNAME : alias, fait matcher un nom de domaine avec un autre nom de domaine
# Telnet & SSH
Le port console, c’est bien, mais administrer un équipement à distance, c’est mieux !

Telnet et SSH sont deux protocoles permettant d'administrer des équipements réseau ou des ordinateurs à distance.

Ces deux protocoles vont nous permettre de lancer des commandes sur les équipements en question, comme si on était connecté physiquement à leur port série.
![[Pasted image 20251112174318.png]]
## Telnet
Telnet (terminal network ou télécommunication network, ou encore télétype network) est un protocole utilisé sur les réseaux TCP/IP, permettant de communiquer avec un serveur distant en échangeant des lignes de texte et en recevant des réponses également sous forme de texte. [Telnet — Wikipédia](https://fr.wikipedia.org/wiki/Telnet).

Telnet est à la **couche 7 du modèle OSI**. Ce protocole est normalisé par l'IETF (RFC 15, 854 et 855).

Telnet utilise **TCP** pour le transport, sur le **port 23** par défaut.

**Attention**, le protocole Telnet est **obsolète** ! On va éviter de l’utiliser.

Les données sont échangées **en clair** entre le client et le serveur. Il faut donc éviter de l’utiliser en production.
## SSH (Secure Shell)
C’est un protocole de communication **sécurisé**. Il impose un **échange de clés de chiffrement** en début de connexion et **tous les segments TCP sont chiffrés** par la suite.

Il devient donc impossible d’utiliser un analyseur de paquets (sniffer) pour voir ce que fait l’utilisateur. [Secure Shell — Wikipédia](https://fr.wikipedia.org/wiki/Secure_Shell)

Le protocole SSH a été conçu avec l’objectif de remplacer les différents protocoles non chiffrés comme **rlogin, telnet, rcp** et **rsh**.

Comme Telnet, SSH est à la **couche 7** du modèle OSI, et utilise le **port TCP 22**.
## Telnet/SSH & Cisco IOS
Regardons comment configurer SSH (ou Telnet, mais on évitera en prod’) sur un équipement Cisco IOS.

Pour cette démo, nous connectons simplement un PC à un switch, avec les adresses suivantes :
![[Pasted image 20251112174604.png]]
## VTY lines
Les équipements Cisco disposent d’un certain nombre de **lignes « VTY »**. Le nombre de ces lignes de terminal virtuelles définit combien de **connexions concurrentes/simultanées** Telnet (ou SSH) sont supportées par l’équipement.

Pour déterminer le nombre de lignes VTY disponibles, lancez la commande suivante en mode Global Configuration :
![[Pasted image 20251112174637.png]]
Un switch 2960 dispose de 16 lignes VTY, par exemple.
## Protocoles supportés
Pour activer Telnet ou SSH, on va devoir choisir une ou plusieurs lignes VTY.

Pour sélectionner les 5 premières lignes et consulter tous les **protocoles supportés** :
![[Pasted image 20251112174708.png]]
_Telnet et SSH seulement pour le 2960, mais certains équipements supportent d’autre protocoles !_
### Configuration Telnet
Activons Telnet sur les 5 première lignes VTY :
![[Pasted image 20251112174737.png]]
On doit ensuite **configurer l’authentification**. Ajoutons un mot de passe avec les commandes (toujours dans la configuration des lignes VTY) :
![[Pasted image 20251112174813.png]]
Remplacez rocknroll par le mot de passe de votre choix.

Sur le PC, ouvrez l’application Command Prompt et lancez la commande suivante pour établir la connexion :
![[Pasted image 20251112174841.png]]
Saisissez le mot de passe renseigné à l’étape précédente quand il vous es demandé.

Nous sommes connectés à distance à notre switch et pouvons lancer des commandes, comme avec un câble console.

On peut aussi utiliser **PuTTY**
### Telnet vs. Hackers
Spoiler alert : Telnet va perdre.

Imaginons que notre réseau n’est pas sécurisé contre l’**ARP poisoning**, par exemple (on verra un peu plus comment protéger nos réseaux contre cette attaque).

On en avait parlé, cette attaque permet à un hacker de faire du **Man-in-the-middle** : intercepter tous les paquets entre notre PC en un équipement.

Pour simuler cela, on peut utiliser le **Sniffer** sur Packet Tracer. Il se trouve dans la section End devices.
![[Pasted image 20251112174915.png]]
Placez-le entre le PC et la Switch, **faites attention à connecter le PC sur le port Ethernet0**.

Cliquez sur le sniffer et allez sur l'**onglet GUI**.

Cliquez sur Show All/None, puis sur Edit Filters. Dans la catégorie Misc, cochez Telnet.

Gardez la GUI du Sniffer ouverte, et relancez la connexion au switch via Telnet sur le PC.

Vous devriez voir plusieurs paquets arriver quand vous allez taper le mot de passe. Regardez le contenu de ces paquets, tout en bas, section TELNET > TELNET DATA.

_Le hacker voit le mot de passe !_

On l’a dit tout à l’heure ! Telnet est justement **obsolète** pour cette raison : les données entre le client et le serveur ne sont **pas chiffrés**, elles sont **en clair**.
### Configuration SSH

_Il est bien chiffré !_
Le protocole SSH nécessite la création de clés RSA ([Chiffrement RSA — Wikipédia](https://fr.wikipedia.org/wiki/Chiffrement_RSA)). RSA est un algorithme de **chiffrement asymétrique**, petit rappel juste après !

Pour créer un pair de clés RSA, on doit d’abord choisir un nom de domaine pour notre réseau (on reviendra sur les noms de domaine locaux plus tard). Lancez les commandes suivantes :
![[Pasted image 20251112175017.png]]
Après avoir quitté le mode Global Configuration avec la commande exit, vous devriez voir le message suivant :
![[Pasted image 20251112175044.png]]
La version 1.99 du protocole SSH n’es pas récente, activons plutôt la version 2 :

**Conf t > ip ssh version 2**

SSH est activé, mais comme pour Telnet, on doit autoriser ce protocole sur nos lignes VTY.

**Conf t > line vty 0 4 > transport input ssh > login local**

L’instruction login local indique à l’équipement que nous utiliserons un utilisateur stocké dans sa base de données interne.

Contrairement à Telnet, avec SSH il nous faut un nom d’utilisateur et un mot de passe. Lancez la commande suivante :

**Conf t > username admin password rocknroll**

Admin est le nom d’utilisateur, rocknroll son mot de passe.

Contrairement au mot de passe Telnet, celui-ci ne sera pas stocké en clair dans la configuration mais sera hashé.

SSH est maintenant configuré !

Pour nous connecter, on va utiliser l'application SSH Client sur le PC. Sélectionnez le protocole SSH, renseignez l'**adresse IP du switch** (192.168.1.250) et le **nom d'utilisateur** créé à l'étape précédente (admin), puis cliquez sur Connect.

Après avoir saisi votre mot de passe, vous êtes connecté en SSH au switch !
### SSH vs. Hackers
On l'a fait pour Telnet, vérifions donc si avec SSH le trafic est bel et bien chiffré !

Remplacez le Sniffer entre les deux machines, et cochez uniquement le filtre SSH dans la GUI.

Connectez-vous en SSH et observez le contenu des paquets.

À part ENCRYPTED DATA, le hacker ne va pas voir grand-chose ! Tout le trafic est bien chiffré, nos mots de passe sont en sécurité !
## Defense in depth
On en a parlé un peu en Saison 1 lors de l'intro sur la sécurité informatique : pour sécuriser au mieux un système, on doit appliquer le principe de défense en profondeur (defense in depth, en anglais).

Ce principe nous conseille de voir la sécurité en plusieurs couches consécutives : si une couche de sécurité est percée, la couche suivante doit prendre le relai.

Pourquoi parle-t-on de ça ?

Dans notre cas : Ce n'est pas parce que nous utilisons SSH et que nos mots de passe ne sont pas interceptables par un hacker, qu'il ne faut pas protéger notre réseau de l'ARP Poisoning et des autres attaques de type **MITM** (Man-In-The-Middle).

Préparation du [[Challenge - Connexion interface Box]]
## Chiffrement (voir [[Cryptographie]])
### Chiffrement vs. Hackage
![[Pasted image 20251112175343.png]]
### Chiffrement symétrique
![[Pasted image 20251112175421.png]]
### Chiffrement asymétrique
![[Pasted image 20251112175444.png]]
Le protocole SSH utilise RSA, est un algorithme de chiffrement asymétrique !
### Chiffrement hybride
![[Pasted image 20251112175511.png]]
