# Pour Atelier C304 : IDS/IPS & SIEM

Suivez les différentes étapes ci-dessous, **en cas de problème merci de poster un message suffisamment détaillé sur le canal _#entraide_** de notre Slack de promo !

> Solo ou en groupe ?

Toutes les manipulations sont à effectuer en solo, sur votre serveur Proxmox. Mais vous pouvez vous mettre en groupe pour vous entraider.

> Durée de l'atelier ?

Une journée entière sera suffisante pour réaliser cet atelier pratique. Nous avons prévu un temps confortable qui vous permettra d'effectuer toutes les manipulations à votre rythme.

<aside>

## **Prérequis**

Avant de commencer, assurez-vous d'avoir les éléments suivants :

- Une VM pfSense fonctionnelle (firewall/gateway).
- Une VM Windows 11 (ou tout autre client sur le LAN).
- Un accès root ou des privilèges sur les machines.
- Une connexion internet pour télécharger les paquets nécessaires.
- **Minimum 10 Go de RAM disponible** sur votre serveur Proxmox (Wazuh est gourmand !).

Nous allons créer deux nouvelles machines : une pour **Suricata** (IDS/IPS) et une pour **Wazuh** (SIEM). Elles seront toutes les deux sur le LAN (vmbr2).

</aside>

## **Introduction**

Dans cet atelier, nous allons mettre en place une **chaîne de détection et de supervision** complète. L'idée est simple : un capteur réseau (**Suricata**) détecte les menaces, puis remonte ses alertes vers un SIEM centralisé (**Wazuh**) pour la visualisation et la corrélation. C'est exactement ce qu'on retrouve dans un **SOC** (Security Operations Center) en entreprise.

**Architecture du lab :**

```
          Internet
              │
       ┌──────┴──────┐
       │  pfSense    │
       │  10.0.0.1   │
       └──────┬──────┘
              │
     ─────── vmbr2 — LAN 10.0.0.0/16 ────────
        │           │             │
   ┌────┴────┐ ┌────┴─────┐  ┌────┴──────┐
   │  Win11  │ │ Suricata │  │   Wazuh   │
   │ .0.10   │ │  .0.50   │  │   .0.40   │
   │ (cible) │ │ (IDS/IPS)│  │  (SIEM)   │
   └─────────┘ └──────────┘  └───────────┘
```

| Machine      | Type                    | IP            | RAM           | Rôle                           |
| ------------ | ----------------------- | ------------- | ------------- | ------------------------------ |
| pfSense      | VM (existante)          | 10.0.0.1      | 1 Go          | Firewall / Gateway             |
| Win11        | VM (existante)          | 10.0.0.10     | 4 Go          | VM cible                       |
| **Suricata** | **VM ou CT (nouvelle)** | **10.0.0.50** | **2 Go**      | **IDS/IPS + Agent Wazuh**      |
| **Wazuh**    | **VM (nouvelle)**       | **10.0.0.40** | **Mini 4 Go** | **SIEM (Manager + Dashboard)** |

# Étape 1 : Installer Suricata (IDS/IPS)

## **Étape 1.1 : Un IDS/IPS c'est quoi ?**

Un **IDS** (Intrusion Detection System) surveille le trafic réseau et génère des alertes quand il détecte un comportement suspect. Un **IPS** (Intrusion Prevention System) fait la même chose, mais peut en plus **bloquer** le trafic malveillant.

**Suricata** est un moteur open-source qui peut fonctionner dans les deux modes :

| Mode             | Fonctionnement                                | Avantage                   | Inconvénient                                    |
| ---------------- | --------------------------------------------- | -------------------------- | ----------------------------------------------- |
| **IDS** (passif) | Écoute une copie du trafic, alerte uniquement | Aucun impact sur le réseau | Ne bloque rien                                  |
| **IPS** (inline) | Se place en coupure du trafic, peut bloquer   | Protection active          | Peut couper du trafic légitime si mal configuré |

