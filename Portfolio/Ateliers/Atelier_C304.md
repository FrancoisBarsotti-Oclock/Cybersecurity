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
>| Win11        | VM (existante)          | 10.0.0.10     | 4 Go          | VM cible                       |
>| **Suricata** | **VM ou CT (nouvelle)** | **10.0.0.50** | **2 Go**      | **IDS/IPS + Agent Wazuh**      |
>| **Wazuh**    | **VM (nouvelle)**       | **10.0.0.40** | **Mini 4 Go** | **SIEM (Manager + Dashboard)** |

---

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



