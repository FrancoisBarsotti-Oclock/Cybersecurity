# 📌 Session A304. Adresse IP : Attribuer une adresse IP à un switch !

On passe en mode Privileged et on affiche la config' actuelle :
![[Pasted image 20251106110813.png]]
**Sh run** = show running-config

On passe en mode configuration et on demande à configurer l'interface **Vlan1** :
![[Pasted image 20251106110845.png]]
**Conf t** = configure terminal

Pourquoi Vlan1 ? On y reviendra, c'est le "réseau virtuel" configuré par défaut sur tous les ports du switch.

On définit l'adresse IP de notre switch (ici, **192.168.1.251**) et son masque de sous-réseau, et on demande à l'interface de rester active en permanence :
![[Pasted image 20251106110911.png]]
Mettre plutôt l’adresse **192.168.1.251**

On peut maintenant quitter le mode configuration avec la commande **exit**.

Étant en mode Privileged, on en profite pour **véri****fi****er** la config' actuelle avec :
![[Pasted image 20251106110941.png]]
_On devrait pouvoir pinger le switch ! (on peut rajouter un autre ordi pour le test de ping aussi).
_Attention : il faut pinguer depuis un ordinateur (et pas depuis un switch)._

## Sauvegarder la config’
![[Pasted image 20251106111008.png]]
## Tester, c'est douter

Après avoir effectué une modification sur la running-config, la première chose à faire et de la tester !

Si le test n'est pas concluant, il suffit de débrancher/rebrancher l'équipement : la configuration startup-config sera restaurée et vos modifications infructueuses effacées.

La config' est correcte ? Pour sauvegarder la configuration actuelle, en mode Privileged, on

lance la commande :
![[Pasted image 20251106111201.png]]
Translating : quand on se trompe, c’est chiant.

Une faute de frappe ?
![[Pasted image 20251106111245.png]]
## Stopper la recherche

Pour interrompre la recherche du nom de domaine, utilisez le raccourci clavier Ctrl+Shift+6 ou Ctrl+6.
![[Pasted image 20251106111311.png]]
## Désactiver la recherche

On peut également désactiver complètement la recherche du nom de domaine. Pour cela, en mode Global configuration, lancez la commande :
![[Pasted image 20251106111333.png]]
![[Pasted image 20251106111344.png]]
## Sécuriser le switch

Le mode Privileged, permettant de modifier la configuration du

switch, doit être sécurisé.

Lancez les commandes suivantes, en mode Global Configuration en remplaçant rocknroll par le mot de passe de votre choix !
![[Pasted image 20251106111418.png]]
## Hostname
Pour donner un nom (hostname) à un équipement Cisco, en mode Global configuration, utilisez la commande :
![[Pasted image 20251106111440.png]]
# Types de réseaux
**LAN, MAN, WAN…

On peut classer les réseaux informatiques par leur étendue géographique :
- **Bus informatique** : réseau de communication dans la carte mère de l’ordinateur
- **PAN** : Personal Area Network (Bluetooth, USB)
- **LAN**: Local Area Network (votre réseau chez vous)
- **WLAN** : Wireless Local Area Network (votre réseau chez vous, mais WiFi !)
- **MAN** : Metropolitan Area Network (un réseau à l’échelle d’une ville)
- **RAN** : Rural Area Network (à l’échelle d’une région / zone rurale)
- **WAN**: Wide Area Network (à l’échelle d’un pays, continent, ou de la planète ! Exemple : Internet).

A **retenir** :
Un **LAN** :
- Permet de connecter des appareils dans une zone limitée
- Est administré par une seule organisation ou un seul individu
- Fournit une bande passante haut débit aux appareils internes
Le **WAN** :
- Permet de connecter les réseaux locaux LAN sur des vastes zones géographiques
- Généralement géré par un ou plusieurs FAI/fournisseurs de services
- Fournit généralement une bande passante plus lente, avec moins de débit qu’à l’intérieur d’un LAN

