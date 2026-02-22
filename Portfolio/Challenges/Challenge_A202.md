# 🏆 Challenge A202: Diagnostic Ping
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Actuellement, sur Windows 10 et Windows 11, il est impossible de pinguer vos machines virtuelles (VM) depuis votre ordinateur hôte (votre PC personnel) sous VirtualBox.
>
>* Votre tâche consiste à permettre à votre machine hôte d’effectuer un ping vers vos VM Windows. En d’autres termes, vous devez configurer votre environnement de manière à rendre vos VM accessibles en réseau depuis votre poste principal.
>
>💡 Rappel :
>Le ping est un test de connectivité réseau.
>
>C’est l’un des tests les plus simples, mais aussi l’un des plus importants à maîtriser en informatique. Il permet de vérifier qu’une machine est accessible sur le réseau et que la communication est possible entre deux hôtes.

Voir 👉 Cours A202 👈

---

## 🖥️🔄🖥️ Test de connectivité réseau 

### - Premier Test Ping

Pour ce test j'ai cherché l'adresse IP de ma VM, en tapant ``ipconfig``. En effet le ping ne passait pas.

VirtualBox fonctionnant en mode NAT (Network Address Translation), il donne juste accès à Internet aux machines virtuelles via un réseau privé isolé, mais elles ne sont pas sur mon réseau local physique.

Il y a deux solutions pour permettre aux VM et à l'hôte de communiquer :

- 🔗 Réseau privé Hôte (Host-only Adapter) 

- 🌉 Accès par pont (Bridge) 

J'ai décidé de le faire avec la deuxième option, j’ai modifié les paramètres de ma VM en activant l’option « Accès par pont » et en lui disant que la connexion se fait par Ethernet via dongle (tout cela sur la vérification du mode réseau) : 

## 🌉 Accès par pont (Bridge) 

Cette solution va permettre aux VMs d'être sur le même réseau physique que l'hôte, avec une adresse IP du même réseau que lui, en utilisant la carte réseau de l'hôte.

![01-Settings](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_01-Settings.png)

Ensuite, j’ai relancé le ping depuis le cmd. D’abord côté Machine Hôte :

1.	Je lance le ping de ma machine hôte, pour tester la boucle locale et, selon les statistiques de la commande ping, mon ping a réussi (0 perdus) :

![02-PingVersVM](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_02-PingVersVM.png)

2.	Je cherche à avoir l’adresse IPv4 de ma carte Ethernet pour pouvoir y lancer le ping (`ipconfig /all`):

![03-ipconfigAll](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_03-ipconfigAll.png)

Je lance le ping sur mon adresse IP (`ping [IPv4]`) pour m’assurer que mon Ethernet a bien une adresse IP Ethernet valide, et les statistiques le confirment (0 perdus):

![04-PingOK1](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_04-PingOK1.png)

Je lance le ping sur mon routeur, et je constante que ça passe aussi (0 perdus) :

![05-PingOK2](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_05-PingOK2.png)

### Côté machine virtuelle

Sous la commande ipconfig, j’ai aussi trouvé une adresse IP décrite en tant que VirtualBox Host-Only Ethernet Adapter, alors je l’ai pingué avec succès aussi (0 perdus) :

![06-PingOK3](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_06-PingOK3.png)

Pour finir, j’ai configuré le pare-feu des deux machines :

Panneau de configuration > Pare-feu > Paramètres avancés > Partage de fichiers et imprimantes entrant ICMpv4 et activer.

![07-Parefeux](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A202_07-Parefeux.png)

---

### 📚 Ressources:

