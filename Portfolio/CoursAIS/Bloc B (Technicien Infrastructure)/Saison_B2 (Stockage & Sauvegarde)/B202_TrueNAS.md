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

![02-PrérequisPools](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_02-Pr%C3%A9requisPools.png)

Quand on commence à faire un **Storage** :

![03-StorageStep1](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_03-StorageStep1.png)

`Pool = VDEV`

![04-StorageVDEV](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_04-StorageVDEV.png)

**Différent Layouts** : Une fois sélectionné on ne peut pas le changer. Par contre, on pourra toujours effacer le pool (sur Export/Disconnect).

Disons que chaque disque est de 50 Go, alors selon layout la répartition sera comme il suit :

* **Stripe** : 50 + 50 + 50 -> 150 Go de données
* **Miroir** : 50 + 50 -> 50 Go de données et 50 Go de secours (pas utilisable)
* **RAID 5** : 50 + 50 + 50 -> 100 Go de données et 50 Go de parité (en générale on prend le nombre de disques -1 pour calculer la capacité de stockage)

Cependant, une fois le Pool créé, les ressources seront montrées en termes de GiB :

![05-RessourcesGiB](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_05-RessourcesGiB.png)

On peut maintenant créer un **Dataset** :

Bon à savoir qu’avec la création du PoolNAS on a déjà créé un Dataset, mais ses permissions ne peuvent pas être éditées car le PoolNAS est en root (juste pour gérer une hiérarchie). C’est pour cela qu’il faut créer un Dataset (pour gérer les permissions).

![06-CréationDataset](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_06-Cr%C3%A9ationDataset.png)

Puis, on a **Shares** pour les partages :

--- 
### Section culture Proxmox

**Template = Clone** : Je peux faire d’une VM un template qui pourra être clonée après.

---

Les groups Windows (SMB) sont les built-in: Quand on crée un compte SMB, on pourra le trouver dans les builtin_users group, en tant que membre (y rajouté par défaut) ; c’est-à-dire, plus besoin de gérer ses permissions.

Cependant, on peut éditer les permissions (Datasets/) pour virer le groupe « everyone » et rajouter d’autres.

![07-Built-in](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_07-Built-in.png)

Dans un autre côté, on aura une VM Win10 prête à être utilisé pour établir connexion avec le NAS

![08-ConnexionWin10](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_08-ConnexionWin10.png)

Et on y rentre avec l’utilisateur SMB qu’on a créé (user : fb*** ; mdp : ********)

![09-AuthWin10](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_09-AuthWin10.png)

Pour la récupération, les **Datasets** pourront être sécurisés avec des Snapshots (cela fait partie de la Data Protection). Le snapshot va être l’instantanée réel de notre disque.

⚠️ _**Attention**_ : un instantanée n’est pas une solution de sauvegarde, c’est juste une solution pour restaurer l’état du disque.

![10-Dataset&Snapshots](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_10-Dataset%26Snapshots.png)

Et on définit son nom, selon la convention de l’entreprise

![11-DatasetNaming](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_11-DatasetNaming.png)

Et voilà qu’il apparaît finalement listé, le snapshot :

![12-SnapshotListé](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_12-SnapshotList%C3%A9.png)

Pour sécuriser l’activité de l’entreprise, on peut paramétrer des snapshots réguliers (une rotation), dans les « `View Snapshot Tasks` → `Add Periodic Snapshot Task` »

![13-PeriodicSnapshots](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_13-PeriodicSnapshots.png)

⚠️ Faire **attention** à la rotation que l’on met concernant la durée de vie (**Lifetime**) : Si on sélectionne de faire à l’heure pendant 2 semaines, cela fera 24 snapshots par jour x semaine, ce qui fera trop de snapshots.

Cela utilise **Cron** pour déclencher les snaps périodiques. Cron est un planificateur de tâches sur Linux qui fait des [cron tabs](https://crontab.guru/), si besoin de solution.

Si l’on a besoin de récupérer l’état du disque, il suffira de « Cloner vers un nouveau Dataset » ou bien de « **Rollbacker** » le dernier snapshot, présent dans « Datasets » :

![14-Clone&Rollback](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_14-Clone%26Rollback.png)

**Clone To New Dataset** sera utile pour récupérer uniquement des pertes par inattention (i.e., dossier effacer par erreur)

**Rollback va rétablir** l’intégralité du Dataset, très utile quand on voit qu’il y a une corruption de masse (genre une attaque par Ransomware) 

Une fois la restauration créée, elle apparaît sans droit de partage ce qui empêche que la restauration arrive à destination. 

![15-Restauration](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_15-Restauration.png)

💡 Il faudra créer son partage, à travers « `Create SMB Share` »

![16-SMBshare](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_16-SMBshare.png)

Avec le partage créé, on pourra voir l’icône SMB et l’information du partage, à droite :

![17-icôneSMB](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_17-ic%C3%B4neSMB.png)

💡 Il faudra aussi gérer ses permissions pour rajouter notre utilisateur :

![18-GestionPermissions](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_18-GestionPermissions.png)

Il suffit d’éditer les permissions, et on laisse « `Other` » sans permission **par mesure de sécurité** (toujours important de côcher « Apply User » et « Apply Group », ainsi que la « Récursivité ») :

![19-UnixPermissionsEditor](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_19-UnixPermissionsEditor.png)

On vérifie que l’édition est prise en compte :

![20-Vérificationédition](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_20-V%C3%A9rification%C3%A9dition.png)

Et on pourra la voir (et l'accéder) sur Windows :

![21-AccèsFinalWin10](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B2%20(Stockage%20%26%20Sauvegarde)/images%20B2/images%20B202/B202_21-Acc%C3%A8sFinalWin10.png)

On pourra prendre le fichier qui avait disparu par erreur, le coller sur le Dataset d’origine et finir par effacer la restauration manuelle (dans mon Dataset de TrueNAS), tout de suite.

### 🚧 En construction 🚧

Challenge du jour 👉 [Challenge_B202](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B2/Challenge_B202.md) 👈

---


