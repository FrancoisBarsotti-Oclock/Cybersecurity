# 🐧Session C404. Linux Containers (LXC) 

**Notions du jour :**

## LXC : Linux Containers 🐧📦

_L’autre façon de conteneuriser_

### Définition 

LXC (Linux Containers) est une technologie de conteneurisation au niveau du **système d'exploitation**.

La ou Docker conteneurise des **applications**, LXC conteneurise des **systèmes complets** 💻

### Analogie Maison 🏠

* 🐋 **Docker** = un studio meuble pour une app Juste ce qu'il faut, rien de plus
* 🐧**LXC** = un appartement complet avec tout le confort Un vrai système avec init,  services, utilisateurs ...
* 🖥️ **VM** = une maison individuelle avec son propre terrain Systeme complet + noyau dédié + matériel virtualisé

## VM vs LXC vs Docker 📊

_Trois niveaux d'isolation_

### Comparatif

|  | **VM** | **LXC** | **Docker** |
| :--: | :--: | :--: | :--: |
| **Noyau** | Dédié | Partagé | Partagé |
| **Init system** | Oui | Oui | NOn |
| **Poids** | Go | ~100 Mo | ~10 Mo |
| **Démarrage** | Minutes | Secondes | Secondes |
| **Isolation** | Forte | Moyenne | Moyenne |
| **Usage** | Multi-OS | Systèmes Linux | Applications |

### Quand utiliser quoi ? 🤔

* **VM** : besoin d'un OS différent (Windows sur Linux) ou isolation forte
* **LXC** : besoin d'un système Linux complet mais léger
* **Docker** : déployer des applications de façon portable

_Ce ne sont pas des concurrents, ce sont des outils complémentaires !_ 🤝

## Comment ça marche ⚙️

### Les briques Linux sous le capot

LXC repose sur des fonctionnalités du **noyau Linux** :

* **Namespaces** : isolation des processus, réseau, utilisateurs ... _Chaque conteneur a sa propre "vue" du système_
* **Cgroups** : limitation des ressources (CPU, RAM, I/O) _On contrôle combien chaque conteneur peut consommer_
* **chroot** : isolation du système de fichiers _Chaque conteneur a sa propre racine_ `/`

💡Docker utilise exactement les **mêmes briques** sous le capot !

### La différence fondamentale

