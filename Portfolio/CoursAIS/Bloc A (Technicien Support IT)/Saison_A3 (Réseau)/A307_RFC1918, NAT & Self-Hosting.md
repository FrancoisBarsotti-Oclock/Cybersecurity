# 🔀✳️ Session A307. RFC1918, NAT & Self-Hosting

Notions
- Adresse IP privées vs. Publique & RFC1918
- NAT : principe
- Types de NAT
- Self-hosting

Programme de la journée
- Pas de correction ce matin !
- RFC1918 & NAT (3h)
- Redirections de port (30min)
- Exercice pratique : self-hosting (1h30)
# NAT (Network Address Translation)
Un mécanisme indispensable pour l’IPv4.

On l’a vu et pratiqué lors du dernier atelier, des sous réseaux différents (avec des adressages différents) peuvent communiquer entre eux grâce aux routeurs.

Ça fonctionne comme ça, Internet, donc ? Pas tout à fait !

Pour bien comprendre pourquoi il nous manque **un élément primordial**, on va essayer d’imaginer comment fonctionne Internet à partir de ce qu’on connaît.

Reprenons un exemple, plusieurs sous-réseaux connectés à Internet par le biais d’un **routeur** (comme votre box dans votre réseau domestique).

Pour info, on appelle parfois le routeur qui permet de connecter un ou plusieurs sous-réseaux à Internet un **routeur de bordure**.

**Internet, c’est l’interconnexion de plusieurs réseaux locaux**, donc on va symboliser Internet par un unique routeur (en réalité, il y a plein de routeurs interconnectés).
![[Pasted image 20251113093304.png]]
Pour l’instant, aucune route n’est en place. Les différents sous-réseaux ne peuvent donc pas communiquer.

**Rappel, à retenir** : une route est une entrée dans la table de routage d’un routeur. Elle permet de définir **vers quelle adresse IP le routeur** (sur laquelle la route est définie) **va devoir rediriger les paquets reçus à destination d’un certain sous-réseau**.

```
-> **Il manque des notes ici !!!**
```

Pour savoir si on peut créer une route par défaut sur un routeur, il faut se poser la question : « est-ce que je dois créer plusieurs routes qui passent par le même routeur ?

Quelle commande permet d’ajouter la route vers 10.10.0.0/16 via 34.56.78.91 ? La route statique -> **ip route 10.10.0.0 255.255.0.0 34.56.78.91**

Lançons ces commandes ip route sur les différents routeurs :
![[Pasted image 20251113094803.png]]
Si on ne s’est pas trompé, le ping doit passer entre tous les sous-réseaux.
## Un quatrième sous-réseau
Nos réseaux domestiques, équipés d’une box (fournie par notre opérateur), **ont souvent le même adressage** : 192.168.1.0/24 ou 192.168.0.0/24.

Rappel : une box FAI, c’est un routeur + un switch + un point d’accès WiFi (+encore d’autres fonctionnalités, potentiellement).

Vu que toutes les box ont souvent le même adressage, ajoutons un quatrième sous-réseau, LAN 4, avec le même adressage que le LAN 1.
![[Pasted image 20251113095136.png]]
Pour essayer de résoudre la pénurie d’IPv4, il y a deux solutions : l’IPv6 et le **NAT**.
Il faut donc qu’on rajoute deux routes :

• Routeur LAN 4
   ▪ route par défaut : vers 0.0.0.0 via 45.67.89.2

• Routeur Internet
   ▪ (existante) vers 192.168.1.0/24 (LAN 1) via 92.34.56.78
   ▪ (existante) vers 172.16.0.0/16 (LAN 2) via 12.34.56.78
   ▪ (existante) vers 10.10.0.0/16 (LAN 3) via 34.56.78.91
   ▪ vers 192.168.1.0/24 (LAN 4) via 45.67.89.1

C’est parti, ajoutons ces deux routes et essayons de pinger le serveur (10.10.0.5) depuis notre nouveau PC du LAN 4 !
Si on passe en mode simulation, on peut constater le problème : **le routeur Internet ne sait pas différencier le LAN 1 du LAN 4** (on reçoit systématiquement 2 paquets sur 4). Et pour cause : ces deux réseaux on le **même plan d’adressage** !

