# ✳️🪟 Session A409. WDS.

Ce cours aborde le déploiement automatisé de systèmes d'exploitation via le réseau en utilisant le rôle **WDS** (Windows Deployment Services). Il permet d'installer Windows sur de multiples machines simultanément, sans avoir besoin de support physique (clé USB/DVD) pour chaque poste. Ainsi que l'injection de pilotes et l'automatisation des installations via des fichiers de réponses.

#### 1. **WDS (Windows Deployment Services)**

[](https://github.com/GitFreed/O-clock/blob/main/RESUME.md#1-wds-windows-deployment-services)
Service Windows Server permettant de déployer des systèmes d’exploitation sur le réseau sans support physique.  
_Exemple :_ installer Windows 10 sur plusieurs PC via un boot réseau.
- **Rôle** : Permet de stocker et de diffuser des images systèmes Windows (fichiers `.wim`) via le réseau. C'est l'évolution des anciens services RIS.
    
- **Images** : Le service repose sur deux types d'images principales :
    - **Image de démarrage (Boot Image)** : C'est l'environnement Windows PE ou WinPE (`boot.wim`, issu de l'ISO Windows 10) qui se charge en premier via le réseau pour lancer l'assistant d'installation. Elle est utilisée pour lancer l'environement d'installation. Elle permet au poste client d'accéder au serveur WDS.
    - **Image d'installation (Install Image)** : C'est l'image du système d'exploitation complet (`install.wim` ou `install.esd`, provenant de l'ISO Windows 10) qui sera copiée sur le disque dur du client. En outre, c'est l'image contenant le système Windows à installer.

- **Fonctionnement via PXE** :
    - Le déploiement repose sur la norme **PXE (Preboot Execution Environment)**. Cette technologie permet à une station de travail de démarrer directement depuis sa carte réseau (avant même le chargement de l'OS local) pour récupérer une image système située sur un serveur. En outre, Méthode permettant à un poste client de démarrer depuis le réseau pour récupérer l’image envoyée par WDS.
    - **Prérequis** : Pour que cela fonctionne, l'environnement doit disposer d'un serveur **DNS** (résolution de noms), d'un serveur **DHCP** (attribution d'IP) et d'un domaine **Active Directory** (authentification).

- **Configuration DHCP pour le PXE** (Option DHCP 60 - ou PXEClient): - Option activée lorsque le serveur WDS et le serveur DHCP se trouvent sur le même serveur.  Elle informe les clients PXE que le serveur DHCP gère également le service de déploiement.
    - Si le DHCP n'est pas sur le même serveur que WDS (exemple : un routeur pfSense), des options spécifiques doivent être configurées pour guider le client PXE :
        - **Option 66 (Boot Server Host Name)** : L'adresse IP ou le nom du serveur WDS.
        - **Option 67 (Bootfile Name)** : Le chemin du fichier de démarrage (ex: `boot\x64\wdsnbp.com` pour BIOS ou `boot\x64\wdsmgfw.efi` pour UEFI).
    - **Option 60 (PXEClient)** : Cette option est nécessaire uniquement si le DHCP et le WDS cohabitent sur le **même serveur**, pour éviter les conflits car ils écoutent tous deux sur le port UDP 67.

- **Limitations et transition vers MDT** :
    - WDS seul montre ses limites, notamment avec Windows 11 (nouveaux formats `.esd`, prérequis TPM/Secure Boot). Microsoft recommande d'utiliser **MDT (Microsoft Deployment Toolkit)**.
    - **MDT** est un outil gratuit qui se superpose à WDS pour offrir des scénarios beaucoup plus riches : il permet d'injecter automatiquement des drivers, d'installer des logiciels post-déploiement, d'exécuter des scripts de personnalisation et de migrer des données utilisateur, ce que WDS ne fait pas nativement. Pour les très grandes structures, on passera sur **SCCM** (System Center).

#### 2. WDS : Gestion des Pilotes (Drivers)

[](https://github.com/GitFreed/O-clock/blob/main/RESUME.md#2-wds--gestion-des-pilotes-drivers)

Pour que l'installation de Windows fonctionne sur différents matériels, WDS permet de gérer et déployer des pilotes (ex: carte réseau, contrôleur de disque).

- **Groupes de Pilotes** : Permet d'organiser les pilotes logiquement. On peut appliquer des **Filtres** pour contrôler leur déploiement :
    - **Filtres Client** : Définissent **QUI** reçoit les pilotes (ex: _Manufacturer_ = Dell, _Model_ = Optiplex 7080).
    - **Filtres Fichier** : Définissent **QUOI** (quels fichiers du groupe) est installé (ex: uniquement les pilotes _Net_ ou _Video_).

- **Injection dans l'Image de Démarrage (Boot Image)** :
    - **Indispensable** : Pour que l'installateur Windows (WinPE) puisse voir le disque dur ou accéder au réseau, les pilotes critiques (Stockage et Réseau) doivent être injectés directement dans l'image de boot (`boot.wim`).
    - **Cas de la Virtualisation (VirtIO)** : Sur des hyperviseurs comme Proxmox/KVM, il faut injecter les pilotes **VirtIO** :
        - **NetKVM** : Pour la carte réseau.
        - **viostor/vioScsi** : Pour le contrôleur de disque.
        - **Balloon** : Pour la gestion dynamique de la mémoire.
    - _Attention_ : L'injection est une modification lourde de l'image `.wim`. En cas de mise à jour de pilote, il faut souvent reconstruire l'image.

#### 2. WDS : Automatisation (Unattend)

[](https://github.com/GitFreed/O-clock/blob/main/RESUME.md#2-wds--automatisation-unattend)

L'objectif est de réaliser une installation "zéro touche" (Zero Touch Installation) où l'administrateur n'a pas besoin de cliquer sur "Suivant" devant chaque poste.

- **Fichier de réponses (Unattend.xml)** :
    - C'est un fichier XML qui contient les réponses aux questions de l'installateur (Langue, Partitionnement du disque, Fuseau horaire, Mot de passe admin local, etc.).
    - Dans les propriétés du serveur WDS (onglet _Client_), on lie ce fichier XML aux architectures (x64/x86) pour qu'il soit chargé automatiquement.

- **Pré-staging (Approbation et Nommage)** :
    - Permet de sécuriser le WDS en demandant une approbation avant l'installation.
    - L'administrateur peut nommer la machine et définir dans quelle OU (Active Directory) elle sera créée avant même que l'installation ne commence.
    - **Droits de jointure** : On définit quel compte est utilisé pour joindre le domaine (souvent un compte de service ou administrateur) avec les droits complets pour créer l'objet ordinateur.

- **Avantage et Inconvénient** :
    - **Avantages** : Installation rapide d'OS natifs, standardisation du parc.
    - **Inconvénient** : Gère uniquement l'installation, pas la maintenance applicative post-install.

----------------------------------------
Pour le TSSR TP (examen), dp (Dossier Professionnel : faire un exemple par compétence)

On peut faire un exemple pour regrouper plusieurs compétences. Il faut juste qu’elles appartiennent au même type d’activité selon le REAC

Pour le AIS : même procédé dp (Dossier Professionnel)

Pas de mise en pratique

C’est un oral qui dure 40 minutes environs, basé que sur le stage (rapport de stage ou Dossier de Projet) et les labs qu’on fasse de côté.

Le DP (Dossier de Projet). Alors, dp + DP + ppt 

------------------------------------------
