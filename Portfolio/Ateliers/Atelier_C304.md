# 🏅 Atelier C304 – IDS/IPS & SIEM
### François BARSOTTI

---

> 👉 [**Instructions**](https://github.com/O-clock-Aldebaran/SC3E04-Atelier-IDS-IPS) 

### 📚 Ressources complémentaires

* [Window 11 Pro sur Proxmox VE](https://www.youtube.com/watch?v=OobKut5Yq6s)

* [Wazuh installation guide](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html)

* [Wazuh official documentation for Linux](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html)

* [Wazuh official documentation for Windows](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html)

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

# Étape 1: Installation de Suricata (IDS/IPS)
## 1.1. Création de la machine Windows 11

Elle n'était pas existante dans mon hyperviseur, alors j'ai dû tout installer (j'avais oublier comment c'est long pour avoir une Win11 Pro fonctionnelle)


## 1.2. Création de la machine Suricata

J'ai choisi de la créer en tant que VM, car en CT elle ne m'acceptait pas le mdp pour y rentrer. 

```
→ Note pour les CT: La première fois de connexion, il faut se connecter en tant que `"root"`
```

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
```bash
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

```bash
vars:
  address-groups:
    HOME_NET: "[10.0.0.0/16]"
    EXTERNAL_NET: "!$HOME_NET"
```

### 1.5.2. Définition de l'interface d'écoute :

Section `af-packet` :
```bash
af-packet:
  - interface: ens18
    cluster-id: 99
    cluster-type: cluster_flow
    defrag: yes
```
💡 **Rappel**: `ens18` pour les VM Proxmox avec VirtIO, mais `eth0` pour les CT. 

### 1.5.3. Activation de détails dans les logs EVE JSON :

→ Jamais oublier le `F6` pour chercher un mot clé dans nano ! Alors, Chercher la section `outputs → eve-log → types → alert` pour décommenter les lignes `payload`, `payload-printable` et `packet` et les laisser comme il suit:

```bash
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

```bash
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```
et on voit qu'il est bien actif 

![05-SuricataDémarré](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_05-SuricataD%C3%A9marr%C3%A9.png)

Avec `tail -f /var/log/suricata/suricata.log` on vérifie les logs de démarrage (**Attention**: au premier démarrage, Suricata peut mettre 30 secondes à 2 minutes pour charger toutes les règles):

![06-SuricataLogsDémarrés](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_06-SuricataLogsD%C3%A9marr%C3%A9s.png)

# Étape 2 : Gestion d'un événement de test

## 2.1. Déclenchage d'une règle connue

Pour retourne volontairement la chaîne `uid=0(root)` dans sa réponse HTTP et déclencher, en conséquence, la règle `ET ATTACK_RESPONSE` (SID 2100498), on exécute l'url suivant dans Suricata:

```
sudo apt install curl -y
curl http://testmynids.org/uid/index.html
```
![07-SuricataRègleConnue](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_07-SuricataR%C3%A8gleConnue.png)

L'alerte pour information ressemble a ça dans les régles Suricata :

```bash
alert ip any any -> any any (msg:"GPL ATTACK_RESPONSE id check returned root"; content:"uid=0|28|root|29|"; classtype:bad-unknown; sid:2100498; rev:7;)
```
## 2.2. Vérification de l'alerte dans les logs

On installe `jq` pour lire le JSON facilement :

```
sudo apt install -y jq
```
Puis  on vérifie les alertes :

```bash
cat /var/log/suricata/eve.json | jq 'select(.event_type=="alert")'
```
![08-SuricataAlerteDansLogs](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_08-SuricataAlerteDansLogs.png)

On peut aussi consulter le log simplifié :

```
cat /var/log/suricata/fast.log
```

![09-SuricataAlerteLogsSimplifiés](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_09-SuricataAlerteLogsSimplifi%C3%A9s.png)

Le fait de voir cette alerte nous dit que Suricata fonctionne correctement ! 🎉

Il a détecté le contenu uid=0(root) dans la réponse HTTP et a déclenché la règle correspondante.

## 2.3. Autres tests possibles

| Test 1 | Commande | Règle déclenchée |
| ---- | ----- | ----- |
| Simulation d’attaque | `curl testmynids.org` | ET MALWARE TestMyIDS |

Ce premier test était bien intéressant pour vérifier que les règles ET sont chargées et que l'on est bien en mode IDS (et pas uniquement capture passive mal configurée): `testmynids.org` renvoyait volontairement une signature d’attaque connue (une chaîne EICAR modifiée). Cependant, le site ne renvoie plus la signature test comme elle l'indique ("just a placeholder...we do not host any illegal or malicious content"). Donc c'est normal que Suricata ne déclenche rien.


![10-SuricataTest1](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_10-SuricataTest1.png)


| Test 2 | Commande | Règle déclenchée |
| ---- | ----- | ----- |
| Scan de ports | `nmap -sS 10.0.0.50` (depuis une autre VM) | ET SCAN / GPL SCAN |

Rappel: `sudo snap intall nmap # version 7.95`pour télécherger sur l'autre VM

![11-SuricataTest2](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_11-SuricataTest2.png)


| Test 3 | Commande | Règle déclenchée |
| ---- | ----- | ----- |
| Requête DNS suspecte | `dig @8.8.8.8 testmynids.org` | Possible ET DNS |

![12-SuricataTest3](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_12-SuricataTest3.png)


| Test 4 | Commande | Règle déclenchée |
| ---- | ----- | ----- |
| Ping ICMP | `ping -c 5 10.0.0.50` | ICMP rules (si activées) |

![13-SuricataTest4](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_13-SuricataTest4.png)

| Test 5 | Commande | Règle déclenchée |
| ---- | ----- | ----- |
| Requête HTTP simple (GET) | `curl http://testphp.vulnweb.com` | ET INFO Outbound HTTP Request |

![14-SuricataTest5](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_14-SuricataTest5.png)

# Étape 3 : Installation de Wazuh (SIEM)

## 3.1. C'est quoi un SIEM?

```
       Sources                          SIEM Wazuh
    ┌──────────┐                   ┌──────────────────┐
    │ Suricata │──── eve.json ────>│  Wazuh Manager   │
    │  (IDS)   │   via agent       │        │         │
    └──────────┘                   │        ▼         │
                                   │  Wazuh Indexer   │
    ┌──────────┐                   │        │         │
    │  Win11   │──── syslog ──────>│        ▼         │
    │ (cible)  │   via agent       │ Wazuh Dashboard  │
    └──────────┘                   └──────────────────┘
```

## 3.2. Création de la VM Wazuh

J'avais installé sur VM Ubuntu 24.04 en considérant que les CT ne sont plus une option car Wazuh nécessite d'un accès système complet et de paramètres kernel spécifiques (`vm.max_map_count`) pour des composants propres, comme l'indexer basé sur OpenSearch.

Mais je ne l'ai pas trouvé stable lors de l'installation de Wazuh. Alors j'ai tout refait sur une VM Debian 12.

## 3.3. Configuration du réseau

Une fois Debian installé, nouvelle procédure pour établir l'ip statique avec `nano /etc/network/interfaces` :

```
        auto ens18
        iface ens18 inet static
            address 10.0.0.40
            netmask 255.255.0.0
            gateway 10.0.0.1
            dns-nameservers 8.8.8.8
```
`systemctl restart networking` 

Puis en vérifie avec `ip addr` ou `ip route` (sur Ubuntu)

Vérification:

```
ping 10.0.0.1       # Gateway
ping 10.0.0.50      # Suricata
ping 8.8.8.8        # Internet
ping 10.0.0.10      # Windows 11 (sans parefeu ICMP)
```
![15-WazuhPingsAll](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_15-WazuhPingsAll.png)

## 3.4. Installation de Wazuh (tout-en-un)

L'installation tout-en-un déploie les trois composants sur la même VM :

```
su -
apt update
apt install curl -y
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

⚠️ **Attention** : L'installation prend entre 5 et 15 minutes selon les performances du serveur. Il ne faut surtout pas l'enterrompre.

S'il finit de s'installer, sans montrer el mot de passe généré, il faudra le récupérer:

```
tar -xvf /home/wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt
cat wazuh-install-files/wazuh-passwords.txt
```
Si rien ne s'affiche, il faudra désinstaller et réinstaller Wazuh:

```
sudo bash ./wazuh-install.sh -a -o
```
## 3.5. Vérification des services

```
systemctl status wazuh-manager
systemctl status wazuh-indexer
systemctl status wazuh-dashboard
```
![16-WazuhRunning](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_16-WazuhRunning.png)

## 3.6. Accès à l'interface web

On ouvre depuis la Win11 du LAN avec l'ip de Wazuh (`https://10.0.0.40`) en s'enregistre avec les donnéés générées aléatoirement par l'installation de Wazuh (en étape 3.4):

![17-AccèsInterfaceWazuh](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_17-Acc%C3%A8sInterfaceWazuh.png)

# Étape 4 : Connexion des sources
## 4.1. Installation de l'agent Wazuh sur Suricata

L'agent Wazuh est un programme léger qui collecte les logs locaux et les envoie au Manager. Depuis la machine Suricata (10.0.0.50) :

```bash
sudo apt install gpg -y

curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring /usr/share/keyrings/wazuh.gpg --import

sudo chmod 644 /usr/share/keyrings/wazuh.gpg

echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list

sudo apt update

sudo WAZUH_MANAGER="10.0.0.40" apt install -y wazuh-agent
```
Activation, démarrage et vérification de l'agent

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
sudo systemctl status wazuh-agent
```

![18-AgentWazuhActif](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_18-AgentWazuhActif.png)

## 4.2. Vérification de connexion de l'agent

**Côté Wazuh Manager** (10.0.0.40) :

```
/var/ossec/bin/manage_agents -l
```

![19-AgentWazuhConnecté](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_19-AgentWazuhConnect%C3%A9.png)

**Côté Dashboard** (10.0.0.10): on va dans `Agents Management` → on voit l'agent avec son nom, son IP (10.0.0.50) et le statut Active (point vert).

![20-AgentWazuhDashboard](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_20-AgentWazuhDashboard.png)

## 4.3. Configuration de collecte des logs Suricata

Par défaut, l'agent Wazuh collecte les logs système (auth.log, syslog…). Il faut lui dire de lire aussi le fichier eve.json de Suricata.

Sur la machine Suricata, il faut éditer la configuration de l'agent :

```
nano /var/ossec/etc/ossec.conf
```
Ajouter le bloc suivant dans la section `<ossec_config>`, avant la balise fermante `</ossec_config>` :

```bash
<!-- Collecte des alertes Suricata -->
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

On redémarre l'agent pour prendre en compte la modification :

```
sudo systemctl restart wazuh-agent
sudo systemctl status wazuh-agent --no-pager -l
```
## 4.4. Vérification de réception des événements

Côté Dashboard qu'on a depuis Windows (https://10.0.0.40)
* Explore → Discover
* On peut y jouer avec la plage de temps souhaitée
* On y peut bien voir les derniers événements faits avec l'agent Suricata

![21-SuricataBoard](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Ateliers/images_ateliers/Atelier_C304/Atelier%20C304_21-SuricataBoard.png)

## 4.5. Installation d'un agent sur Win11

### **En construction... to be continued...**