LAN et WAN permettent de définir des étendues géographiques

Un LAN un peu spécifique est parfois rencontré :  **SOHO** (Cet acronyme signifie Small Office / Home Office, et désigne donc les réaux locaux de petite taille).
## Intranet, Extranet
Intranet, Extranet ou Internet permettent de définir des services accessibles par différentes personnes :
- **Intranet** : service accessible uniquement par les salariés de l’entreprise (exemple : lERP de l’entreprise.
- **Extranet** : service accessible par les clients, fournisseurs, etc. de l’entreprise (exemple : un portail sur lequel les clients peuvent suivre leurs commandes).
- **Internet** : service « public », accessible par tout le monde (exemple : un moteur de recherche, un site web public, etc.).
### Fiabilité d’un réseau
Très important en entreprise !

Les critères :
- **Tolérance aux pannes/ haute disponibilité** : si un câble ou un routeur tombe en panne, le réseau doit continuer à fonctionner. On cherche à éviter les **SPOF** -Single Point Of Failure, donc on double tous les équipements critiques.
- **Evolutivité/ scalabilité/ montée en charge** : le réseau doit répondre à nos besoins actuels, mais doit également pouvoir s’adapter à des changements sans devoir tout refaire / tout changer.
- **Sécurité** : on veut garantir la confidentialité des données, l’intégrité des données, la disponibilité des données sur le réseau.
- **Qualité de service** : la QoS consiste à prioriser certains flux par rapport à d’autres : on veut par exemple que la téléphonie/voix sur IP soit prioritaire par rapport aux mails ou autres flux data.
## Connexion à internet, les offres dispo !
- **Câble** : proposé par les fournisseurs de services de télévision par câble, haut débit.
- **DSL** : passe par une ligne téléphonique, haut débit
   * **ADSL** : Asymmetric (débit montant et descendant différent), très adapté aux utilisateurs finaux/particuliers il y a quelques années (usage descendant principalement)
   * **VDSL** : Very high-speed rate Digital Subscriber Line: débit plus élevé que l’ADSL, nécessite d’être très proche (moins de 1000 mètres) du DSLAM.
   * **SDSL** : Symmetric (débit montant et descendant identique), plus adapté aux entreprises.
- **WiMax** : C’était pour les campagnes profondes [WiMAX — Wikipédia](https://fr.wikipedia.org/wiki/WiMAX)
- **Cellulaire** : passe par un réseau de téléphonie mobile (plus ou moins haut débit, 3G, 4G, 5G, etc.).
   * on peut même se fabriquer son propre routeur avec ce genre de carte M2 PCIe
- **Satellite** : latence qui peut être beaucoup plus élevée (sauf dans les cas de satellites à orbite très basse comme Starlink), intéressant pour les zones rurales non desservies par les autres solutions.
- **Ligne commutée** : peu coûteux, faible bande passante/débit, utilise un modem -obsolète.
- **FTTH** : Fiber To The Home, offres fibres “mutualisées” pour particuliers, le débit n’est pas garanti ! La rapidité du temps d’intervention en cas de panne n’est pas garantie (le pire c’est SFR).
- **FTTO/FTTB** : Fiber To The Office/ Building, offres fibres non mutualisées, débit symétrique et garanti, temps de rétablissement plus rapide en cas de panne en général (sauf pour SFR, catastrophique).
Les réseaux d’aujourd’hui sont dits **convergents**, on peut y faire passer toutes sortes de flux (téléphonie, vidéo, données, voix, etc.).
## Routeurs
Comment on fait communiquer des machines dans des réseaux différents ?
![[Pasted image 20251106134347.png]]
Un routeur est un équipement réseau informatique assurant le **routage des paquets**. Son rôle est de faire **transiter des paquets d’une interface réseau vers une autre**.

Dans les réseaux Ethernet les routeurs opèrent au nivau de la **couche 3 du modèle OSI**.

[Routeur — Wikipédia](https://fr.wikipedia.org/wiki/Routeur)
![[Pasted image 20251106134418.png]]
Le routeur permet de faire communiquer deux (ou plus !) réseaux différents.
![[Pasted image 20251106134445.png]]
Le cas de **Box** : appareil hybride qui fait switch, router…
### Ça ressemble à quoi ?
![[Pasted image 20251106134528.png]]
Le type de **routeur** que l’on a à la **maison**
**Line** à se connecter au WAN
Le **Switch** (en jaune, appelé Ethernet, c’est notre LAN)
![[Pasted image 20251106134600.png]]
Le type de **routeur** d’**entreprise** (version ancienne Cisco)
![[Pasted image 20251106134626.png]]
Le type de **routeur** d’**entreprise** (Next Generation on Fire Wall, NGFW). Ils peuvent être administrés sur une interface Web.
![[Pasted image 20251106134858.png]]
Avec une carte **SIM 4G**, il peut donner du WiFi (comme notre téléphone, qui se transforme en routeur quand on partage nos dates en mode WiFi).
Les **repéteurs WiFi** ne sont pas bons : on les laisse pour les particuliers et On les évite en entreprise.
## Pratique !
Relions deux réseaux LAN indépendant avec un routeur Cisco 1941 !

Les routeurs doivent être configurés

Commandes de Configuration de la première phase :

**conf t  
interface gigabitEthernet 0/1  
ip address 172.16.0.1 255.255.255.0  
no shutdown  
exit**

Commandes de Configuration de la deuxième phase (à vérifier):

**conf t  
interface gigabitEthernet 0/0  
ip address 192.168.1.1 255.255.255.0  
no shutdown  
exit**

Après nous pouvons pinguer les machines avec le routeur. Par contre, sans faire la passerelle (gateway), nous ne pourrons pas encore pinguer les machines du LAN 1 avec les machines du LAN 2.

Pour ça, il faut utiliser l’adresse du routeur comme porte de sortie (d’une LAN à une Autre). Elle sera la passerelle.
![[Pasted image 20251106144101.png]]
Et là nous pourrons lancer un Ping depuis le LAN 1 vers le LAN 2, mais il n’y aura pas de réponse si la passerelle n’est pas configurée aussi dans la machine du LAN2.

Nous pouvons la configurer sur **DHCP** pour toutes les machines, pour éviter de le faire sur chacune des machines séparemment et manuellement.
## Solution
![[Pasted image 20251106144213.png]]
### Routage – Table de routage

Pour router les paquets vers le bon réseau, un routeur va consulter sa **table de routage**. Il stocke :

- Les adresses de ses propres interfaces
- Les adresses de sous-réseaux auxquels il est directement connecté (Son but n’est pas d’établir la communication entre machines, mais entre réseaux).
- Les routes statiques, définies par un administrateur
- Les routes dynamiques, apprises par des protocoles de routage dynamique (RIP, BGP, OSPF,…) > on en parle en saison C.
- Une route par défaut
[Table de routage — Wikipédia](https://fr.wikipedia.org/wiki/Table_de_routage)

Commande : show ip route
## Routage statique
Un peu plus compliqué.
Une route statique permet d’indiquer à un routeur par quel autre routeur il doit passer pour joindre un réseau spécifique.

Pour ajouter une route statique sur un routeur Cisco, on utilise la commande **ip route**.
**Exemple** : Pour ajouter une route vers le réseau 10.20.0.0 /16, en passant par le routeur ayant l’IP **172.16.0.2**, la commande à lancer sera :
![[Pasted image 20251106160222.png]]
Quand nous avons plusieurs routeurs, il faudra faire étape par étape
## Route par défaut
Si le router ne sait pas comment router un paquet, son dernier recours sera sa route par défaut (si elle est définie).

Pour ajouter un route par défaut, par exemple par l’adresse IP **10.53.27.9** :
![[Pasted image 20251106160322.png]]
## Pratique, bis
Ce coup-ci, on met **deux** routeurs.

(si on conserve le 1941, il faudra ajouter un module) : Avec le 1941 il n’y a que deux interfaces, donc il faut rajouter un module sur les deux routeurs, pour en avoir plus d’interfaces. On les connecte par **câble optique (couleur orange)**.

Les commandes utilisées :
**enable  
conf t  
hostname Routeur2  
enable secret rocknroll  
  
**interface gigabitEthernet 0/1  
ip address 10.0.0.1 255.255.255.0  
no shutdown  
end**

Pour sauvegarder, il faut être dans *"enable"*:
**show running-config  
copy run sta** (pour sauvegarder)

La communication entre deux routeurs (sans machines dedans) sera considéré comme un autre réseau.

On commence par connecter les deux routeurs

Commandes pour configurer le deuxième routeur :
**conf t  
interface gigabitEthernet 0/0/0  
ip address 10.10.10.2 255.255.255.0  
no shutdown  
end  
copy run sta** (sauvegarder)

Commandes pour configurer le premier routeur
**conf t  
interface gigabitEthernet 0/0/0  
ip address 10.10.10.1 255.255.255.0  
no shutdown  
end**

Il faudra dire au routeur 1 et au routeur 2 la porte de sortie de chacun.

Route statique > vers un seul LAN (**ip route**)
**Sur routeur 1 :  
conf t  
ip route 10.0.0.0 255.255.255.0 10.10.10.2** (pour arriver à 10.0.0.0 il faut que tu passes par 10.10.10.2)**end  
copy run sta** (sauvegarder)

Route par défaut >  fonctionne mieux vers deux ou plus LAN, ce qui me fait gagner du temps
**Sur routeur 2 :  
conf t  
ip route 0.0.0.0 0.0.0.0 10.10.10.1** (pour que tu arrives à n’importe quel LAN, il faut que tu passes par 10.10.10.1)  
**end  
copy run sta** (sauvegarder)

![[Pasted image 20251106160439.png]]
## DHCP

On peut demander à notre serveur…
Le mieux c’est de faire un exemple, sur notre routeur 2 pour le LAN en 10.0.0.0 /16 :

**Conf t > ip dhcp pool LAN3 > network 10.0.0.0 255.255.0.0 > default-router 10.0.0.1 > dns-server 8.8.8.8 > exit > ip dhcp excluded-address 10.0.0.1 10.0.0.10 > end**

Activons le DHCP sur le PC, on devrait récupérer une addressee et pouvoir pinger les autre réseaux grâce à notre passerelle ! (Après je peux rajouter une nouvelle machine et demander de recevoir une ip automatiquement par DHCP. Cela évitera que chaque nouvelle machine doive être configurée.)

Il faut éviter avoir différents DHCP car seulement le plus rapide va répondre. Alors, il faudra le laisser uniquement sur notre serveur (et pas sur les routeurs)

Pour retirer le DHCP : **no ip dhcp pool LANLIL**
## Routage statique : analogie de la poste
Pour bien comprendre le routage statique, et la différence entre ce qui se passe à la couche 2 et à la couche 3, on peut faire une analogie !

Alice, qui habite à Lille, veut écrire à Bob, qui habita à Limoges.

Les étapes ci-dessous permettent de comprendre la différence entre ce qui se passe au niveau 2 et au niveau 3 sur un réseau TCP/IP.
**Pour configurer la passerelle d'un switch (Route par default) :  
  **enable > conf t >ip default-gateway 192.168.59.1 > end > copy run sta**

**Commande traceroute :** Pour montrer la trajectoire d’un point A à un Point B (à travers les routeurs, il ne montre jamais les Switches). 
  
- **sur MacOS/Linux : traceroute  
- **sur Windows : tracert**
