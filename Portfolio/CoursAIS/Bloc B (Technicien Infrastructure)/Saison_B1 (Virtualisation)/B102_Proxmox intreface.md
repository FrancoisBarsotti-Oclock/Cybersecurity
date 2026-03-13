# Session B102 : ProxMox interface

### Notions du jour

Proxmox et les concepts autour des hyperviseurs de type 1

* Les clusters
* La HA
* Les migrations
* La réplication

On a fait ça après un gros tour du propriétaire de notre bare-metal :

* Les noeuds
* Le datacenter
* Les options
* La gestion des utilisateurs et des permissions
* et plein d’autres

### local et local-lvm

### local et local-lvm

**local** c’est le disque dur pour le system, qui sert notamment à y verser les ISO pour l'installation des VM. **local-lvm** pour héberger les VM.
Cela est possible de l’avoir quand on a créé une partition de disques. C’est la meilleure pratique en entreprise, pour éviter de tout perdre ensemble dans un incident.

![01-local](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_01-local.png)

## Datacenter

### Summary

Le **Summary** va nous aider à faire du monitoring

![02-Summary](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_02-Summary.png)

### Notes

**Notes** nous permettra de communiquer ou enregistrer de commentaires importants

![03-Notes](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_03-Notes.png)

### Cluster

Le **Cluster** c’est un ensemble de machines distinctes qui fonctionnent comme si elles étaient un seul système (comme avoir plusieurs PC qui se partagent le travail). Un node c’est une des machines individuelles du cluster

![04-Cluster](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_04-Cluster.png)

On va créer le cluster sur un node et on va le rejoindre sur le deuxième (ou sur les autres) node.

### Options

Dans **Options**, on fait du paramétrage 

![05-Options](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_05-Options.png)

### Backup

Puis, pour créer la planification des sauvegardes, avec toutes les options :

![06-Backup](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_06-Backup.png)

Sur l’onglet « **Retention** », on peut Configurer comment on va garder les sauvegardes. C’est bon de voir toute sa documentation dans « **Help** »

![07-Retention](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_07-Retention.png)

Par exemple, si je veux garder juste les 5 dernières sauvegardes, je le paramètre :

![08-Planification](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_08-planification.png)

### Replication

La **Réplication** qui permet de faire une copie synchronisée d’une VM : Permet d'avoir une autre VM / réseau identique en cas de problème afin de pouvoir maintenir l'état de fonctionnement du système ou du réseau virtuel. c'est un clone synchronisé du node qui peut basculer au premier plan en cas de besoin, car elle répartie les charges.

![09-Replication](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_09-Replication.png)

Quand on va créer la réplication on fait aussi une planification (**Job**) :

![10-Job](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_10-Job.png)

### Permissions

Les **Permissions** permettent de créer/gérer des utilisateurs, groups, pools, roles, realms, etc

![11-Permissions](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_11-Permissions.png)


Pour la création d’un **user** il faut choisir « Proxmox VE authentical » sur Realm


![12-User](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_12-User.png)


**Pools** va être un regroupement logique (revoir replay du matin)


![13-Pools](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_13-Pools.png)


**Attention à l’attribution des rôles** : Ils sont présentés par concept (Name) et décrits par Privilèges

![14-Roles](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_14-Roles.png)


Sinon, on pourra aussi créer des **nouveaux rôles** (names) avec ses privilèges :


![15-NewRoles](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_15-NewRoles.png)


**Attention** : Les **permissions** (droits) sont à être données plutôt par groups, car le faire par user nous demandera de justifier pourquoi on différence un utilisateur du restant d’utilisateurs

! [16-Permissions](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_16-Permissions.png)


### HA 

**HA** (_High Availability_) permet de 

![17-HigAvailability](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_17-HighAvailability.png)


### SDN

**SDN** (_Software Defined Network_) sert à la création des VLAN. 


![18-SDN](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_18-Software%20Defined%20Network.png)


### Firewall

Le **Firewall** permet de filtrer les tentatives d’intrusion, pour contrôler et sécuriser le trafic.


![19-Firewall](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B1%20(Virtualisation)/images%20B1/images%20B102/B102_19-Firewall.png)

---

Challenge du jour 👉 [Challenge_B102](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B1/Challenge_B102.md) 👈

---
