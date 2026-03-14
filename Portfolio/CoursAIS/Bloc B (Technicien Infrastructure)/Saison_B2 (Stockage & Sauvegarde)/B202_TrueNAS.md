# 💾 Session B202. TrueNAS

### Notions du jour :

* **ZFS** : Système de fichiers avancé utilisé par TrueNAS.

    _Avantages_ : intégrité des données, détection des erreurs, snapshots, RAID logiciel.

* **Storage** : Espace de stockage global qui regroupe les disques physiques.

    _Exemple_ : un pool contenant plusieurs disques.

* **vdev (Virtual Device)** : Groupe de disques formant une brique du stockage ZFS.

    _Exemple_ : vdev en RAIDZ pour la tolérance de panne.

* **Pool de stockage** : Ensemble de vdevs permettant de stocker les données.

    _Exemple_ : un pool nommé tank.

* **Dataset** : Sous-espace logique dans un pool ZFS.

    → Permet de gérer quotas, permissions et snapshots.

* **Quotas** : Limites de stockage appliquées à un dataset.
    _Exemple_ : bloquer un dossier à 100 Go maximum.

* **Permissions** : Droits d’accès aux fichiers et dossiers.

    _Exemple_ : lecture seule ou lecture/écriture.

* **SMB** : Partage de fichiers pour les clients Windows.

* **NFS** : Partage de fichiers utilisé principalement sous Linux.

* **iSCSI** : Accès au stockage en mode disque distant.

* **Snapshots ZFS** : Copies instantanées des données.Utiles pour restaurer des fichiers supprimés ou modifiés.

💡 Tout ce qui est serveur on lui met une adresse IP fixe :
```
System / Network/ Interfaces: Edit
```
👉 60 secondes pour accepter 

![01-AttributionIPstatique](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_01-AttributionIPstatique.png)

Pour créer des pools il faut avoir les trois suivants

![02-PrérequisPools]()

Quand on commence à faire un **Storage** :

![03-StorageStep1]()

`Pool = VDEV`

![04-StorageVDEV]()

**Différent Layouts** : Une fois sélectionné on ne peut pas le changer. Par contre, on pourra toujours effacer le pool (sur Export/Disconnect).

Disons que chaque disque est de 50 Go, alors selon layout la répartition sera comme il suit :

* **Stripe** : 50 + 50 + 50 -> 150 Go de données
* **Miroir** : 50 + 50 -> 50 Go de données et 50 Go de secours (pas utilisable)
* **RAID 5** : 50 + 50 + 50 -> 100 Go de données et 50 Go de parité (en générale on prend le nombre de disques -1 pour calculer la capacité de stockage)

Cependant, une fois le Pool créé, les ressources seront montrées en termes de GiB :

![05-RessourcesGiB]()

On peut maintenant créer un **Dataset** :

Bon à savoir qu’avec la création du PoolNAS on a déjà créé un Dataset, mais ses permissions ne peuvent pas être éditées car le PoolNAS est en root (juste pour gérer une hiérarchie). C’est pour cela qu’il faut créer un Dataset (pour gérer les permissions).

![06-CréationDataset]()

Puis, on a **Shares** pour les partages :

--- 
### Section culture Proxmox

**Template = Clone** : Je peux faire d’une VM un template qui pourra être clonée après.

---

Les groups Windows (SMB) sont les built-in: Quand on crée un compte SMB, on pourra le trouver dans les builtin_users group, en tant que membre (y rajouté par défaut) ; c’est-à-dire, plus besoin de gérer ses permissions.

Cependant, on peut éditer les permissions (Datasets/) pour virer le groupe « everyone » et rajouter d’autres.

![07-Built-in]()

Dans un autre côté, on aura une VM Win10 prête à être utilisé pour établir connexion avec le NAS

![08-ConnexionWin10]()

Et on y rentre avec l’utilisateur SMB qu’on a créé (user : fb*** ; mdp : ********)

![09-AuthWin10]()

Pour la récupération, les **Datasets** pourront être sécurisés avec des Snapshots (cela fait partie de la Data Protection). Le snapshot va être l’instantanée réel de notre disque.

⚠️ _**Attention**_ : un instantanée n’est pas une solution de sauvegarde, c’est juste une solution pour restaurer l’état du disque.

![10-Dataset&Snapshots]()

Et on définit son nom, selon la convention de l’entreprise

![11-DatasetNaming]()

Et voilà qu’il apparaît finalement listé, le snapshot :

![12-SnapshotListé]()

Pour sécuriser l’activité de l’entreprise, on peut paramétrer des snapshots réguliers (une rotation), dans les « `View Snapshot Tasks` → `Add Periodic Snapshot Task` »

![13-PeriodicSnapshots]()

⚠️ Faire **attention** à la rotation que l’on met concernant la durée de vie (**Lifetime**) : Si on sélectionne de faire à l’heure pendant 2 semaines, cela fera 24 snapshots par jour x semaine, ce qui fera trop de snapshots.

Cela utilise **Cron** pour déclencher les snaps périodiques. Cron est un planificateur de tâches sur Linux qui fait des [cron tabs](https://crontab.guru/), si besoin de solution.

Si l’on a besoin de récupérer l’état du disque, il suffira de « Cloner vers un nouveau Dataset » ou bien de « **Rollbacker** » le dernier snapshot, présent dans « Datasets » :

![14-Clone&Rollback]()

**Clone To New Dataset** sera utile pour récupérer uniquement des pertes par inattention (i.e., dossier effacer par erreur)

**Rollback va rétablir** l’intégralité du Dataset, très utile quand on voit qu’il y a une corruption de masse (genre une attaque par Ransomware) 

Une fois la restauration créée, elle apparaît sans droit de partage ce qui empêche que la restauration arrive à destination. 

![15-Restauration]()

💡 Il faudra créer son partage, à travers « `Create SMB Share` »

![16-SMBshare]()

Avec le partage créé, on pourra voir l’icône SMB et l’information du partage, à droite :

![17-icôneSMB]()

💡 Il faudra aussi gérer ses permissions pour rajouter notre utilisateur :

![18-GestionPermissions]()

Il suffit d’éditer les permissions, et on laisse « `Other` » sans permission **par mesure de sécurité** (toujours important de côcher « Apply User » et « Apply Group », ainsi que la « Récursivité ») :

![19-UnixPermissionsEditor]()

On vérifie que l’édition est prise en compte :

![20-Vérificationédition]()

Et on pourra la voir (et l'accéder) sur Windows :

![21-AccèsFinalWin10]()

On pourra prendre le fichier qui avait disparu par erreur, le coller sur le Dataset d’origine et finir par effacer la restauration manuelle (dans mon Dataset de TrueNAS), tout de suite.












