# 💾 Session B203. Veeam.

### Notions du jour :

* **VEEAM Backup & Replication**

    Utilitaire de sauvegarde permettant de protéger des fichiers, des serveurs et des machines virtuelles.

* **Sauvegarde de fichiers (File Backup)**

    Sauvegarde des données stockées sur un serveur SMB (partage réseau Windows) vers un repository de sauvegarde.

    _Exemple_ : sauvegarde d’un dossier partagé contenant des documents utilisateurs.

* **Repository de sauvegarde**

    Emplacement où sont stockées les sauvegardes.

    Types utilisés :
    * NFS : partage réseau souvent utilisé sous Linux
    * iSCSI : stockage réseau présenté comme un disque

* **Sauvegarde de VM**

    Protection complète d’une machine virtuelle (disques, configuration, état).

    _Exemple_ : sauvegarde d’une VM hébergée sur Proxmox vers un repository NFS.

* **Infrastructure VEEAM**

    Ensemble des composants configurés dans VEEAM :

    * Serveur VEEAM
    * Sources de données (serveur SMB, Proxmox)
    * Repository de sauvegarde

* **Jobs de sauvegarde**

    Tâches qui définissent :

    * Quoi sauvegarder
    * Où stocker la sauvegarde
    * Quand lancer la sauvegarde

    Le fonctionnement est présenté en deux étapes :
    4.	Configuration complète du job (pré-requis)
    5.	Lancement et exécution du job (manuel ou automatisé)

## Veeam

_Une très bonne solution entreprise_

---
### Section Culture Linux :
Pour effacer tout ce qu’il y a sur les Temps (bien utile pour libérer de l’espace quand on va « donwload » un ISO) :
```nginx
cd /var/tmp
rm -rf *

# à savoir que rf * fait tout effacer
```

### Culture Virtualisation :
Proxmox a des conteneurs que l’on trouve dans le local/ CT Templates. Les VM créées avec ces templates vont se différencier en forme de carré. 

![01-CTProxmox]()

Elles sont moins lourdes, et aussi moins sécurisées, alors il ne faut pas les mettre en privilégié.

### Culture Home-lab :
-Voir les carte mère « Machinist x99 »

---

Pendant l’installation de Veeam, on peut voir les ports qu’il utilise pour fonctionner :

![02-VisualisationdePorts]()

C’est lent pour démarrer la première fois, le temps d’attendre que toutes les applications de Veem finissent de se lancer :

![03-LancementVeeam]()




### 🚧 En construction 🚧

Challenge du jour 👉 [Challenge_C203](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B2/Challenge_B203.md)

---