* 🐧**LXC** : lance un **init system** (systemd) -> un vrai OS qui tourne
* 🐋 **Docker** : lance **un seul processus** (l'application)

_C'est pour ça qu'on peut faire systemctl dans un LXC, mais pas dans un Docker !_

## LXC en pratique 🔧

### Installation
```apache
# Debian/Ubuntu
sudo apt install lxc lxc-templates

# Vérifier L'installation
lxc-checkconfig
```

### Créer un conteneur

```apache
# Créer un conteneur Debian (sur Ubuntu par exemple)
sudo lxc-create -n mon-conteneur -t debian -- -r bookworm

# Ou un Ubuntu (sur Debian, par exemple)
sudo lxc-create -n web-server -t ubuntu -- -r jammy
```

💡 **Note** : Ubuntu ne communique pas sa clé à jour par défaut, mais l'installation va nous donner le site sur lequel on peut la récupérer pour installer les templates.

_Le template ( -() télécharge et configure automatiquement l’OS_

### Commandes de base

```apache
# Démarrer
sudo lxc-start -n mon-conteneur

# Se connecter (au console)
sudo lxc-attach -n mon-conteneur

# Arrêter
sudo lxc-stop -n mon-conteneur

# Lister Les conteneurs
sudo lxc-ls -- fancy

# Supprimer
sudo lxc-destroy -n mon-conteneur
```

### Une fois dedans… 📍

C’est un **vrai système Linux !**

```apache
# On peut faire tout ça dans un conteneur LXC :
apt update && apt install nginx
systemctl start nginx
useradd -m webadmin
ip addr show

```

_Essayez de faire systemctl dans un conteneur Docker..._ ça ne marchera pas 😏

## Configuration réseau 🌐

### Par défaut

LXC crée un bridge réseau (1xcbr0) :

* Les conteneurs recoivent une IP via DHCP
* Ils peuvent communiquer entre eux
* Accès Internet via NAT

### Configuration custom

Le fichier de config de chaque conteneur :

```yaml
# /var/Lib/Lxc/mon-conteneur/config

lxc.net.0.type = veth
lxc.net.0.link = 1xcbr0
lxc.net.0.flags = up
lxc.net.0.ipv4.address = 10.0.3.100/24
lxc.net.0.ipv4.gateway = 10.0.3.1
```

### Ce que LXC fait mieux

* ✅ Exécuter des **services système** (systemd, cron, sshd ... )
* ✅ Simuler un **serveur complet** sans le poids d'une VM
* ✅ Idéal pour des **labs de test** réseau/sécurité
* ✅ Meilleur pour l'**administration système** traditionnelle

### Ce que Docker fait mieux

* ✅ **Portabilite** : une image tourne partout (Linux, Mac, Windows)
* ✅ **Écosystème** : DockerHub, Compose, Swarm/K8s
* ✅ **CI/CD** : parfait pour les pipelines de déploiement
* ✅ **Microservices** : un conteneur = un processus

## En résumé 

| **Critère** | **LXC** | **Docker** |
| :--: | :--: | :--: |
| **Philosophie** | !système complet | Application isolée |
| **systemd** | ✅ Oui | ❌ Non |
| **Dockerfile** | ❌ Non | ✅ Oui |
| **Orchestration** | Limitée | Swarm, K8s |
| **Portabilité** | Linux only | Multi-plateforme |
| **Cas d'usage** | Labs, infra | Apps, Ci/CD |


### Le combo gagnant
**Proxmox** utilise LXC nativement !

* Interface web pour creer des conteneurs LXC
* Templates pré-configurés (Debian, Ubuntu, Alpine ... )
* Gestion réseau, stockage, snapshots intégrés

_Sur Proxmox, on peut mixer VMs et conteneurs LXC sur le même hôte_ 🤝

### Quand choisir LXC sur Proxmox ?

* 🍂 Vous voulez un "serveur leger" sans le poids d'une VM
* 🧪 Vous montez un lab de test reseau/secu
* 🐧 Vous n'avez besoin que de Linux
* ⚡ Vous voulez un démarrage instantané

### Question 🤔

_Quel outil choisiriez-vous pour :_

1. Déployer une app web Node.js en production ? → **Docker** 🐋

2. Monter un lab avec 5 serveurs Linux pour tester du réseau ? → **LXC** (sur Proxmox) 🐧

3. Faire tourner un Windows Server ?- → **VM** ! LXC et Docker ne supportent pas Windows 💻

### En résumé 📝

* 🐧 **LXC** = conteneurs **système** (un vrai Linux léger)
* 🐋 **Docker** = conteneurs **applicatifs** (un processus isolé)
* ⚙️ Memes briques sous le capot (namespaces, cgroups)
* 🤝 **Complémentaires**, pas concurrents

Le bon outil pour le bon usage !

## LXD : Le gestionnaire de conteneurs moderne 🚀📦

_LXC sous stéroïdes_

### Définition

LXD (prononcé "lex-dee") est un gestionnaire de conteneurs système et de machines virtuelles.

C'est une **surcouche** au-dessus de LXC qui ajoute une API REST, un CLI moderne et des fonctionnalités avancées 🧠

### Analogie 

* 🐧 **LXC** = le moteur (les briques de base)
* 🚀 **LXD** = la voiture complete (moteur + tableau de bord + GPS + confort)

On ne manipule plus les fichiers de config a la main - LXD s'occupe
de tout !

### LXD vz LXC – en bref

|  | **LXC** | **LXD** |
| :--: | :--: | :--: |
| **Interface** | Commandes 1xc-* | Commande 1xc |
| **Config** | Fichiers manuels | API REST |
| **Images** | Templates locaux | Dépôt d'images distant |
| **Réseau** | Config manuelle | Gestion intégrée |
| **Stockage** | Basique | Pools avancés (ZFS, Btrfs...) |
| **VMs** | ❌ Non | ✅ Oui |
| **Clustering** | ❌ Non | ✅ Oui |

⚠️ _**Attention** à la confusion : la commande lxc (sans tiret) = LXC, lxc -* (avec tiret) = LXC !_

### De Canonical à ... Canonical

* **2014** : Canonical (Ubuntu) crée LXD comme surcouche de LXC
* **2023** : Canonical retire LXD de la Linux Containers community
* **2023** : LXD devient un projet **100% Canonical** sous licence Apache 2.0
* **2023** : La communauté fork LXD- naissance d'**Incus**

_Incus est maintenu par les anciens developpeurs de LXD au sein de Linux Containers. Les commandes sont quasi identiques !_

### LXD ou Incus ? 🤔

* **LXD** : maintenu par Canonical, intégré à Ubuntu (snap)
* **Incus** : fork communautaire, disponible sur toutes les distros

Les deux outils sont tres similaires. Ce qu'on apprend pour l'un s'applique à l'autre 🤝

## Installation 🔧

### Sur Ubuntu (snap)

```nginx
# Installer LXD
sudo snap install lxd

# Ajouter L'utilisateur au groupe Lxd
sudo usermod -aG lxd $USER
newgrp lxd
```

### Sur Debian (Incus)

```bash
# Ajouter Le dépôt
sudo apt install extrepo
sudo extrepo enable incus

# Installer
sudo apt update && sudo apt install incus

# Ajouter L'utilisateur au groupe
sudo usermod -aG incus $USER
```

_Incus utilise la commande incus au lieu de lxc, mais la systaxe est identique_

### lnitialisation

```nginx
# Configuration interactive
lxd init
```
**Questions posées** :

* Clustering ? → no (pour un usage standalone)
* Storage backend ? → dir (simple) ou zfs (recommandé)
* Network bridge ? → yes (crée 1xdbrø)
* Accès réseau à distance ? -> no (sécurité)

Accepter les valeurs par défaut est un bon point de départ

### Lancer un conteneur

```nginx
# Créer et démarrer un conteneur Ubuntu
lxc launch ubuntu:24.04 mon-serveur

# Créer et démarrer un Debian
lxc launch images:debian/12 web-server

# Créer et démarrer un Alpine (ultra-Léger)
lxc launch images:alpine/3.19 mini-server
```

C’est tout ! Le téléchargement de l’image + création + démarrage en **une seule commande** 🎉

### Commandes de base

```rust
# Lister les conteneurs
lxc list

# Entrer dans un conteneur
1xc exec mon-serveur -- bash

# Arrêter / démarrer / redémarrer
lxc stop mon-serveur
lxc start mon-serveur
lxc restart mon-serveur

# Supprimer (doit être arrêté)
lxc delete mon-serveur

# Forcer La suppression
lxc delete mon-serveur -- force
```



