# 🏅 Atelier C304 – IDS/IPS & SIEM
### François BARSOTTI

---

> 👉 [**Instructions**](https://github.com/O-clock-Aldebaran/SC3E04-Atelier-IDS-IPS) 

📚 **Ressources complémentaires**

* [Window 11 Pro sur Proxmox VE](https://www.youtube.com/watch?v=OobKut5Yq6s)

---

## 🎯 Contexte et pitch de l'atelier

>### Prérequis
>Avant de commencer, assurez-vous d'avoir les éléments suivants :
>
>* Une VM pfSense fonctionnelle (firewall/gateway).
>* Une VM Windows 11 (ou tout autre client sur le LAN).
>* Un accès root ou des privilèges sur les machines.
>* Une connexion internet pour télécharger les paquets nécessaires.
>* **Minimum 10 Go de RAM disponible** sur votre serveur Proxmox (Wazuh est gourmand !).
>
>Nous allons créer deux nouvelles machines : une pour Suricata (IDS/IPS) et une pour Wazuh (SIEM). Elles seront toutes les deux sur le LAN (vmbr2).
>
>### Introduction
>Dans cet atelier, nous allons mettre en place une chaîne de détection et de supervision complète. L'idée est simple : un capteur réseau (Suricata) détecte les menaces, puis remonte ses alertes vers un SIEM centralisé (Wazuh) pour la visualisation et la corrélation. C'est exactement ce qu'on retrouve dans un SOC (Security Operations Center) en entreprise.
>
>#### Architecture du lab :
>```
>          Internet
>             │
>      ┌──────┴──────┐
>      │  pfSense    │
>      │  10.0.0.1   │
>      └──────┬──────┘
>             │
>    ─────── vmbr2 — LAN 10.0.0.0/16 ────────
>       │           │             │
>  ┌────┴────┐ ┌────┴─────┐  ┌────┴──────┐
>  │  Win11  │ │ Suricata │  │   Wazuh   │
>  │ .0.10   │ │  .0.50   │  │   .0.40   │
>  │ (cible) │ │ (IDS/IPS)│  │  (SIEM)   │
>  │ (cible) │ │ (IDS/IPS)│  │  (SIEM)   │
>  └─────────┘ └──────────┘  └───────────┘
>```
>
>| **Machine** | **Type** | **IP** | **RAM** | **Rôle** |
>| ------------ | ----------------------- | ------------- | ------------- | ------------------------------ |
>| pfSense      | VM (existante)          | 10.0.0.1      | 1 Go          | Firewall / Gateway             |
>| Win11        | VM          | 10.0.0.10     | 4 Go          | VM cible                       |
>| **Suricata** | **VM ou CT (nouvelle)** | **10.0.0.50** | **2 Go**      | **IDS/IPS + Agent Wazuh**      |
>| **Wazuh**    | **VM (nouvelle)**       | **10.0.0.40** | **Mini 4 Go** | **SIEM (Manager + Dashboard)** |

---

## 1.1. Création de la machine Windows 11

Elle n'était pas existante dans mon hyperviseur, alors j'ai dû tout installer (j'avais oublier comment c'est long pour avoir une Win11 Pro fonctionnelle)



## 1.2. Création de la machine Suricata

J'ai choisi de la créer en tant que VM, car en CT elle ne m'acceptait pas le mdp pour y rentrer.

Pour configurer une IP statique, une fois installée, j'ai fait plutôt la commande: `sudo nano /etc/netplan/00-installer-config.yaml` car sur Ubuntu Server on passe plutôt par Netplan pour cette configuration, et le renseignement sur nano change un peu également:

```
    network:
    version: 2
    renderer: networkd
    ethernets:
        ens18:
        dhcp4: no
        addresses:
            - 10.0.0.50/16
        routes:
            - to: default
            via: 10.0.0.1
        nameservers:
            addresses:
            - 8.8.8.8
            - 1.1.1.1
            search:
            - suricata.local
```
`sudo netplan apply` ou bien `sudo netplan try`, si l'on est en ssh.

Pui en vérifie avec `ip addr` ou `ip route` (sur Ubuntu)

![01-SuricataIP](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_01-SuricataIP.png)

## 1.3. Vérification de connectivité

Quelle que soit l'option choisie (CT ou VM), vérifiez la connectivité :

```
ping 10.0.0.1      # Gateway pfSense
ping 8.8.8.8       # Internet (via pfSense)
```
Pour lancer un ping vers Windows 11 aussi (`ping 10.0.0.10`), on active les requêtes ICMP du parefeau Windows: 

`wf.msc` → Inbound rules → File and Printer Sharing (Echo Request - ICMPv4-In)

![02-PingpfSense&Google&Win11](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_02-PingpfSense%26Google%26Win11.png)

Bien sûr, une fois le ping testé, on éteint la permission de requête ICMP pour les désactiver à nouveau du parefeu

## 1.4. Installation de Suricata
```
sudo apt update && apt upgrade -y
sudo apt install -y suricata suricata-update
```
et on vérifie l'installation
```
suricata --build-info | head -5
```
![03-SuricataVersion](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_03-SuricataVersion.png)

## 1.5. Configuration de Suricata

On fait l'édition du fichier de configuration principal:

```
sudo nano /etc/suricata/suricata.yaml
```
### 1.5.1. Définition du réseau à protéger (HOME_NET):

Section `vars → address-groups` :

```
vars:
  address-groups:
    HOME_NET: "[10.0.0.0/16]"
    EXTERNAL_NET: "!$HOME_NET"
```

### 1.5.2. Définition de l'interface d'écoute :

Section `af-packet` :
```
af-packet:
  - interface: ens18
    cluster-id: 99
    cluster-type: cluster_flow
    defrag: yes
```
**Rappel**: `ens18` pour les VM Proxmox avec VirtIO, mais `eth0` pour les CT. 

### 1.5.3. Activation de détails dans les logs EVE JSON :

→ Jamais oublier le `F6` pour chercher un mot clé dans nano ! Alors, Chercher la section `outputs → eve-log → types → alert` pour décommenter les lignes `payload`, `payload-printable` et `packet` et les laisser comme il suit:

```
- alert:
    payload: yes
    payload-printable: yes
    packet: yes
    tagged-packets: yes
```

Attention ! On doit respecter l'indentation ! YAML est très sensible à l'indentation. Les options payload, payload-printable et packet doivent être au même niveau d'indentation que tagged-packets (4 espaces).

💡 Pourquoi décommenter ces lignes ? Par défaut, Suricata ne log que le minimum (signature, IP, port). En activant `payload` et `payload-printable`, on aura le contenu du paquet qui a déclenché l'alerte — très utile pour investiguer dans Wazuh. Le fichier `eve.json` est le format de log structuré que Wazuh lira pour récupérer les alertes (`eve-log` doit être aussi bien activé).

## 1.6. Télécharement des règles

`sudo suricata-update` pour télécharger et intaller les règles dans `/var/lib/suricata/rules/suricata.rules`.

![04-SuricataUpdate](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_04-SuricataUpdate.png)

Puis, on peut vérifier le nombre de règles chargées avec `grep -c "^alert" /var/lib/suricata/rules/suricata.rules`

→ à l'ocassion, on en a `48 730` 

## 1.7. Démarrage de Suricata