Suricata compare chaque paquet réseau à un ensemble de **règles** (signatures). Chaque règle décrit un pattern malveillant connu :

```
alert http any any -> any any (msg:"ET ATTACK_RESPONSE id check returned root"; content:"uid=0(root)"; sid:2100498; rev:7;)
```

<aside>

Voici un résumé de la syntaxe d'une règle Suricata :

- `alert` : action à effectuer (alerter, drop, pass...)
- `http` : protocole surveillé
- `any any -> any any` : source/destination (ici tout le trafic)
- `content:"uid=0(root)"` : le motif à chercher dans le paquet
- `sid:2100498` : identifiant unique de la règle (Signature ID)

</aside>

## **Étape 1.2 : Créer la machine Suricata (VM ou CT)**

Vous avez le choix entre créer une **VM** ou un **conteneur LXC (CT)** pour Suricata. Un CT est plus léger et consomme moins de ressources, ce qui peut être intéressant si votre serveur Proxmox est limité.

**Option A — Créer un CT (recommandé si ressources limitées) :**

Dans Proxmox → **Créer CT** :

| Onglet   | Champ             | Valeur                               |
| -------- | ----------------- | ------------------------------------ |
| Général  | CT ID             | 400 (ou autre libre)                 |
| Général  | Hostname          | Suricata                             |
| Général  | Mot de passe      | Un mot de passe de votre choix       |
| Template | Template          | debian-13-standard (ou ubuntu-24.04) |
| Disque   | Taille            | 20 Go                                |
| CPU      | Cœurs             | 2                                    |
| Mémoire  | RAM               | 2048 Mo (2 Go)                       |
| Réseau   | Bridge            | **vmbr2**                            |
| Réseau   | IPv4              | **Static**                           |
| Réseau   | Adresse IPv4/CIDR | **10.0.0.50/16**                     |
| Réseau   | Passerelle        | **10.0.0.1**                         |
| DNS      | Serveur DNS       | 8.8.8.8                              |

Cliquez **Terminer** → **Démarrer**.

<aside>

**Option B — Créer une VM :**

Dans Proxmox → **Créer VM** :

| Onglet  | Champ     | Valeur                             |
| ------- | --------- | ---------------------------------- |
| Général | VM ID     | 400 (ou autre libre)               |
| Général | Nom       | Suricata                           |
| OS      | ISO Image | debian-13 (ou ubuntu-24.04 server) |
| Système | Type      | Linux                              |
| Disque  | Taille    | 20 Go                              |
| CPU     | Cœurs     | 2                                  |
| Mémoire | RAM       | 2048 Mo (2 Go)                     |
| Réseau  | Bridge    | **vmbr2**                          |
| Réseau  | Modèle    | VirtIO                             |

