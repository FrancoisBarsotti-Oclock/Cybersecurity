# 💾 Session B204. Proxmox Backup Server (PBS).

### Notions du jour :

* **Proxmox Backup Server (PBS)**

    Solution dédiée à la sauvegarde des machines virtuelles et conteneurs Proxmox.

    Elle permet des sauvegardes fiables, vérifiables et restaurables.

* **Installation de PBS sur Linux existant**

    Démonstration d’une installation sur un système Linux déjà en place afin d’expliquer :

    * La gestion des sources APT
    * L’adaptation des dépôts pour Debian 13

* **Installation via ISO**

    Installation classique de PBS à l’aide de l’ISO officielle sur une machine virtuelle hébergée sur Proxmox.

* **Configuration réseau du PBS**

    Utilisation obligatoire de deux cartes réseau :

    * Une interface pour joindre les machines virtuelles du cluster Proxmox
    * Une interface dédiée à la communication directe avec Proxmox VE

    Cette configuration est imposée par l’architecture réseau et la PFSense en place.

* **Adressage IP**

    Configuration manuelle des adresses IP sur le PBS afin d’assurer la connectivité avec Proxmox et le réseau des VM.

* **Datastore**

    Espace de stockage utilisé par PBS pour enregistrer les sauvegardes.

    Création d’un datastore après la configuration du stockage disque.

* **Notions de RAID**

    Présentation des modes de protection des disques :

    * RAID : tolérance aux pannes
    * RAID-Z : variante ZFS avec protection des données
    * Miroir : duplication des données sur plusieurs disques

* **Jointure Proxmox VE / PBS**
    
    Ajout du PBS dans Proxmox VE avec validation par empreinte (fingerprint) pour sécuriser la communication.

* **Sauvegardes**

    Réalisation et explication :
    * Sauvegarde d’une machine virtuelle
    * Vérification de l’intégrité de la sauvegarde
    * Sauvegarde d’un conteneur
    * Vérification de la sauvegarde du conteneur

* **Restauration et tests**

    * Collecte de fichiers depuis une sauvegarde de conteneur (picking)
    * Restauration complète d’une sauvegarde
    * Simulation d’erreurs pour valider le bon fonctionnement de la restauration


### 🚧 En construction 🚧

Challenge du jour 👉 [Challenge_C204](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B2/Challenge_B204.md)

---