Si on consulte les routes du routeur Internet avec la commande **show ip route static**, on peut observer que pour les sous-réseaux 192.168.1.0/24 **deux destinations sont enregistrées** :
- 92.34.56.78
- 45.67.89.1
![[Pasted image 20251113112932.png]]
Le routeur va donc **répartir** les paquets à destination de ce sous-réseau **vers ces deux adresses IP**, un paquet vers LAN 1, le suivant vers LAN 4, puis à nouveau vers LAN 1, puis vers LAN 4, et ainsi de suite.

On constate donc qu’Internet ne peut pas simplement fonctionner avec ce qu’on a vu jusque-là. Et d’ailleurs, on l’avait même évoqué : toute machine communiquant sur un réseau informatique doit avoir une **adresse IP unique**.

Oui, mais du coup, avec la RFC 1918, il y plein de machines qui on la même adresse IP (par exemple, 192.168.1.5) ! Comment elles peuvent être connectées et communiquer sur le même réseau, Internet ?
## Adresse publique vs. Privée
Et si l’on disait que toute machine connectée à internet n’a en fait pas une, mais deux adresses IP ? (et on ne parle pas d’IPv6).

Rendez-vous sur [What Is My IP Address - See Your Public Address - IPv4 & IPv6](https://whatismyipaddress.com/)

On ne partage pas son adresse IP…

Ce que l’on voit sur le site c’est l’adresse IP publique de notre Box, ce qui ne correspondra pas à l’ip privée (192.168.1.1). Son adresse IP publique c’est sa patte connectée à Internet.

Le principe du NAT c’est d’emprunter l’adresse publique de notre Box pour…
![[Pasted image 20251113113440.png]]
Essayez de retourner sur [https://whatismyipaddress.com/](https://whatismyipaddress.com/) depuis votre smartphone, connecté au WiFi de votre box. **Votre smartphone a la même IP que votre PC !**

Comment est-ce possible ? **C’est justement le NAT qui permet de faire ça**.

Désactivez le WiFi et actualisez la page. Maintenant, votre téléphone est connecté en 4G/5G à un routeur différent (plus à votre box), en on a donc une adresse IP différente.

**Cette adresse IP est notre adresse publique, routable/joignable sur Internet.**

Toutes les adresses qui ne sont pas dans les plages listées par la RFC 1918 sont des adresses IP publiques. Elles sont utilisables sur Internet ! (mais pas forcément utilisées à un instant t)

**Rappel** : c’est **la pénurie d’adresses IPv4** sur Internet qui nous oblige à recourir à des adresses privées pour nos machines. Sans cette pénurie toutes les machines pourraient avoir une IP routable sur Internet.
## NAT : Le principe
Il y a une pénurie d’adresses IPv4, chaque machine connectée à Internet **ne peut pas posséder sa propre adresse IP publique**.

Nos machines utilisent donc sur un même réseau des adresses IP privées, qui ne sont pas uniques ni routable sur Internet.

Notre FAI ne nous fournit qu’**une seule adresse IP publique pour tout notre réseau**, et cette IP est assignée à la « patte » de notre box connectée à Internet.  

Dès que notre PC veut communiquer avec une machine sur Internet, **notre routeurbox va nous « prêter » temporairement son adresse IP publique**, unique et routable sur Internet.

C’est ça, le NAT ! _Enfin plus précisément, c’est l’un des types de NAT possibles_.

La communication peut ainsi s’effectuer, et au retour notre box renverra les données reçus à notre machine sur le réseau local en utilisant son adresse IP privée.
### En pratique
Reprenons nos 4 sous-réseaux de tout-à-l ’heure.

Avant d’activer le NAT, nous allons supprimer les deux routes que nous avions ajoutées à destination du sous-réseau 192.168.1.0/24 . Nous n’en aurons plus besoin une fois le NAT en place !

**Rappel** (on l’a dit juste avant) : les adresses privées (RFC1918) ne sont **pas routables** sur Internet.

**Pas routables = pas de routes** (vers ces réseaux) !

Sur le routeur Internet, pour supprimer d’un coup les deux routes, lancez les commandes :
![[Pasted image 20251113113527.png]]
Sur le routeur du LAN 4, lancez les commandes suivantes pour indiquer sur quelle interface est la **zone « intérieure » du NAT** (le LAN) et sur quelle interface est la **zone « extérieure »** (le WAN) :
![[Pasted image 20251113113551.png]]
Pour que le NAT fonctionne, il faut encore qu’on “**autorise” les machines de notre LAN à utiliser l’adresse IP de notre interface de sortie** (l'adresse publique du routeur, routable sur Internet).

Lancez les commandes suivantes pour créer une nouvelle **liste de contrôle d’accès** contenant tout notre sous-réseau, et autoriser cette liste à utiliser l’adresse IP de l’interface GigabitEthernet 0/3/0 :
![[Pasted image 20251113113616.png]]
L’utilisation de l’instruction overload à la fin de la dernière commande permet d’activer le **mode PAT**. On en reparle après !

Dès la dernière commande lancée, on devrait pouvoir pinger le serveur depuis notre PC derrière le LAN !

Observons en mode simulation ce qui se passe, d’abord sans NAT (sur LAN 2, par exemple) puis avec NAT.

- Quand le PC du LAN 2 ping le serveur 10.10.0.5, les paquets IP contiennent l’adresse IP privée du PC comme adresse source, **y compris une fois sortis du LAN 2**.
- En revanche, quand on ping depuis le LAN 1, **avec le NAT**, les paquets IP ne contiennent l’adresse IP privée du PC que dans le LAN 1 : **dès qu’ils sortent sur Internet, l’adresse IP source et remplacée par celle du routeur**.

La commande « **show ip nat translations** » nous montre la transformation de nos IP
## Types de NAT
Nous avons pris l’exemple le plus courant, **quasi-omniprésent sur les réseaux domestiques** et fréquemment utilisé en entreprise, mais il existe différents types de NAT !
- **NAT statique** : le routeur dispose d’**autant d’adresses IP publiques que de machines qui doivent accéder à Internet** et qui n’ont que des adresses IP privées. On perd l’intérêt principal du NAT, qui évite justement de devoir disposer d’une adresse IP publique par machine.
- **NAT dynamique avec pool d’adresses** : **plusieurs adresses IP publiques sont disponibles** sur le routeur, mais **pas suffisamment pour toutes les machines** ayant des adresses IP privées. Les adresses publiques peuvent être attribuées à certaines machines, ou partagées.
- **NAT dynamique avec surcharge** (overload), avec **PAT** (Port Address Translation)/ **masquerading** : une seule adresse IP publique est disponible pour toutes les machines du réseau ayant des adresses IP privées. C’est le type de NAT utilisé sur les box des FAI, sur nos réseaux domestiques, et en général c’est celui qu’on rencontre le plus souvent !
### PAT
PAT, pour Port Address Translation, est un mécanisme utilisé en mode **NAT dynamique avec surcharge** sur les routeurs.

Le principe ? Le routeur va potentiellement **modifier le port source TCP ou UDP des paquets envoyés** par la machine en même temps qu’elle remplace l’adresse IP source par la sienne.

Ainsi, à la réception de la réponse le routeur pourra **diriger le paquet vers la bonne machine en consultant le port de destination** !
## Redirection de ports
Avant d’expliquer ce que c’est, il faut qu’on corrige quelque-chose :

Les machines du LAN 3 et du LAN 2  ont des adresses IP privées : **elles ne sont pas censées être routable sur Internet**, il faut donc également qu’on active le NAT sur les routeurs correspondants.

Commençons par le LAN 3. Sur le routeur, il faut qu’on saisisse les commandes suivantes :
![[Pasted image 20251113120412.png]]
Pour vérifier que tout fonctionne bien, essayons de pinger le PC dans le LAN 2 (172.16.0.5) depuis le serveur.

Essayons aussi d’accéder au site web héberge sur notre serveur derrière le NAT.

_Ah, mais du coup, on ne peut plus utiliser l’adresse du serveur 10.10.0.5 ? Elle est privée, donc pas routable sur Internet !_
_Si les adresses IP privées de nos machines ne sont pas accessibles derrière le NAT, ça veut dire qu’on ne peut pas héberger de serveur derrière un NAT ?_

Si, **heureusement, c’est possible** ! (sinon on ne pourrait pas auto-héberger de site web chez nous, par exemple, ou de serveur Minecraft, ou ce que vous voulez)

Comment faire pour joindre notre serveur ?

Avec une **redirection de port** ! (port fowrarding)

On va dire à notre routeur du LAN 3 que **tous les paquets reçus à destination de son adresse IP publique sur le port 80** sont en fait **destinés à notre serveur**.
Pour créer cette redirection de port, on doit lancer la commande suivante sur le routeur du LAN 3 :
![[Pasted image 20251113133418.png]]
Tout le trafic reçu sur le port 80 de l’IP publique de notre routeur (34.56.78.91) sera redirigé vers le port 80 du serveur (10.10.0.5)
Une fois cette commande lancée, on peut ouvrir n’importe quel navigateur web sur nos machines connectées aux trois autres LAN et taper l’adresse http://34.56.78.91.

_Et si on veut mettre plusieurs serveurs derrière un NAT ? On a qu'un seul port 80 !_

On devra utiliser d'autres ports, pas le choix ! Par exemple :
![[Pasted image 20251113174944.png]]
**Attention**, dans le navigateur il faudra donc taper http://34.56.78.81:8080, par exemple.
Et si on regardait sur nos box ? C'est parti, direction ou http://192.168.1.1 http://192.168.1.254 !
_Aucune de ces adresses ne pointe vers ma box !_

Pour connaître l'adresse IP de notre box, on peut chercher l'IP de la passerelle dans les paramètres réseau.

Sur Windows, on peut aussi scanner toutes les machines sur un réseau avec un outil comme [https://angryip.org/](https://angryip.org/)  . Sur GNU/Linux (mais aussi sur Windows !), on peut utiliser le célèbre logiciel [https://nmap.org/](https://nmap.org/) .
## Self-hosting
Grâce au NAT de notre box, on peut donc **auto-héberger** des services chez nous, disposer de nos propres serveurs plutôt que de les louer !

On appelle ça du **self-hosting**.

Et si on essayait d'héberger un site web chez nous, là, tout de suite ? _Sur notre PC ?_ Oui !
### Pratique : self-hosting
Pour cet exo, on va utiliser le serveur web **Caddy**.

C’est un logiciel, multi-plateforme, qu’on peut très rapidement mettre en service pour servir des sites web basiques.

_On peut aussi faire des trucs assez complexes et poussés avec, mais là ce n’est pas l’objectif._

Sur Windows, rendez-vous ( [Releases · caddyserver/caddy](https://github.com/caddyserver/caddy/releases))et téléchargez le zip caddy_x.x.x_windows_amd64.zip (x.x.x correspondant à la dernière version dispo).

Faites clic-droit > extraire tout et choisissez d’extraire vers le dossier C:\caddy.

**Attention**, si vous n'avez pas extrait l'archive .zip au bon endroit, les instructions suivantes ne fonctionneront pas.
#### Premier lancement
Pour lancer le serveur web caddy, on doit passer par le terminal. Lancez les commandes suivantes :
![[Pasted image 20251113175719.png]]
#### Configuration de Caddy
Si on se rend sur http://localhost:2019/config/, on peut visualiser la configuration active de caddy. Vous ne voyez rien du tout ? C’est normal : on ne l’a pas encore configuré.

Dans le dossier C:\caddy faites clic droit > Nouveau > Document texte. Choisissez caddy.json comme nom de fichier.
Problème, Windows **cache par défaut certaines extensions de fichiers**.

Pour corriger ça, dans l’explorateur de fichier sur Windows 10, cliquez sur Affichage > Options > Modifier les options des dossiers et de recherche, puis sur la fenêtre qui vient de s’ouvrir allez dans l’onglet Affichage, puis décochez la case Masquer les extensions des fichiers dont le type est connu.

Cliquez sur Appliquer puis OK.

Maintenant que les extensions sont visibles, renommez le fichier : caddy.json au lieu de caddy.json.txt.

_Windows ne sait plus comment ouvrir le fichier !_

Pour l’instant, **on va l’ouvrir avec le bloc-notes**, vu qu'un fichier .json c’est un simple fichier texte.
[JavaScript Object Notation — Wikipédia](https://fr.wikipedia.org/wiki/JavaScript_Object_Notation)

Plus tard, on installera un éditeur de texte plus sympa pour modifier des fichiers de config' !

Copiez-collez la configuration suivante dans le fichier :
![[Pasted image 20251113180414.png]]
N'oubliez pas d'enregister les modifications.
On va pouvoir stopper le serveur avec un Ctrl+C dans le terminal, puis le relancer avec la commande suivante :
![[Pasted image 20251113180527.png]]
On peut aller sur http://localhost/ dans le navigateur.
_Une page blanche ?_

C’est normal, si on regarde la config, on a indiqué qu’on voulait “servir” (c’est un serveur web, ça “sert” du code HTML/CSS) le contenu du dossier C:\caddy\www mais on ne l’a pas créé !

On n’a pas non plus de site web à mettre dedans.
On peut demander à ChatGPT de générer un bout de HTML/CSS très simple :

**Génère moi une page web humoristique en HTML CSS en un seul fichier index.html que je puisse utiliser direct. Mets deux trois blagues de ton cru et autres calembours, je te fais confiance.**
Je précise "en un seul fichier" car on peut séparer le code HTML et CSS dans plusieurs fichiers différents (c’est d’ailleurs une bonne pratique, pour mieux s’y retrouver). Là, ce sera plus simple si tout est regroupé dans un seul fichier.

Évitez d’utiliser ChatGPT pour vous informer ou vous expliquer des concepts : il ne “comprend” pas ce qu’il écrit, un humain sera plus pertinent pour ça. Par contre, pour générer du contenu texte d’exemple, comme dans notre cas, c’est **parfait**.

Ce que ChatGPT m’a généré avec le prompt précédent, à ma première tentative :
![[Pasted image 20251113181238.png]]
Copiez-collez le code généré par ChatGPT dans le fichier index.html (après l'avoir créé), dans le dossier C:\caddy\www (après l’avoir créé, lui aussi).

Là-encore, utilisez le bloc-notes Windows (ne double-cliquez pas sur le fichier index.html, faites clic droit > ouvrir avec).
Actualisez la page, vous deviez voir un magnifique site généré par ChatGPT :
Vous ne comprenez pas la troisième “blague” ? ChatGPT a simplement essayé de traduire littéralement une blague de l’anglais vers le français
#### Redirection de port
NAT, redirection de port, vous vous souvenez ?

Rendez-vous sur la **page d’administration de votre box** (on a vu tout à l'heure comment faire) et **connectez-vous**.

Le nom d’utilisateur est en général admin, et le mot de passe est parfois les premiers caractères de votre clé WiFi, ou alors une valeur par défaut (admin, par exemple). Il faut chercher sur Internet en fonction du modèle de votre box et de votre opérateur !

Rendez-vous ensuite dans les réglages réseau avancés (ce menu peut porter différents noms selon le FAI), et cherchez “NAT”, “PAT” ou “redirection de port”.

Vérifiez l’adresse IP privé de votre PC avec la commande ipconfig.

**Ajoutez une redirection de port :**

• mettez l’IP de votre PC ou sélectionnez-le dans la liste déroulante
• en port “interne”, mettez 80 (le port qu’on a mis dans la config’ de Caddy)
• en port “externe”, mettez 80 aussi !
• si on vous propose tcp ou udp, sélectionnez tcp.
• donnez un nom à votre redirection

Regardez quelle est l’adresse IP publique de votre box avec un site web comme [https://www.myip.com/](https://www.myip.com/) ou [https://whatismyipaddress.com/](https://whatismyipaddress.com/) .

Sur votre smartphone **déconnecté du WiFi**, essayez d’aller sur cette adresse IP, par exemple : http://92.135.191.244 (remplacez cette adresse par la vôtre !).

Vous devriez voir le super site généré par ChatGPT !

Si vous voulez, vous pouvez nous partager votre adresse IP dans le chat, histoire qu'on voit ce que vous avez généré avec ChatGPT.

_Mais ce n’est pas hyper risqué, de partager son IP ?_

En réalité, pas tant que ça :

• Votre box possède un pare-feu : si vous ne l’avez pas désactivé, il y a de fortes chances pour qu’il bloque tout le trafic en provenance d’internet.
• Votre PC Windows possède un pare-feu (si vous ne l’avez pas désactivé), lui aussi bloque une partie du trafic.
• Votre PC Windows (10 ou 11) est parfaitement à jour, surtout les derniers correctifs de sécurité (n’est pas ?)
• On est entre nous, on va éviter de chercher à s’attaquer mutuellement (n’est pas ? Non, là en vrai je suis sérieux, n’oubliez pas que c’est passible de poursuites pénales !)
• Votre adresse IP est probablement dynamique : elle change à une certaine fréquence (bail dhcp).

Une fois que vous avez fini de tester, **très important : n’oubliez pas de désactiver/supprimer la redirection de port que nous avons mis en place**.

Cette redirection de port est une porte d’entrée potentielle pour des hackers, vu que c’est en quelque sorte un trou sur le pare-feu de la Box : tout le trafic en provenance d’internet reçu sur le port 80 de la box est redirigé vers le port 80 notre machine. On ne sait jamais !
