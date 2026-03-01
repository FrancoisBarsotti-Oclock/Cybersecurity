# 🔺🐧 Session A507. Arch Linux.

Notions du jour

Lien physique (avec un autre chemin à la ressource) vs lien symbolique

Arch linux : https://wiki.archlinux.org/title/Installation_guide_(Fran%C3%A7ais)#Obtenir_l'image_d'installation 

La première chose à faire c’est changer le clavier :

Localectl list-keymaps et après charger toute la liste, faire Ctrl + C pour sortir 
et taper localkeys fr-latin1

Ensuite on peut faire archinstall pour continuer la configuration en interface basique, ça va le faire uniquement si l’on a une connexion à internet

pour les amis sur PROMOX , allez dans local / ISO IMAGES / cliquer sur Dowload from URL et mettre : https://elda.asgardius.company/archlinux/iso/2025.12.01/archlinux-2025.12.01-x86_64.iso 

Pour une installation de ArchLinux en UEFI sur PROXMOX il faut décocher la case Clefs de préinscription (Système)

Arch Linux devrait fonctionner sur n'importe quelle machine utilisant l'architecture x86_64 dotée d'un minimum de 512 MiB de RAM, bien que plus de mémoire soit nécessaire pour démarrer le système « live » en vue de l'installation. [1] Une installation basique devrait utiliser moins de 2 GiB d'espace disque

c vite fait de vérifier la signature, on tape Get-FileHash .\archlinux-2025.12.01-x86_64.iso | Format-List dans powershell pour avoir l hash et le compare au fichier : https://archlinux.org/iso/2025.12.01/sha256sums.txt 

https://archlinux.org/download/#checksums 

https://i12bretro.github.io/tutorials/0874.html 

Arch Linux Installation in Proxmox: A Complete Guide - DevOps Thiago | Thiago Dos Santos

Archinstall pour installer en configurant avec une espèce d’interface

ext4 c’est la partition par défaut pour Linux
btrfs la partition de plusieurs marceaux 

Voir plus sur la Swap, mais c’est comme une aide à la RAM (différence entre hibernation et mise en veille)

Post-installation
https://wiki.archlinux.org/title/General_recommendations_(Fran%C3%A7ais) 

L’arch qu’on voit dans la console, s’appelle fast-fetch (c’est l’ASCII art d’Arch Linux). Pour l’aficher sur la console il faut faire les commandes :

sudo pacman -S fastfetch (juste la première fois, pour installer)
fastfetch (à chaque fois qu’on veuille l’avoir affichée)

![Mon installation Arch Linux](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A5%20(GNU%20Linux)/images%20A5.md/images%20A507/A507_Mon%20Arch%20Linux.png)

il faut modifier le fichier .config/hypr/hyperland.conf pour le mettre en FR 
