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







