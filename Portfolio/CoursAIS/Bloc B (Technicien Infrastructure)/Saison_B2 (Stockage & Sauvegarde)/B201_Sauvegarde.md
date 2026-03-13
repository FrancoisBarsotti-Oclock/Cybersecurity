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







