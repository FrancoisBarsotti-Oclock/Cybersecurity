# 🐧Session C404. Linux Containers (LXC) 

**Notions du jour :**
* LXC vs. Docker
* LXC sur Proxmox
* Bonus : LXD


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

**LXD** (prononcé "lex-dee") est un gestionnaire de conteneurs système et de machines virtuelles.

C'est une **surcouche** au-dessus de LXC qui ajoute une API REST, un CLI moderne et des fonctionnalités avancées 🧠

### Analogie 🚗

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

## Historique rapide 📜

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

_Accepter les valeurs par défaut est un bon point de départ_ 👍

## Premiers pas 🐣

### Lancer un conteneur

```yaml
# Créer et démarrer un conteneur Ubuntu
lxc launch ubuntu:24.04 mon-serveur

# Créer et démarrer un Debian
lxc launch images:debian/12 web-server

# Créer et démarrer un Alpine (ultra-Léger)
lxc launch images:alpine/3.19 mini-server
```

C’est tout ! Le téléchargement de l’image + création + démarrage en **une seule commande** 🎉

### Commandes de base

```yaml
# Lister les conteneurs
lxc list

# Entrer dans un conteneur
lxc exec mon-serveur -- bash

# Arrêter / démarrer / redémarrer
lxc stop mon-serveur
lxc start mon-serveur
lxc restart mon-serveur

# Supprimer (doit être arrêté)
lxc delete mon-serveur

# Forcer La suppression
lxc delete mon-serveur -- force
```

### Informations sur un conteneur

```nginx
# Vue détaillée
lxc info mon-serveur

# Configuration complète
lxc config show mon-serveur
```

Le lxc info affiche : état, IP, CPU, RAM, disque, PID, snapshots...

## Gestion des images ️🏞️

### Les dépôts d'images

LXD accède à des **serveurs d'images distants** :
* ubuntu: → images Ubuntu officielles (Canonical)
* images: → images communautaires (Debian, CentOS, Alpine, Fedora...)

```nginx
# Lister les images Ubuntu disponibles
lxc image list ubuntu:

# Lister les images Debian
lxc image list images: debian
# Lister les images locales (déjà téléchargées)
lxc image list
```

### Gérer les images locales

```nginx
# Télécharger une image sans créer de conteneur
lxc image copy ubuntu:24.04 local: --alias ubuntu-noble

# Créer une image à partir d'un conteneur existant
lxc publish mon-serveur --alias mon-image-custom

# Supprimer une image locale
lxc image delete mon-image-custom
```

_Publier un conteneur configuré = créer votre propre "template" réutilisable_ 🔄

## Fichiers & transferts 📂

### Copier des fichiers

```nginx
# Hôte → conteneur
lxc file push fichier-local.txt mon-serveur/root/

# Conteneur → hôte
lxc file pull mon-serveur/etc/nginx/nginx.conf ./
# Éditer un fichier directement
lxc file edit mon-serveur/etc/hosts
```

### Copier un dossier entier

```nginx
# Pousser un dossier (récursif)
lxc file push -r mon-site/ mon-serveur/var/www/html/

# Récupérer un dossier
lxc file pull -r mon-serveur/var/log/ ./logs-serveur/
```

_Beaucoup plus simple que docker cp ! Pas besoin de reconstruire une image_ 😉

## Configuration des conteneurs ⚙️

### Limiter les ressources

```nginx
# Limiter la RAM à 512 Mo
lxc config set mon-serveur limits.memory 512MB

# Limiter à 2 CPUs
lxc config set mon-serveur limits.cpu 2
# Limiter le disque à 5 Go
lxc config device set mon-serveur root size=5GB
```

### Autres configurations utiles

```nginx
# Démarrage automatique au boot de l'hôte
lxc config set mon-serveur boot.autostart true

# Priorité de démarrage (plus petit = démarre en premier)
lxc config set mon-serveur boot.autostart.priority 10
# Empêcher l'élévation de privilèges
lxc config set mon-serveur security.privileged false
```

### Profils (profiles) 📋

Les profils permettent de **réutiliser des configurations** :

```nginx
# Créer un profil
lxc profile create web-server

# Configurer le profil
lxc profile set web-server limits.memory 1GB
lxc profile set web-server limits.cpu 2

# Appliquer le profil à un conteneur
lxc launch ubuntu:24.04 site-prod --profile default --profile web-server

# Voir les profils existants
lxc profile list
```

_Le profil default est toujours appliqué — les profils supplémentaires viennent s'ajouter_

## Réseau 🌐

### Le bridge par défaut

À l'initialisation, LXD crée un bridge lxdbr0 :
* DHCP intégré (attribution automatique des IPs)
* DNS intégré (résolution par nom de conteneur)
* NAT vers l'extérieur

```nginx
# Voir la config réseau
lxc network show lxdbr0

# Lister les réseaux
lxc network list
```

### IP statique

```nginx
# Assigner une IP fixe
lxc config device override mon-serveur eth0 \
ipv4.address=10.10.10.100
```

### Créer un réseau custom

```nginx
# Créer un nouveau bridge
lxc network create mon-reseau \
ipv4.address=192.168.100.1/24 \
ipv4.nat=true

# Attacher un conteneur à ce réseau
lxc network attach mon-reseau mon-serveur eth1
```

### Redirection de ports

```nginx
# Rediriger le port 80 de l'hôte vers le conteneur
lxc config device add mon-serveur http proxy \
listen=tcp:0.0.0.0:80 \
connect=tcp:127.0.0.1:80
```

_Équivalent du -p 80:80 de Docker, mais configurable à chaud !_🔥

## Stockage 💾

### Les storage pools
LXD gère le stockage via des **pools** :

```nginx
# Lister les pools
lxc storage list
# Voir les détails d'un pool
lxc storage show default
# Créer un nouveau pool ZFS
lxc storage create fast-pool zfs size=50GB
```

### Backends supportés

| **Backend** | **Avantages** | **Cas d'usage** |
| :--: | :--: | :--: |
| **dir** | Simple, aucune dépendance | Tests, débutants |
| **zfs** | Snapshots rapides, compression | Recommandé en prod |
| **btrfs** | Snapshots, sous-volumes | Alternative à ZFS |
| **lvm** | Familier pour les sysadmins | Infra existante |
| **ceph** | Stockage distribué | Clustering |

_ZFS est le choix recommandé pour la plupart des cas d'usage_🏆

## Snapshots 📸

### Créer et restaurer

```nginx
# Prendre un snapshot
lxc snapshot mon-serveur snap-avant-maj

# Lister les snapshots
lxc info mon-serveur # section "Snapshots"

# Restaurer un snapshot
lxc restore mon-serveur snap-avant-maj
# Supprimer un snapshot
lxc delete mon-serveur/snap-avant-maj
```

### Snapshots automatiques

```nginx
# Snapshot toutes les heures, garder les 24 derniers
lxc config set mon-serveur snapshots.schedule "0 * * * *"
lxc config set mon-serveur snapshots.schedule.stopped false
lxc config set mon-serveur snapshots.expiry 24h
```

_Format cron classique ! Parfait pour les environnements de test_ 🧪

### Créer un conteneur à partir d'un snapshot

```nginx
# Copier un snapshot vers un nouveau conteneur
lxc copy mon-serveur/snap-avant-maj nouveau-serveur
lxc start nouveau-serveur
```

Idéal pour dupliquer rapidement un environnement configuré 🔄

## Machines virtuelles 💻

_Oui, LXD fait aussi des VMs!_

### Conteneur vs VM dans LXD

```nginx
# Lancer un conteneur (par défaut)
lxc launch ubuntu:24.04 mon-ct

# Lancer une VM
lxc launch ubuntu:24.04 ma-vm --vm
```

Même commande, même gestion, même réseau — seule l'isolation change !

### Pourquoi des VMs dans LXD ?

* 🔒Isolation forte (noyau séparé)
* 🪟Support d'autres OS (pas seulement Linux)
* 🛡️ Séparation totale quand la sécurité l'exige

|  | **Conteneur LXD** | **VM LXD** |
| :--: | :--: | :--: |
| **Noyau** | Partagé | Dédié |
| **Performance** | Quasi-native | Légère overhead |
| **Démarrage** | ~1s | ~10s |
| **Isolation** | Namespaces | Hyperviseur (QEMU) |

