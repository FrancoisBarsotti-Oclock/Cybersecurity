# 🪟👀 Session A412. VDI & Hyper-V.

Ce cours introduit la base de la VDI (Virtual Desktop Infrastructure) : l'hyperviseur. Sur Windows Server, le rôle **Hyper-V** permet de créer et gérer des machines virtuelles qui serviront de modèles ("Masters") pour les bureaux virtuels.

- **Installation du rôle Hyper-V** :
    
    - S'installe via "Ajouter des rôles et fonctionnalités" > **Hyper-V**.
    - **Configuration** :
        - **Ethernet** : Il est conseillé de ne pas cocher la carte réseau durant l'installation (pour configurer le switch virtuel manuellement plus tard).
        - **Migration** : Laisser par défaut (CredSSP).
        - **Emplacement** : Changer le chemin par défaut pour stocker les VM dans un dossier dédié (ex: `C:\VM`) pour une meilleure organisation.
    - **Redémarrage** : Obligatoire pour charger l'hyperviseur au niveau noyau.
- **Gestion du Réseau : Commutateurs Virtuels (vSwitch)** :
    
    - Une fois installé, on configure le réseau via le **Gestionnaire Hyper-V** > **Gestionnaire de commutateur virtuel**.
    - Il existe 3 types de commutateurs :
        1. **Externe (Bridge)** : La VM est connectée directement au réseau physique (comme si elle était branchée au switch réel). Elle obtient une IP du DHCP du réseau LAN.
        2. **Interne (NAT)** : La VM communique uniquement avec l'hôte physique et les autres VM. (Souvent utilisé avec du NAT).
        3. **Privé (Local Only)** : La VM communique uniquement avec les autres VM sur le même vSwitch. Isolation totale de l'hôte physique.
    - _Note_ : La création d'un vSwitch externe crée une interface réseau virtuelle visible dans `ncpa.cpl`.
- **Création d'une VM "Master"** :
    
    - L'objectif est de créer une VM modèle (Master) propre, qui servira de base pour le déploiement de masse.
    - **Installation de l'OS** : On configure la VM pour démarrer via le réseau ("Installer à partir d'un serveur d'installation réseau") afin de récupérer l'image Windows via **WDS**.
- **Générations de VM : Gen 1 vs Gen 2** :
    
    - C'est un choix crucial à la création de la VM.
    - **Génération 1 (Gen 1)** :
        - Simule un matériel ancien.
        - **BIOS** classique (Legacy).
        - Disque IDE / Partition **MBR**.
        - Compatible avec les vieux OS (Windows 7, vieux Linux).
    - **Génération 2 (Gen 2)** :
        - Standard moderne (recommandé).
        - **UEFI**.
        - Disque SCSI / Partition **GPT**.
        - Supporte le **Secure Boot** (Démarrage sécurisé). _Attention : il faut parfois désactiver le Secure Boot pour certaines distributions Linux._
(incomplet)

### Déploiement VDI & Sysprep
Ce cours finalise la mise en place de la VDI (Virtual Desktop Infrastructure). L'objectif est de transformer une machine virtuelle Windows 10 "Master" en un modèle déployable massivement via les services RDS, offrant ainsi à chaque utilisateur son propre PC virtuel.

#### 1. Préparation du Master (Windows 10)

[](https://github.com/GitFreed/O-clock/blob/main/RESUME.md#1-pr%C3%A9paration-du-master-windows-10)

Avant de dupliquer une VM, il faut la "nettoyer" pour qu'elle soit neutre.

- **Nettoyage des comptes** :
    
    - On active le compte **Administrateur** intégré (via `lusrmgr.msc` ou Gestion de l'ordinateur).
    - On se connecte avec ce compte Admin.
    - On **supprime** le compte utilisateur initial (celui créé lors de l'installation) et son profil. _But : Avoir une image sans fichiers utilisateurs parasites._
- **Sysprep (System Preparation Tool)** :
    
    - C'est l'outil indispensable pour l'autonomie matérielle et la duplication. Il se trouve dans `C:\Windows\System32\Sysprep\sysprep.exe`.
    - **Modes d'utilisation** :
        - **Mode Audit** : Permet de démarrer en mode administrateur spécial pour installer des logiciels, des drivers ou faire des mises à jour _avant_ de sceller l'image.
        - **Mode OOBE (Out-Of-Box Experience)** : C'est le mode final. Au prochain démarrage, la machine lancera l'assistant de configuration (choix de la langue, clavier, création utilisateur...), comme un PC neuf sortant du carton.
    - **L'option "Généraliser" (Generalize)** : **Cruciale**. Elle supprime les informations spécifiques au matériel et surtout le **SID** (Security Identifier) unique de la machine. Si on ne généralise pas, on ne peut pas déployer l'image dans un domaine Active Directory (conflit d'identifiants).
    - **Action** : Pour le VDI, on choisit **OOBE** + Cocher **Généraliser** + Option d'extinction **Arrêter**.

#### 2. Déploiement VDI (Processus RDS)

[](https://github.com/GitFreed/O-clock/blob/main/RESUME.md#2-d%C3%A9ploiement-vdi-processus-rds)

Une fois le Master éteint (Syspreppé), le serveur RDS prend le relais pour créer la "Collection" de bureaux virtuels.

- **Rôle nécessaire** : Contrairement au RDS classique (Session), le VDI nécessite le rôle **Hôte de virtualisation des services Bureau à distance** (RD Virtualization Host) installé sur le serveur physique Hyper-V.
    
- **Processus d'installation (Théorique)** :
    
    1. Dans le Gestionnaire de serveur > Services Bureau à distance.
    2. Lancer l'assistant "Créer une collection de bureaux virtuels".
    3. **Type** : "Pooled" (Bureaux partagés, non persistants) ou "Personal" (Bureaux persistants, l'utilisateur garde ses modifs).
    4. **Source** : On sélectionne le fichier disque dur du Master (`C:\VM\Win10-MASTER.vhdx`).
    5. **Déploiement** : Le serveur va copier ce disque, et créer X machines virtuelles basées dessus.
- **Résultat** : Dans le portail web RDS (`/RDWeb`), l'utilisateur voit une icône "Windows 10 VDI". Quand il clique, le serveur allume une des VM disponibles et le connecte dessus.
    

#### Bonus : Optimisation Disque (Proxmox / QCOW2)

[](https://github.com/GitFreed/O-clock/blob/main/RESUME.md#bonus--optimisation-disque-proxmox--qcow2)

En bonus, voici la méthode pour réduire la taille d'un disque virtuel `qcow2` sur Proxmox (Linux). Les disques virtuels ont tendance à grossir même si on supprime des fichiers dedans. Cette manip permet de récupérer l'espace vide (sparsify).

- **Condition** : La VM doit impérativement être **éteinte**.
- **Procédure (Shell Proxmox)** :
    1. Passer en root : `sudo su -`
    2. Aller dans le dossier de stockage des images (adapter l'ID `9000` à votre VM) : `cd /var/lib/vz/images/9000`
    3. Vérifier la taille actuelle : `ls -lh`
    4. **Convertir et compresser** (Création d'une copie optimisée `newdisk`) : `qemu-img convert -f qcow2 -O qcow2 -o preallocation=off vm-9000-disk-1.qcow2 newdisk.qcow2` _(Cette commande réécrit le disque en ignorant les blocs vides)._
    5. Supprimer l'ancien disque (Attention, irréversible) : `rm vm-9000-disk-1.qcow2`
    6. Renommer le nouveau disque pour qu'il prenne la place de l'ancien : `mv newdisk.qcow2 vm-9000-disk-1.qcow2`
