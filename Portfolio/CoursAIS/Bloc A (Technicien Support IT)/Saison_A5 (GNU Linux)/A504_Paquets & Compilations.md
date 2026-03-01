# 🐧 Session A504. Paquets, compilations.

### Notions du jour

## Gestionnaires de paquets

Installer, mettre à jour et supprimer des logiciels proprement sur une distribution.

Dans ces slides :

- le rôle d'un gestionnaire de paquets
- apt/dpkg (Debian/Ubuntu), pacman (Arch), rpm + dnf (RHEL/CentOS/Fedora)
- commandes clés et fichiers de conf utiles
- idées de démos à reproduire
### Rappel : à quoi sert un paquet ?
- archive contenant binaires, dépendances, scripts de post-install, métadonnées
- téléchargé depuis des dépôts signés (sources officielles ou internes)
- installé avec vérification d'intégrité et résolution des dépendances
- permet les mises à jour cohérentes du système (sécurité, correctifs)
### apt / dpkg (Debian & dérivés)
- dpkg : outil bas niveau pour manipuler des . deb locaux
- apt / apt-get : résout dépendances, parle aux dépôts listés dans /etc/apt/sources. list et /etc/apt/sources.list.d/ *. List
- cache téléchargements dans /var/cache/apt/archives
Le Dossier /etc/sudoers.d/ : Ce répertoire est conçu pour contenir des fichiers de configuration supplémentaires (appelés "drop-in files"). Le fichier principal /etc/sudoers est configuré pour inclure automatiquement (souvent via la directive #includedir /etc/sudoers.d) tout fichier qu'il trouve dans ce dossier.
Commandes clés :
![[Pasted image 20251215105813.png]]
`sudo apt update`                           # rafraîchir la liste des paquets
`udo apt upgrade`                          # mettre à jour les paquets installés
`sudo apt install htop`                     # installer (résolution auto des dépendances)
`sudo apt remove htop`                 # désinstaller
`sudo apt search docker`                # rechercher
`sudo apt show openssh-server`    # infos détaillées
`sudo dpkg -i fichier.deb`               # installer un .deb local
`sudo dpkg -l | head`                     # lister les paquets installés
update : check les paquets installés et vois si y'a des MAJ; il met à jours les dépôts en fonction du sources.list
upgrade : télécharge et installe les mises à jour; met à jour les logiciels.
### pacman (Arch & dérivés)

- binaire unique qui gère synchro, install, cache dans /var/cache/pacman/pkg
- configuration : /etc/pacman. conf et listes de miroirs dans /etc/pacman.d/mirrorlist
- pas d'AUR par défaut : nécessiterait un helper (ex. yay ) non officiel

Commandes clés :
![[Pasted image 20251215111104.png]]
`sudo pacman -Syu`               # synchronise la base + met à jour tout le système
`sudo pacman -S htop`         # installe un paquet
`sudo pacman -Ss nginx`     # recherche dans les dépôts
`sudo pacman -Qi htop`       # infos sur un paquet installé
`sudo pacman -Ql htop`      # fichiers installés par le paquet
`sudo pacman -Rns htop`   # supprime + dépendances orphelines + fichiers conf
### rpm + dnf (Red Hat & dérivés)
- rpm : outil bas niveau pour les . rpm (pas de résolution de dépendances)
- dnf (ou yum sur anciennes versions) : gère dépôts, transactions, dépendances
- configuration des dépôts : fichiers . repo dans /etc/yum. repos.d/
Une transaction est un mécanisme de sécurité qui assure que les données sont toujours fiables, même en cas de problème.
Commandes clés :
`sudo dnf check-update`      # liste les mises à jour
`sudo dnf upgrade`                   # met à jour
`sudo dnf install htop`            # installe depuis les dépôts
`sudo dnf remove htop`         # désinstalle
`sudo dnf search nginx`         # recherche
`sudo dnf info openssh`        # infos
`sudo rpm -ivh fichier.rpm`    # installer un .rpm local
`rpm -qa | head`                            # lister les paquets installés
`rpm -ql bash`                              # fichiers installés par un paquet

#### Comparatif rapide
![[Pasted image 20251215112143.png]]
Haut niveau gère l'installation complète des paquets, bas niveau installe des paquets individuels. . dpkg installe un paquet mais ne gère pas les dépendances, ce qui rend l’opération plus complexe. apt utilise en arrière‑plan mais gère automatiquement les dépendances, les mises à jour et les dépôts, rendant l’installation beaucoup plus simple.
**Conseil** : utilisez toujours l'outil haut niveau pour bénéficier de la résolution des dépendances et des transactions sûres.
### Démos à faire en TP
1. Lister les dépôts configurés (cat /etc/apt/sources. List pacman -Syy, dnf repolist)
2. Installer puis retirer un paquet simple (htop, curl, tree)
3. Examiner les fichiers installes par un paquet (dpkg -L, pacman -Ql, rpm -ql)
4. Observer le cache et nettoyer (sudo apt clean, sudo pacman-Sc, sudo dnf clean all)
5. Mettre à jour une machine de test et surveiller les journaux (/var/log/apt/history. log, journalctl -u pacman-init, /var/log/dnf.log)
# Périphériques et hardware

Comprendre et surveiller le matériel sous Linux.

Dans ces slides :
- surveiller les performances du système
- gérer les processus (l’instance d’un service en cours d’exécution)
- commandes lspci, lsusb, dmesg
- commandes lsmod, insmod, modprobe
- le dossier /dev
## Surveiller les performances
- top, `betop`, htop : charge CPU, RAM, processus en temps réel (**btop** est plus complet et il faut l'installer)
- vmstat 1, `iostat -x 1` (pkg sysstat, il faut installer) : vue CPU/mémoire/E/S S (**Ctrl + C** pour sortir)
- `free -h` : mémoire utilisée, cache, swap
- df -h, lsblk : occupation et topologie des disques
- sar -u 1 5 : historique/échantillonnage (nécessite sysstat)

Demos en VM : lancer stress-ng ou compiler un projet et observer CPU/RAM/E/S.
## Gestion des processus
- `ps aux`, puis `ps aux | grep nginx` : lister tous les processus actifs (ps : processus status » / a : montre les processus de **tous les utilisateurs** / u : affiche les processus sous **forme lisible**, avec le **nom de l’utilisateur, l’utilisation CPU ou RAM**, etc/ x : inclut les processus **sans terminal**)
- `pgrep nginx` / `pkill nginx` : trouver/terminer par nom
- `kill -TERM < pid >/ kill -KILL < pid >` : signaux
- `nice -n 5 cmd / renice +5 < pid >` : priorité CPU
- `systemctl status < service >` : services gérés par systemd

## lspci, lsusb, dmesg

- `lspci -v` : périphériques PCI (carte réseau, GPU ... / v : verbos, dis-moi plus)
- `lsusb - v` : périphériques USB (clés, webcams ... / v : verbos, dis-moi plus)
- `dmesg | tail -n 50` : Affiche les derniers 50 messages du noyau (détections, erreurs)
- `dmesg -w` : suivre en direct un branchement USB/PCI

Demos : suivre dmesg -w pendant un redemarrage de service ou l'ajout d'une interface réseau virtuelle (NAT/bridged), vérifier lspci/l susb listent les contrôleurs virtio.

## Modules du noyau : Lsmod, insmod, modprobe

- `lsmod` : modules du noyau qui sont actuellement chargés + utilisation mémoire
- `modinfo < module >` : infos (version, licence, options) du module qu’on veut découvrir
- `sudo modprobe < module >` : charge avec dépendances
- `sudo modprobe -r < module >` : décharge
- `sudo insmod fichier.ko` : insère un module local (sans dépendances)

Démos : (dé)charger un module présent (virtio_net, loop), observer dmesg lors du modprobe.
### Le dossier /dev (devices)

- interface des périphériques sous forme de fichiers spéciaux
- `ls -l /dev/sd` ou bien `/dev/nvme` : disques détectés
- `ls -l /dev/tty` : terminaux/ports série
- udev gère la création dynamique (règles dans le fichier `/etc/udev/rules.d/`) : il écoute le noyau et il le décrit
- accès contrôlé par permissions/groupes (disk, tty, video ... )

Démos : créer un disque loop pour simuler un périphérique :
![[Pasted image 20251215140739.png]]
`sudo dd if=/dev/zero of=/tmp/disk.img bs=1M count=10`
`sudo losetup -fP /tmp/disk.img`    # crée /dev/loopX
`ls -l /dev/loop`
`sudo losetup -d /dev/loopX`
# Compiler des programmes
Installer depuis le code source quand un paquet n'existe pas (ou trop ancien).

Dans ces slides :
- prérequis pour compiler
- workflow classique configure / make / make install
- exemple rapide avec un petit programme C
- quelques bonnes pratiques pour éviter de casser le système
## Pourquoi compiler ?
- obtenir une version plus récente qu'en dépôt
- activer/désactiver des options spécifiques (modules, drivers)
- tester/patcher un logiciel avant packaging (les plus important, pour adapter l’outil à l'utilisation qu'on veut lui donner -logiciel libre, pour développer nous même un logiciel -même de virus)

**Attention** : préférer les paquets officiels quand ils existent (mise à jour et dépendances gérées).
## Prérequis

- outils de base : compilateur, linker, make, headers
- Debian/Ubuntu : **sudo apt install build-essential** (ou bien **apt list –installed | grep build-essential** pour aller directement au fichier souhaité)
- RHEL/Fedora: sudo dnf groupinstall "Development Tools"
- Arch : **sudo pacman -S base-devel**
- dépendances spécifiques : -dev / -devel des bibliothèques utilisées
- espace disque et CPU (compilation peut être longue)
### Workflow classique (tar.gz)
![[Pasted image 20251215143525.png]]
**tar xf logiciel-1.2.3.tar.gz**
**cd logiciel-1.2.3**
**./configure -- prefix=/usr/local**    # détecte l'environnement, options
**Make**   # compile
**sudo make install**   # installe (binaire + man + conf)
![[Pasted image 20251215143740.png]]
Commandes utiles :

- ./**configure -- help** : voir les options disponibles
- **make - j$(nproc)** : paralléliser
- make check ou make test : tests fournis
- **sudo make uninstall** (si prévu) pour retirer
### Exemple rapide : recompiler un outil existant (htop)

Prérequis : ncurses headers (libncursesw5-dev ou ncurses - devel).
![[Pasted image 20251215143842.png]]
**wget https://github.com/htop-dev/htop/archive/refs/tags/3.3.0.tar.gz**

**tar xf 3.3.0.tar.gz**
**cd htop-3.3.0**
**./autogen.** **Sh**          # déjà fait dans certaines archives, sinon nécessaire
**./configure -- prefix=/usr/local**
**make -j$(nproc)**
**sudo make install**
**htop -- version**

**Attention** : Remplacez htop par l'outil qui vous manque (ex : rsync, screen tmux) en consultant son README pour les dépendances.
