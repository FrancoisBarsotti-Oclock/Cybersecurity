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

Pour ce test je vais chercher l'adresse IP de ma VM (ici la Win10), soit depuis le terminal en tapant ``ipconfig``, soit dans les paramètres réseaux.

![01-Ip]

#### Le ping échoue.

![02-échec]

VirtualBox fonctionnant en mode NAT (Network Address Translation), il donne juste accès à Internet aux machines virtuelles via un réseau privé isolé, mais elles ne sont pas sur mon réseau local physique.

Il y a deux solutions pour permettre aux VM et à l'hôte de communiquer :

- Réseau privé Hôte (Host-only Adapter) 🔗

- Accès par pont (Bridge) 🌉

## Réseau privé Hôte (Host-only Adapter) 🔗

Cette solution va permettre de créer un réseau privé et isolé dans lequel il n'y aura que le PC hôte et les VMs. Pour garder la connexion à Internet on va garder l'interface réseau NAT, et on va ajouter une interface Host-Only puis démarrer la VM.

![03-Settings]

On récupère sa nouvelle adresse IP ``192.168.56.101``

![04-NouvelleIP]

Après un nouveau ping ne fonctionnant pas, j'ai cherché d'ou ça venait et il s'avère que le Firewall Windows peu bloquer la demande de ping. J'ai donc été activer les règles de traffic entrant (et sortant pour le faire dans l'autre sens) concernant le ping (Demande d'écho). Traffic entrant/sortant ICMPv4.

![05-Parefeu]

Une fois ces changement effectués, j'ai pu ping la VM depuis l'hôte et l'hôte depuis la VM.

![06-PingVersVM]

![07-PingVersHôte]

## 🌉 Accès par pont (Bridge) 

Cette solution va permettre aux VMs d'être sur le même réseau physique que l'hôte, avec une adresse IP du même réseau que lui.

On change cette fois l'interface réseau en Accès par pont, en utilisant la carte réseau de l'hôte.

![08-Settings]

On récupère la nouvelle IP ``192.168.1.17``, on voit déjà qu'on est sur le même masque (/24) sur l'hôte.

![09-NouvelleIP]

On fait un Ping pour vérifier, et c'est OK cette fois-ci.

![10-LastPing]

Pour Ping l'hôte depuis la VM, je dois cette fois activer les règles de traffic entrant ICMPv4 sur l'hôte.

Sur la VM Win11, je suis passé directement en Accès par pont (Bridge). Aucun problème particulier.

---

### 📚 Ressources:

