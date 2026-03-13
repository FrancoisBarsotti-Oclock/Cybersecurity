# 💾 Session B201. Sauvegarde & stockage

### Notions du jour :

* **DAS, NAS, SAN** : trois architectures de stockage. DAS = stockage local, NAS = stockage en réseau via fichiers, SAN = stockage réseau en mode bloc.

* **NFS, SMB, iSCSI** : protocoles d’accès aux données. NFS et SMB pour le partage de fichiers, iSCSI pour l’accès disque à distance.

* **RAID** : technique combinant plusieurs disques pour améliorer performance et/ou sécurité des données.

* **SDS (Software Defined Storage)** : stockage piloté par logiciel (ex. vSAN, Ceph) indépendant du matériel.

* **Types de sauvegarde** : complète, incrémentielle et différentielle.

* **Règle 3-2-1** : bonne pratique de sauvegarde avec plusieurs copies et supports.

* **Sauvegarde vs snapshots** : sauvegarde = copie indépendante, snapshot = image rapide d’un état.

* **TrueNAS** : solution NAS open-source basée sur ZFS.

### Fiche Récap

* DAS = local, NAS = fichiers réseau, SAN = blocs réseau.

* NFS/SMB pour fichiers, iSCSI pour disques distants.

* RAID améliore performance et/ou tolérance aux pannes.

* SDS dissocie le stockage du matériel physique.

* Sauvegarde ≠ snapshot : usages complémentaires.

* La règle 3-2-1 est une base essentielle en sauvegarde.

* TrueNAS est une solution NAS fiable basée sur ZFS.

## Sauvegarde & stockage 💾

_Primordial en sécurité info : il faut faire des sauvegardes_

_Et il faut bien les stocker quelque-part_

### Introduction
_Primordial en sécurité ?_ 🤔

La **sécurité absolue (parfaite, inviolable) n'existe pas**, aucun système n'est sécurisé à 100%.

➡️ Il faut **se préparer à l'échec des mesures de sécurité** que nous avons mis en place.

Prenons un cas de figure très courant :
une **attaque par ransomware** (_rançongiciel_ en FR) !

Le **serveur de fichiers** de l'entreprise est touché😱

Toutes les données des utilisateurs sont stockées dessus, **les informations vitales à l'activité de l'entreprise** ne sont plus accessibles ... quelles solutions s'offrent à nous ?

Pour les ransomwares, on a toujours les deux options suivantes : 

1. Payer la rançon💵 (très mauvaise idée, aucune garantie)
2. Ne pas payer la rançon

Le deuxième cas de figure est le plus raisonnable, mais il implique de devoir faire une croix sur les données "kidnappées" ... **Sauf si on a des sauvegardes** !

* Voir [exemple de Blackshades](https://en.wikipedia.org/wiki/Blackshades)

* [Red Flag Domains]( https://red.flag.domains/) est un site français qui liste les domaines malicieux.

## Guide de l'ANSSI
L'ANSSI a fait un guide pour donner quelques recommandations à ce sujet (sauvegarde):
📖 [Sauvegarde des systèmes d'information](https://messervices.cyber.gouv.fr/guides/fondamentaux-sauvegarde-systemes-dinformation) 

et aussi pour l’[authentification multifacteur et mdp](https://messervices.cyber.gouv.fr/guides/recommandations-relatives-lauthentification-multifacteur-et-aux-mots-de-passe)

### PRA / PCA

Dans tout SI, il faut prévoir des Plans de Reprise d'Activité (PRA) ou des Plans de Continuité d'Activité (PCA).

Ces plans indiquent comment remettre un service en état de fonctionnement après un incident. Les sauvegardes (et leur restauration !) sont primordiales dans un PRA ou PCA.

En anglais, on parle de Disaster Recovery Plan (DRP).

PRA n’est pas pareil que PRI (P Reprise Informatique, pour juste le système et non pas l’activité), ainsi que PCA n’est pas pareil que PCI.

Ils sont souvent faits ensemble, mais les comptes rendus seront différents.

### Testez !
Avoir des sauvegardes, c'est bien.

**Tester leur restauration, c'est mieux !** (des sauvegardes impossibles à restaurer après un incident les rendent caduques).

Il faut en tester certains types et à des certains moments. On teste les restaurations **sur un système isolé**.

Cela rentre dans les deux cadres, de PRA et PCA.

## Sauvegarde
_Facile la sauvegarde, un copier/coller sur une clé USB et hop terminé. Eh bien, non !_

La création d'un cluster et la gestion de la haute disponibilité fait partie d'un plan de continuité (PCI, sur ESXi ou Proxmox). [Video sur la mise en pratique](https://www.youtube.com/watch?v=NI353Ev9TNc) 

### PDMA & DMIA

La PDMA, **Perte de Données Maximale Admissible** (RPO, Recovery Point Objective us) est la durée en heures (ou minutes) que l'on est **prêt à perdre** en cas d'incident.

La DMIA, **Durée Maximale d'Interruption Admissible** (RTO, Recovery Time Objective us), est le **temps d'interruption toléré** en cas d'incident, avant que le service soit à nouveau opérationnel.

Ces métriques sont définies par **accord de niveau de service** (SLA, Service Level Agreement), entre la DSI et la Direction Générale.

![01-PDMA&DMIA]()

Reprenons notre exemple de tout à l'heure :
Si l'attaque par ransomware se produit à **10h** du matin, et que la **dernière sauvegarde** a été effectuée à **2h** du matin, on a perdu **8 heures de données**.

Si la PDMA de l'entreprise est supérieur à 8h, pas de problème ! (on a quand même perdu des données, mais on a respecté le SLA)

Le SLA est défini soit par notre DSI ou par le client direct.

Quelques exemples :

* PDMA de 24h : il faut sauvegarder quotidiennement
* PDMA de 12h : il faut sauvegarder 2x par jour
* PDMA de 6h : il faut sauvegarder 4x par jour
* etc.

Quand la PDMA est très basse (quelques minutes seulement, voir aucune perte admissible), la sauvegarde ne suffit plus : il faut mettre en place un système de **réplication des données** (les deux doivent être combinés). **Attention** : La réplication de données n’est pas une sauvegarde !

## Règle "3-2-1"

Pour sauvegarder dans les règles de l'art, il faut ...

* au moins **3 copies** d'un fichier (la production + 2 sauvegardes),
* stockées sur au moins **2 supports** de stockage différents,
* dont **1 support hors-site/hors-ligne**.

_On peut aller plus loin, mais la règle ci-dessus est censée être le minimum._

![02-Règle3-2-1]()

_**Exemple** : une sauvegarde en interne sur un serveur différent, et une sauvegarde sur bande à conserver hors-site._

![03-Exemple règle]()

_Pas envie d'apporter les sauvegardes dans un coffre-fort ? Remplacez les bandes par un stockage Cloud !_

### Rétention

Parmi les SLA définies de consort entre la DSI et la Direction de l'entreprise, il faut définir la politique de **rétention des sauvegardes**.

La durée de rétention, c'est le nombre de jours (ou de mois/années) pendant lequel on va devoir **conserver une sauvegarde**. (et après, on pourra supprimer la sauvegarde pour libérer de l'espace). Cela sera toujours lié à l’activité (hôtellerie, pharmacie, etc.).

Comme précisé dans les recommandations de l'ANSSI, cette durée peut varier selon le type de sauvegarde. Type de sauvegarde ?

## Types de sauvegarde

Imaginons qu'on veuille sauvegarder un dossier, par exemple la racine de notre serveur de fichiers.

Il existe plusieurs types de sauvegarde, le plus simple est la **sauvegarde complète** : on fait une copie conforme du dossier à sauvegarder sur un support différent.

(si on garde la sauvegarde sur le même serveur, ça perd tout son intérêt).

En cas d'incident, la restauration est facile : il suffit de reprendre la dernière sauvegarde complète !

Inconvénient de la sauvegarde complète ? Elle **prend du temps**, puisqu'il faut sauvegarder l'ensemble des fichiers !
Pour pallier ce problème, on peut utiliser l'un des deux types de sauvegardes suivant :

* sauvegarde incrémentielle
* sauvegarde différentielle

Voir le chapitre qu’IT Connect a dédié à [la sauvegarde incrémentielle et différentielle](https://www.it-connect.fr/comprendre-la-sauvegarde-incrementielle-et-differentielle/) 

### Sauvegarde incrémentielle

Quand on fait une sauvegarde incrémentielle, on sauvegarde **uniquement les fichiers ajoutes/modifiés depuis la dernière sauvegarde complète ou incrémentielle**.

Cette sauvegarde est beaucoup **plus rapide et très légère**, mais la **restauration est plus complexe** : il faut restaurer la dernière sauvegarde complète + toutes les sauvegardes incrémentielles effectuées depuis la dernière sauvegarde complète.

![04-Sauvegarde incrémentielle]()

### Sauvegarde différentielle

On peut aussi choisir de faire une sauvegarde **différentielle** : dans ce cas, on sauvegarde **uniquement les fichiers ajoutés/modifiés depuis la dernière sauvegarde complète** (même s'il y a eu d'autres sauvegardes différentielles entre temps).

Un peu **plus lentes et volumineuses que les sauvegardes incrémentielles**, les sauvegardes différentielles sont **plus simples à restaurer** : on restaure la dernière sauvegarde complète et la dernière sauvegarde différentielle.

![05-Sauvegarde différentielle]()

## Sauvegarde vs. Snapshot

_(vs. réplication)_

Les **snapshots** et **sauvegarde** sont souvent confondus, ce sont en réalité **deux techniques complémentaires**.

On peut aussi mettre en place de la **réplication**.

_Snapshot ?_

Un snapshot (instantané FR) est la **photographie d'un système de fichiers à un instant t**. Ils permettent de pouvoir revenir dans le passé, à la date à laquelle a été faite le snapshot. C’est juste une représentation d’un état, c’est pour ça il est rapide (il ne copie pas les fichiers).

Attention : les snapshots ne peuvent pas être considérées comme des sauvegardes, puisque les données ne sont pas copiées ! Seules les données modifiées ou supprimées depuis le dernier snapshot (ou le snapshot initial) sont conservées.

Les snapshots sont **rapides** (à créer et à restaurer) et relativement légers (individuellement). On peut donc se permettre de faire des snapshots fréquents (toutes les 30min ou 60min, par exemple), pour pouvoir revenir à des **versions antérieures** de nos fichiers.

Cette rapidité a un inconvénient : les snapshots sont stockes sur la même baie de stockage (en général). Et même si ce n'est pas le cas, les snapshots seuls ne permettent pas la restauration des données.

### Bonne pratique :

En général, on combine sauvegardes et snapshots.

Exemple :

* une sauvegarde complète toutes les semaines
* une sauvegarde différentielle ou incrémentielle tous les jours
* un snapshot toutes les heures

_Et la réplication ?_

Pour certaines entreprises, on ne tolère **pas d'interruption de service**. Dans ce cas, on peut mettre en place de la **réplication** !

Cette réplication peut être **synchrone** (en "miroir", toutes les entrées/sorties sont faites sur 2 serveurs/disques/supports simultanément), ou **asynchrone** (copie des modifications après la fin de l'écriture sur le premier support, ou toutes les 1/5/10 minutes).

En cas d'incident, il suffit de basculer sur le répliquât.

## Stockage

_C’est bien beau, mais on sauvegarde sur **quoi** ?_

Voir cette [Vidéo intéressante sur stockage](https://www.youtube.com/watch?v=8cNPpLswdLY) 

### Hot vs. Cold

Il existe de nombreux types de supports de stockage !

On utilise la température (hot/cold) pour "classer" ces supports.

Plus un support est "**chaud**", plus il est **proche de la production**, plus on peut **facilement/rapidement écrire et lire sur ce support**. À l'inverse, un support est "**froid**" quand il est **plus difficile ou plus lent de lire/écrire sur ce support**, qu'il n'est pas connecté à la production, peut-être même pas alimenté électriquement.

### Online vs. nearline vs. offline

On n’utilise pas toujours la température pour classer les types de supports !

•	stockage "online" : correspond à un stockage "chaud", directement accessible.
•	stockage "nearline" : "tiède", pas directement accessible mais facilement accessible (par exemple, un disque dur externe à connecter)
•	stockage "offline" : stockage "froid", plus difficilement accessible (stockage sur bandes sur site distant par exemple)

### Médias de stockage
Lire sur le [stockage SAN](https://www.hpe.com/fr/fr/what-is/san-storage.html) 

* bande magnétique (LTO)
* disque dur (mécanique)
* supports optiques (CD, DVD, Blu-ray + M-Disc)
* mémoire flash (disques dur SSD, carte SD/microSD, clés USB)
_Liste non exhaustive !_

Chaque media a des avantages et des inconvénients, il n'y a pas un média _parfait_.

Selon le média choisi, différentes mises en œuvre sont possibles :

* DAS (Directly Attached Storage)
* NAS (Network Attached Storage)
* SAN (Storage Area Network)
* Stockage Cloud

### DAS

Le _Directly Attached Storage_ (**DAS**) est le type de stockage que nous utilisons le plus souvent : le média (disque dur, clé USB, etc.) est **directement connecté/attaché à une machine**.

Très rapide, il a l'inconvénient de ne **pas** être **accessible aux autres machines** sur le réseau, comme c’est local.

§[06-DAS]()

### NAS

Un serveur **NAS** (_Network Attached Storage_) est un serveur spécifiquement conçu pour le stockage de données.

Il est accessible par toutes les machines sur le réseau via des protocole comme SMB/CIFS ou NFS, mais est moins rapide qu'un stockage DAS. C’est du RJ45.

![07-NAS]()

### SAN

Un **SAN** (_Storage Area Network_) est un **réseau de stockage**. Plus exactement, ce sont des **baies de stockage** directement accessibles en mode bloc (comme en DAS) par le système de fichiers des serveurs.

C'est comme si on avait un disque dur en DAS, mais physiquement connecté à plusieurs serveurs. C’est de la fibre.

![08-SAN]()

## RAID

Le **RAID** (_Redundant Array of Independent Disks_) est un ensemble de techniques de virtualisation du stockage permettant de **repartir des données sur plusieurs disques durs** afin d'améliorer les **performances**, la **sécurité** ou la **tolérance aux pannes**.

[Exemple d’un super RAID en vente](https://www.inmac-wstore.com/broadcom-megaraid-mr408i-o-controleur-de-stockage-sata-6gb-s-sas-12gb-s-pcie-4-0-nvme-pcie-4-0-x8/p7343722.htm) 

### Intérêt du RALD

Le RAID permet soit :

* **redondance** des données sur plusieurs disques pour avoir une certaine tolérance aux pannes matérielles (exemple : RAID 1)
* **répartition** des données sur plusieurs disques pour améliorer les performances (exemple : RAID O)
* compromis entre redondance et répartition (exemple : RAID 5)

### Niveaux RAID

Il existe différents "niveaux" d'architecture RAID, numérotés à partir de 0. Les plus connus :

* **RAID 1** : mise en "mirroir" de 2 (ou +) disques (tolère la panne d'un disque)
* **RAID O** : répartition des données sur 2 (ou +) disques (ne tolère aucune panne, mais + performant)
* **RAID 5** : 3 disques mini, utilisation d'un bit de parité pour reconstruire les données en cas d'incident (tolère la panne d'un disque)

_On peut également combiner certains niveaux entre eux (RAID 10 = RAID 1+0, par exemple)._

![09-NiveauxRAID]()

### Matériel vs. logiciel

On peut faire du RAID de deux façons différentes :

* **RAID matériel** : nécessite une carte contrôleur PCI spécifique relativement coûteuse, souvent avec une batterie.
* **RAID logiciel** : assure par le système d'exploitation (avec lequel nous allons travailler en cours)

## Solutions

_Sur le marché, pour le stockage & la sauvegarde !_

### Solutions stockage

Fabricants de NAS réputés :
* Synology
* QNAP

Fabricants de SAN :
* Dell
* HP Entreprise
* IBM

---

### Section culture

[Vidéo sur les écoutes](https://www.youtube.com/watch?v=0lz2KRRGQZI)

[Vidéo sur le mithes de la tech](https://www.youtube.com/watch?v=CT72czY9MBE)

---

## NAS DIY

Il est possible, bien que rarement rencontré en entreprise, de créer son propre NAS. Les OS les plus souvent rencontrés sont :

* TrueNAS (propose aussi des solutions pro)
* OpenMediaVault
* Unraid (payant)
* Rockstor

### Solutions de sauvegarde

* Veeam
* UrBackup
* Proxmox Backup Server

## Place à la Pratique
TrueNAS, on va utiliser Scale

### 🚧 En construction 🚧