_Le gros avantage : gérer conteneurs ET VMs avec les mêmes commandes!_🎯

## Clustering 🏢

### LXD en cluster

LXD peut fonctionner en **cluster multi-nœuds** :

* Haute disponibilité des conteneurs
* Migration live entre les nœuds
* Base de données distribuée (Dqlite)

```nginx
# Sur le premier nœud
lxd init # répondre "yes" au clustering

# Sur les autres nœuds
lxd init # rejoindre le cluster existant
```

### Migration live

```nginx
# Déplacer un conteneur vers un autre nœud
lxc move mon-serveur --target noeud-2

# Copier un conteneur vers un autre nœud
lxc copy mon-serveur noeud-2:mon-serveur-copie
```

_La migration se fait à chaud,sansinterruption de service !_🔥

## Sécurité 🔐

### Conteneurs non-privilégiés

Par défaut, les conteneurs LXD sont **non-privilégiés** :

* Le root dans le conteneur ≠ root sur l'hôte
* Mapping d'UID/GID (65536 → 0 dans le conteneur)
* Réduit considérablement la surface d'attaque

```nginx
# Vérifier le mode
lxc config get mon-serveur security.privileged
# false = non-privilegié (par défaut, bien !)
```

### Bonnes pratiques
* ✅Garder les conteneurs non-privilegiés
* ✅Limiter les ressources (CPU, RAM, disque)
* ✅Utiliser des profils pour standardiser la sécurité
* ✅Mettre à jour les images régulièrement
* ❌Ne jamais activer security.privileged=true sans raison
* ❌Ne pas exposer l'API LXD sur le réseau sans TLS

### AppArmor & Seccomp

LXD applique automatiquement des profils de sécurité :

* **AppArmor** : restreint les accès fichiers et capacités
* **Seccomp** : filtre les appels système dangereux

_Pas besoin de configurer quoi que ce soit — c'est actif par défaut !️_ 🛡️

## LXD vs Docker — récap' 🔍

### Philosophies différentes

|  | **LXD** | **Docker** |
| :--: | :--: | :--: |
| **Approche** | Système complet | Application isolée |
| **Cible** | Sysadmins, infra | Développeurs, DevOps |
| **Gestion** | Comme une VM légère | Comme un processus |
| **Persistance** | Système de fichiers persistant | Conteneurs éphémères + volumes |
| **VMs** | ✅Intégrées | ❌Non |
| **Clustering** | ✅Natif | Via Swarm/K8s |
| **Écosystème** | Plus restreint | Énorme (Hub, Compose...) |

### Quand choisir LXD ?

* 🧪Labs et environnements de test
* 🍂 Infrastructure légère (remplacer des VMs)
* 🎓Formation sysadmin (vrais systèmes Linux)
* 🔒Isolation de services système
* ️🖥️ Mix conteneurs + VMs sur un même hôte

## Commandes essentielles — récap' 📋

### Conteneurs

```nginx
lxc launch <image> <nom> # Créer et démarrer
lxc exec <nom> -- bash # Entrer dans le conteneur
lxc stop/start/restart <nom> # Gestion du cycle de vie
lxc delete <nom> # Supprimer
lxc list # Lister
lxc info <nom> # Détails
```

### Fichiers & config

```nginx
lxc file push/pull # Transférer des fichiers
lxc config set/get # Configuration
lxc profile create/set # Profils
lxc snapshot/restore # Snapshots
```

### Réseau & stockage

```nginx
lxc network list/show/create # Gestion réseau
lxc storage list/show/create # Gestion stockage
lxc image list/copy/delete # Gestion images
```

## En résumé 📝

* 🚀**LXD** = gestionnaire moderne de conteneurs système (et VMs)
* 🐧Surcouche de **LXC** avec API REST et CLI élégant
* 📸**Snapshots, profils, clustering** intégrés
* 🔐Sécurité par défaut (non-privilegié, AppArmor, Seccomp)
* 💻Gère aussi bien les **conteneurs** que les **VMs**
* 🍴Alternative communautaire : **Incus**

L'outil idéal pour les sysadmins qui veulent la légèreté des conteneurs avec le confort d'une VM !🎯

---

Pas de challenge prévu, pour préparer l'ECF du lendemain

---