Cliquez **Terminer** → **Démarrez** → installez le système (installation minimale, pas d'interface graphique).

Après l'installation, configurez une IP statique :

```bash
nano /etc/network/interfaces
```

```
auto ens18
iface ens18 inet static
    address 10.0.0.50
    netmask 255.255.0.0
    gateway 10.0.0.1
    dns-nameservers 8.8.8.8
```

```bash
systemctl restart networking
```

## **Étape 1.3 : Vérifier la connectivité**

Quelle que soit l'option choisie (CT ou VM), vérifiez la connectivité :

```bash
ping 10.0.0.1      # Gateway pfSense
ping 8.8.8.8       # Internet (via pfSense)
```

<aside>

Attention !

Si le ping vers Internet ne fonctionne pas, vérifiez que pfSense autorise le trafic sortant depuis le LAN et que le NAT est actif sur l'interface WAN. C'est nécessaire pour télécharger les paquets Suricata.

⚠️ **Ne testez pas avec `ping 10.0.0.10` (Win11)** à ce stade : par défaut, le pare-feu Windows bloque les requêtes ICMP (ping). Un timeout ne signifie pas que le réseau ne fonctionne pas ! Si vous voulez vérifier, vous pouvez autoriser le ping côté Windows dans les règles du pare-feu, ou simplement tester avec la gateway pfSense qui répond toujours.

</aside>

## **Étape 1.4 : Installer Suricata**

```bash
apt update && apt upgrade -y
apt install -y suricata suricata-update
```

Vérifiez l'installation :

```bash
suricata --build-info | head -5
```

![alt text](images/image.png)

## **Étape 1.5 : Configurer Suricata**

Éditez le fichier de configuration principal :

```bash
nano /etc/suricata/suricata.yaml
```

**1. Définir le réseau à protéger (HOME_NET) :**

Cherchez la section `vars` → `address-groups` :

```yaml
vars:
  address-groups:
    HOME_NET: "[10.0.0.0/16]"
    EXTERNAL_NET: "!$HOME_NET"
```

<aside>

💡 HOME_NET doit correspondre à votre réseau LAN. Suricata utilise cette variable pour savoir quel trafic est "interne" et quel trafic est "externe". Si HOME_NET est mal défini, les règles ne se déclencheront pas correctement.

</aside>

**2. Définir l'interface d'écoute :**

Cherchez la section `af-packet` :

```yaml
af-packet:
  - interface: eth0
    cluster-id: 99
    cluster-type: cluster_flow
    defrag: yes
```

<aside>

💡 Vérifiez le nom de votre interface avec `ip a`. Dans un CT c'est généralement `eth0`, dans une VM Proxmox avec VirtIO c'est `ens18`. Adaptez en conséquence.

</aside>

**3. Activer les détails dans les logs EVE JSON :**

Cherchez la section `outputs` → `eve-log` → `types` → `alert`. Par défaut, les options intéressantes sont commentées :

```yaml
- alert:
    # payload: yes             # enable dumping payload in Base64
    # payload-printable: yes   # enable dumping payload in printable (lossy) format
    # packet: yes              # enable dumping of packet (without stream segments)
```

Décommentez les lignes `payload`, `payload-printable` et `packet` pour obtenir :

```yaml
- alert:
    payload: yes
    payload-printable: yes
    packet: yes
    tagged-packets: yes
```

<aside>

Attention !
Vous devez respecter l'indentation ! YAML est très sensible à l'indentation. Les options `payload`, `payload-printable` et `packet` doivent être au même niveau d'indentation que `tagged-packets` (4 espaces).

![alt text](images/image-3.png)

💡 **Pourquoi décommenter ces lignes ?** Par défaut, Suricata ne log que le minimum (signature, IP, port). En activant `payload` et `payload-printable`, vous aurez le contenu du paquet qui a déclenché l'alerte — très utile pour investiguer dans Wazuh. Le fichier `eve.json` est le format de log structuré que Wazuh lira pour récupérer les alertes.

</aside>

<aside>

💡 Vérifiez aussi que `eve-log` est bien activé (`enabled: yes`) plus haut dans la section. C'est normalement le cas par défaut, mais mieux vaut vérifier :

```yaml
outputs:
  - eve-log:
      enabled: yes
      filetype: regular
      filename: eve.json
```

</aside>

![alt text](images/image-2.png)

## **Étape 1.6 : Télécharger les règles**

Suricata utilise des jeux de règles communautaires. Le plus courant est **Emerging Threats Open** :

```bash
suricata-update
```

![alt text](images/image-1.png)

Cette commande télécharge et installe les règles dans `/var/lib/suricata/rules/suricata.rules`.

Vérifiez le nombre de règles chargées :

```bash
grep -c "^alert" /var/lib/suricata/rules/suricata.rules
```

Vous devriez avoir plusieurs dizaines de milliers de règles.

## **Étape 1.7 : Démarrer Suricata**

```bash
systemctl enable suricata
systemctl start suricata
systemctl status suricata
```

![alt text](images/image-4.png)

Vérifiez les logs de démarrage :

```bash
tail -f /var/log/suricata/suricata.log
```

Attendez de voir la ligne :

![alt text](images/image-5.png)

Cela signifie que Suricata écoute activement le trafic sur l'interface réseau.

<aside>

Attention !

Au premier démarrage, Suricata peut mettre 30 secondes à 2 minutes pour charger toutes les règles. Soyez patient et surveillez les logs. Si Suricata ne démarre pas, vérifiez la syntaxe de votre fichier yaml avec :

`suricata -T -c /etc/suricata/suricata.yaml`

![alt text](images/image-6.png)

</aside>

# Étape 2 : Générer un événement de test

## **Étape 2.1 : Déclencher une règle connue**

Depuis la VM/CT Suricata, exécutez :

```bash
apt install curl -y
curl http://testmynids.org/uid/index.html
```

![alt text](images/image-7.png)

Cette URL retourne volontairement la chaîne `uid=0(root)` dans sa réponse HTTP, ce qui déclenche la règle **ET ATTACK_RESPONSE** (SID 2100498).

L'alerte pour information ressemble a ça dans les régles Suricata :

```bash

alert ip any any -> any any (msg:"GPL ATTACK_RESPONSE id check returned root"; content:"uid=0|28|root|29|"; classtype:bad-unknown; sid:2100498; rev:7;)
```

## **Étape 2.2 : Vérifier l'alerte dans les logs**

Installez `jq` pour lire le JSON facilement :

```bash
apt install -y jq
```

Puis vérifiez les alertes :

```bash
cat /var/log/suricata/eve.json | jq 'select(.event_type=="alert")'
```

Vous devriez voir quelque chose comme :

```json
{
  "timestamp": "2025-XX-XXTXX:XX:XX.XXXXXX+0000",
  "event_type": "alert",
  "src_ip": "10.0.0.50",
  "dest_ip": "XXX.XXX.XXX.XXX",
  "alert": {
    "action": "allowed",
    "signature_id": 2100498,
    "signature": "GPL ATTACK_RESPONSE id check returned root",
    "category": "Potentially Bad Traffic",
    "severity": 2
  }
}
```

![alt text](images/image-8.png)

Vous pouvez aussi consulter le log simplifié :

```bash
cat /var/log/suricata/fast.log
```

![alt text](images/image-9.png)

<aside>

> Si vous voyez cette alerte, Suricata fonctionne correctement ! 🎉

> Il a détecté le contenu `uid=0(root)` dans la réponse HTTP et a déclenché la règle correspondante.

</aside>

## **Étape 2.3 : Autres tests possibles**

| Test                 | Commande                                   | Règle déclenchée         |
| -------------------- | ------------------------------------------ | ------------------------ |
| Scan de ports        | `nmap -sS 10.0.0.50` (depuis une autre VM) | ET SCAN / GPL SCAN       |
| Requête DNS suspecte | `dig @8.8.8.8 testmynids.org`              | Possible ET DNS          |
| Ping ICMP            | `ping -c 5 10.0.0.50`                      | ICMP rules (si activées) |

<aside>

💡 Le test `curl testmynids.org` est le plus fiable pour un premier test car la règle est présente dans quasiment tous les jeux de règles ET.

</aside>

# Étape 3 : Installer Wazuh (SIEM)

## **Étape 3.1 : Un SIEM c'est quoi ?**

Un **SIEM** (Security Information and Event Management) est une plateforme qui collecte les logs de multiples sources (IDS, pare-feu, serveurs...), les normalise, les corrèle pour détecter des attaques, et alerte les analystes via un tableau de bord centralisé.

**Wazuh** est un SIEM open-source composé de trois briques :

| Composant           | Rôle                                                         | Port              |
| ------------------- | ------------------------------------------------------------ | ----------------- |
| **Wazuh Manager**   | Reçoit les logs des agents, applique les règles de détection | 1514, 1515, 55000 |
| **Wazuh Indexer**   | Stocke et indexe les événements (basé sur OpenSearch)        | 9200              |
| **Wazuh Dashboard** | Interface web de visualisation et d'investigation            | 443               |

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

## **Étape 3.2 : Créer la VM Wazuh**

Dans Proxmox → **Créer VM** :

| Onglet  | Champ     | Valeur                             |
| ------- | --------- | ---------------------------------- |
| Général | VM ID     | 401 (ou autre libre)               |
| Général | Nom       | Wazuh                              |
| OS      | ISO Image | debian-13 (ou ubuntu-24.04 server) |
| Système | Type      | Linux                              |
| Disque  | Taille    | 50 Go                              |
| CPU     | Cœurs     | 2 (idéalement 4)                   |
| Mémoire | RAM       | **8192 Mo (8 Go)**                 |
| Réseau  | Bridge    | **vmbr2**                          |
| Réseau  | Modèle    | VirtIO                             |

<aside>

Attention !

**8 Go de RAM est le minimum recommandé** pour une installation tout-en-un (Manager + Indexer + Dashboard). L'Indexer (OpenSearch) est très gourmand. En dessous de 4 Go, l'installation échouera.

⚠️ **Wazuh nécessite une VM, pas un CT.** Les composants Wazuh (notamment l'Indexer basé sur OpenSearch) ont besoin d'un accès système complet et de paramètres kernel spécifiques (`vm.max_map_count`) qui ne sont pas disponibles dans un conteneur LXC.

</aside>

Cliquez **Terminer** → **Démarrez** → installez le système.

## **Étape 3.3 : Configurer le réseau**

```bash
nano /etc/network/interfaces
```

```
auto ens18
iface ens18 inet static
    address 10.0.0.40
    netmask 255.255.0.0
    gateway 10.0.0.1
    dns-nameservers 8.8.8.8
```

```bash
systemctl restart networking
```

Vérification :

```bash
ping 10.0.0.1       # Gateway
ping 10.0.0.50      # Suricata
ping 8.8.8.8        # Internet
```

## **Étape 3.4 : Installer Wazuh (tout-en-un)**

L'installation tout-en-un déploie les trois composants sur la même VM :

```bash
su -
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

<aside>

Attention !

L'installation prend entre **5 et 15 minutes** selon les performances de votre serveur. Ne l'interrompez pas !

</aside>

À la fin de l'installation, le script affiche les **identifiants par défaut** :

```
INFO: --- Summary ---
INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
    User: admin
    Password: <mot_de_passe_généré>
```

![alt text](images/image-10.png)

<aside>

⚠️ **NOTEZ CE MOT DE PASSE.** Il est généré aléatoirement et ne sera plus affiché. Si vous le perdez :

```bash
tar -xvf /home/wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt
cat wazuh-install-files/wazuh-passwords.txt
```

</aside>

## **Étape 3.5 : Vérifier les services**

```bash
systemctl status wazuh-manager
systemctl status wazuh-indexer
systemctl status wazuh-dashboard
```

Les trois services doivent être **active (running)**.

## **Étape 3.6 : Accéder à l'interface web**

Depuis Win11 (ou toute machine du LAN), ouvrez un navigateur :

```
https://10.0.0.40
```

- **Utilisateur** : admin
- **Mot de passe** : celui noté à l'étape 3.4

<aside>

💡 Le certificat SSL est auto-signé. Votre navigateur affichera un avertissement de sécurité. Cliquez **Avancé** → **Accepter le risque** pour continuer.

</aside>

![alt text](images/image-11.png)

Vous devez voir le tableau de bord Wazuh avec le menu à gauche (Security Operations, etc.). L'onglet **Agents Management** doit être vide pour l'instant (aucun agent connecté).

# Étape 4 : Connecter les sources

## **Étape 4.1 : Installer l'agent Wazuh sur Suricata**

L'agent Wazuh est un programme léger qui collecte les logs locaux et les envoie au Manager. Depuis la machine Suricata (10.0.0.50) :

```bash
apt install gpg -y

curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg

echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee /etc/apt/sources.list.d/wazuh.list

apt update

WAZUH_MANAGER="10.0.0.40" apt install -y wazuh-agent
```

<aside>

💡 La variable `WAZUH_MANAGER="10.0.0.40"` indique à l'agent l'adresse du serveur Wazuh. Sans cette variable, l'agent ne saura pas où envoyer les logs.

</aside>

Activez et démarrez l'agent :

```bash
systemctl daemon-reload
systemctl enable wazuh-agent
systemctl start wazuh-agent
systemctl status wazuh-agent
```

![alt text](images/image-12.png)

## **Étape 4.2 : Vérifier la connexion de l'agent**

**Côté Wazuh Manager** (10.0.0.40) :

```bash
/var/ossec/bin/manage_agents -l
```

![alt text](images/image-13.png)

Vous devez voir l'agent Suricata avec le statut **Active**.

**Côté Dashboard** : Allez dans **Agents Management** → vous devez voir l'agent avec son nom, son IP (10.0.0.50) et le statut **Active** (point vert).

![alt text](images/image-14.png)

## **Étape 4.3 : Configurer la collecte des logs Suricata**

Par défaut, l'agent Wazuh collecte les logs système (auth.log, syslog…). Il faut lui dire de lire aussi le fichier **eve.json** de Suricata.

Sur la machine Suricata, éditez la configuration de l'agent :

```bash
nano /var/ossec/etc/ossec.conf
```

Ajoutez le bloc suivant **dans la section `<ossec_config>`**, avant la balise fermante `</ossec_config>` :

```xml
<!-- Collecte des alertes Suricata -->
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

<aside>

![alt text](images/image-15.png)

Voici les points importants à vérifier :

- Le `log_format` doit être **json** (pas syslog). Wazuh sait parser nativement le format EVE JSON de Suricata.
- Le chemin doit correspondre exactement au fichier configuré dans `suricata.yaml`.
- L'utilisateur `wazuh` (ou `ossec`) doit avoir les droits de lecture sur le fichier : `chmod 644 /var/log/suricata/eve.json`

</aside>

Redémarrez l'agent pour prendre en compte la modification :

```bash
systemctl restart wazuh-agent
```

## **Étape 4.4 : Vérifier la réception des événements**

Retournez sur le **Dashboard Wazuh** (https://10.0.0.40) :

1. Allez dans **Explore => Discover**
2. Sélectionnez une plage de temps récente (ex : « Last 15 minutes »).

![alt text](images/image-16.png)

3. Vous devez voir des événements apparaître avec `agent.name: Suricata`.

![alt text](images/image-17.png)

<aside>

💡 Si vous ne voyez rien, générez du trafic sur la machine Suricata (`curl testmynids.org`, `apt update`, navigation web…) et attendez 1-2 minutes que les logs soient transmis.

</aside>

## **Étape 4.5 : (Optionnel) Installer un agent sur Win11**

Pour enrichir le SIEM avec les logs de la VM cible :

1. Depuis Win11, téléchargez l'agent Wazuh Windows : `https://packages.wazuh.com/4.x/windows/wazuh-agent-4.13.3-1.msi`
2. Installez avec l'adresse du manager :

```powershell
wazuh-agent-4.13.3-1.msi /q WAZUH_MANAGER="10.0.0.40"
```

3. Démarrez le service :

```powershell
NET START Wazuh
```

![alt text](images/image-18.png)

Verifiez aussi dans les services Windows que le service Wazuh est bien démarré.

![alt text](images/image-19.png)

Vous verrez alors deux agents dans le Dashboard : Suricata et Win11.

# Étape 5 : Valider la détection de bout en bout

## **Étape 5.1 : Provoquer un événement de test**

Depuis la machine Suricata :

```bash
curl http://testmynids.org/uid/index.html
```

## **Étape 5.2 : Vérifier l'alerte locale (Suricata)**

```bash
tail -5 /var/log/suricata/fast.log
```

Vous devez voir l'alerte `GPL ATTACK_RESPONSE id check returned root`.

## **Étape 5.3 : Vérifier l'alerte dans Wazuh**

Dans le **Dashboard Wazuh** (https://10.0.0.40) :

1. Allez dans **Discover**.
2. Dans la barre de recherche, filtrez par : `rule.groups: suricata`
3. Ou recherchez : `data.alert.signature_id: 2100498`
4. Vous devez voir l'alerte avec tous les détails :

| Champ Wazuh         | Valeur attendue                                              |
| ------------------- | ------------------------------------------------------------ |
| rule.description    | Suricata: Alert - GPL ATTACK_RESPONSE id check returned root |
| data.alert.severity | 2                                                            |
| data.src_ip         | 10.0.0.50 (ou l'IP source)                                   |
| agent.name          | Suricata                                                     |

<aside>

> Si vous voyez cette alerte dans Wazuh, votre chaîne de détection est complète ! 🎉

> Le flux complet fonctionne : Trafic réseau → Suricata détecte → eve.json → Agent Wazuh → Manager → Indexer → Dashboard

</aside>

# Bonus : Règle personnalisée et corrélation

### **B.1 : Créer une règle Suricata personnalisée**

Créez un fichier de règles custom sur la machine Suricata :

```bash
nano /var/lib/suricata/rules/local.rules
```

Ajoutez une règle qui détecte un mot-clé spécifique dans le trafic HTTP :

```
alert http any any -> any any (msg:"CUSTOM - Mot secret detecte dans le trafic HTTP"; flow:established,to_server; content:"SuperSecret2025"; nocase; sid:1000001; rev:1; classtype:policy-violation;)
```

<aside>

Voici un résumé de la règle :

- `alert http` : déclencher une alerte sur le trafic HTTP
- `any any -> any any` : quelle que soit la source et la destination
- `content:"SuperSecret2025"` : chercher cette chaîne dans le contenu
- `nocase` : insensible à la casse
- `sid:1000001` : identifiant unique (les SID custom commencent à 1000001)
- `classtype:policy-violation` : catégorie de l'alerte

</aside>

### **B.2 : Activer la règle**

Éditez la configuration Suricata pour inclure le fichier de règles locales :

```bash
nano /etc/suricata/suricata.yaml
```

Dans la section `rule-files`, ajoutez :

```yaml
rule-files:
  - suricata.rules
  - local.rules
```

![alt text](images/image-20.png)

Redémarrez Suricata :

```bash
systemctl restart suricata
```

Vérifiez que la règle est chargée :

```bash
cat /var/log/suricata/suricata.log
```

![alt text](images/image-21.png)

### **B.3 : Déclencher la règle personnalisée**

Depuis n'importe quelle machine du LAN, envoyez une requête contenant le mot-clé :

```bash
curl http://10.0.0.50/SuperSecret2025
```

<aside>

💡 Même si le serveur web n'existe pas sur 10.0.0.50, Suricata analyse le trafic au niveau réseau et détectera le contenu `SuperSecret2025` dans la requête HTTP.

Si l'alerte ne se déclenche pas, essayez avec netcat pour simuler un échange :

Sur Suricata : `nc -l -p 8080`

Depuis une autre VM : `echo -e "GET /SuperSecret2025 HTTP/1.1\r\nHost: test\r\n\r\n" | nc 10.0.0.50 8080`

</aside>

### **B.4 : Vérifier dans Suricata**

```bash
cat /var/log/suricata/eve.json | jq 'select(.alert.signature_id==1000001)'
```

Vous devriez voir :

```json
{
  "alert": {
    "action": "allowed",
    "signature_id": 1000001,
    "signature": "CUSTOM - Mot secret detecte dans le trafic HTTP",
    "category": "Potential Corporate Privacy Violation",
    "severity": 3
  }
}
```

![alt text](images/image-22.png)

### **B.5 : Vérifier la corrélation dans Wazuh**

Dans le **Dashboard Wazuh** :

1. **Discover** → recherchez `data.alert.signature_id: 1000001`
2. L'alerte custom doit apparaître avec le message `CUSTOM - Mot secret detecte dans le trafic HTTP`

<aside>

> Vous avez démontré la corrélation complète ! 🎉

> Une règle **personnalisée** dans Suricata détecte un pattern spécifique, l'alerte remonte automatiquement dans le **SIEM Wazuh**, et un analyste SOC peut voir, filtrer et investiguer cette alerte depuis le Dashboard centralisé.

</aside>

# Dépannage

### Suricata ne démarre pas

| Vérification                           | Action                                             |
| -------------------------------------- | -------------------------------------------------- |
| Erreur de syntaxe dans suricata.yaml ? | `suricata -T -c /etc/suricata/suricata.yaml`       |
| L'interface existe-t-elle ?            | `ip a` → vérifiez le nom (eth0 en CT, ens18 en VM) |
| Permissions sur les logs ?             | `chown suricata:suricata /var/log/suricata/`       |
| Pas assez de RAM ?                     | `free -m` → minimum 1 Go libre                     |

### Suricata tourne mais aucune alerte

| Vérification                               | Action                                                    |
| ------------------------------------------ | --------------------------------------------------------- |
| HOME_NET est-il correct ?                  | Doit correspondre à votre réseau (10.0.0.0/16)            |
| Les règles sont-elles chargées ?           | `grep "rules loaded" /var/log/suricata/suricata.log`      |
| L'interface est-elle la bonne ?            | Vérifiez `af-packet: interface` dans suricata.yaml        |
| Le trafic passe-t-il par cette interface ? | `tcpdump -i eth0 -c 10` (ou ens18 en VM)                  |
| Le test est-il correct ?                   | `curl http://testmynids.org/uid/index.html` (pas HTTPS !) |
| (CT) Le mode promiscuous est-il activé ?   | Vérifiez la config LXC (voir étape 1.2)                   |

### Wazuh ne s'installe pas

| Vérification                      | Action                                    |
| --------------------------------- | ----------------------------------------- | --------- |
| Pas assez de RAM ?                | `free -m` → minimum 4 Go, idéalement 8 Go |
| Pas assez de disque ?             | `df -h` → minimum 20 Go libres            |
| Pas d'accès Internet ?            | `curl -s https://packages.wazuh.com`      |
| Le port 443 est-il déjà utilisé ? | `ss -tlnp                                 | grep 443` |

### L'agent ne se connecte pas au Manager

| Vérification                          | Action                                                     |
| ------------------------------------- | ---------------------------------------------------------- | -------------------------- |
| Le Manager écoute-t-il ?              | `ss -tlnp                                                  | grep 1514` sur la VM Wazuh |
| L'adresse Manager est-elle correcte ? | Vérifiez `/var/ossec/etc/ossec.conf` → `<server><address>` |
| Le pare-feu bloque-t-il ?             | `nc -zv 10.0.0.40 1514` depuis l'agent                     |
| L'agent est-il enregistré ?           | `/var/ossec/bin/manage_agents -l` sur le Manager           |

### Les alertes Suricata n'apparaissent pas dans Wazuh

| Vérification                          | Action                                                  |
| ------------------------------------- | ------------------------------------------------------- |
| Le bloc `<localfile>` est-il ajouté ? | Vérifiez `/var/ossec/etc/ossec.conf` sur l'agent        |
| Le chemin eve.json est-il correct ?   | `ls -la /var/log/suricata/eve.json`                     |
| L'agent peut-il lire le fichier ?     | `chmod 644 /var/log/suricata/eve.json`                  |
| L'agent a-t-il été redémarré ?        | `systemctl restart wazuh-agent`                         |
| Le format est-il `json` ?             | Vérifiez `<log_format>json</log_format>` (pas `syslog`) |

#