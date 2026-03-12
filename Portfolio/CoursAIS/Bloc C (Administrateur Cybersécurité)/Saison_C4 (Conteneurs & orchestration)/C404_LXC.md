# 🐧Session C403. Linux Containers (LXC) 

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


