# 🔍 Session C305. IDS/ IPS – SIEM & CTI.

**Notions du jour:**
* IDS/IPS
* SIEM & CTI

## IDS / IPS : Suricata & Wazuh 🛡️
_ Détecter, alerter, bloquer_

### IDS - Intrusion Détection System

Un système qui **observe** le trafic ou l'activité et **alerte** quand quelque chose de suspect se passe

* 👁️ Il **regarde**
* 🔔 Il **prévient**
* 🚫 Mais il **ne bloque pas**

_**Analogie** : une caméra de surveillance qui envoie une alerte au vigile_

### IPS - Intrusion Prevention System

Un système qui **observe** le trafic ET **agit** automatiquement

* 👁️ Il **regarde**
* 🔔 Il **prévient**
* ✋ Et il **bloque** en temps réel !

_**Analogie** : un vigile qui voit ET qui plaque au sol_ 💪

### IDS vs IPS : la vraie différence

* **IDS** = mode passif  → il **copie** le trafic et l'analyse à côté
* **IPS** = mode inline → il est **sur le chemin** du trafic et peut le bloquer

_En pratique, beaucoup d'outils font les deux : on choisit le mode à la configuration_

## Les grandes familles 🏠

### Par emplacement

* **NIDS / NIPS** - **N**etwork : surveille le **réseau** (trafic qui passe) 🌐
* **HIDS / HIPS** - **H**ost : surveille une **machine** (fichiers, logs, processus) 💻

_NIDS regarde les paquets, HIDS regarde ce qui se passe **sur** le serveur_

### Par méthode de détection

* 📝 **Signatures** : patterns connus (comme un antivirus réseau)
* 📊 **Anomalies** : déviation par rapport à un comportement "normal"
* 📏 **Politiques** : violation de règles définies (ex: trafic interdit)

_En pratique, les outils modernes combinent les trois approches_

### Où les placer ?
* 🌐 **NIQS** - sur un port miroir du switch, ou en coupure (inline pour IPS)
* 💻 **HIDS** - installe comme agent sur chaque machine à surveiller

Le NIDS voit le trafic réseau, le HIDS voit ce qui se passe **localement**

**Les deux sont complémentaires !** 🤝

## Suricata 🔎 📢

_Le moteur IDS/IPS réseau open-source_
Suricata = la relève moderne

### C'est quoi Suricata ?

* Un **NIDS / NIPS** open-source (licence GPL)
* Développé par l'**OISF** (Open Information Security Foundation)
* Successeur spirituel de **Snort** (l'ancêtre des IDS réseau)

_Snort = le pionnier / Suricata = la relève moderne_

### Pourquoi Suricata ? 💡

* **Multi-thread** : exploite tous les cœurs CPU (Snort = mono-thread)
* **Protocoles** : HTTP, ITLS, DNS, SMB, SSH ... analyse en profondeur
* **Logs EVE JSON** : format structure, parfait pour un SIEM
* **Communauté active** : règles mises à jour en permanence

_C'est l'IDS/IPS recommande par l'ANSSI pour les infrastructures françaises_

### Les modes de fonctionnement
* 🔍 **IDS** (af-packet) : écoute passive sur une interface réseau
* ✋ **IPS** (nfqueue / af-packet inline) : en coupure, bloque le trafic malveillant
* 📁 **Offline** (pcap) : analyse de captures réseau existantes

_On commence toujours en mode IDS pour calibrer avant de passer en IPS !_ ⚠️

### L’architecture Suricata
```apache
Trafic réseau
     │
┌────▼────┐
│ Capture │ (af-packet / nfqueue)
└────┬────┘
     │
┌────▼────┐
│ Décodage│ (Ethernet → IP → TCP/UDP)
└────┬────┘
     │
┌────▼─────────┐
│ Détection    │ (règles / signatures)
└────┬─────────┘
     │
┌────▼────┐
│ Outputs │ → EVE JSON, alertes, pcap
└────┬────┘
```

### Les règles Suricata 📋

Une regle = une signature de trafic suspect
```ps
alert http $HOME_NET any -> $EXTERNAL_NET any (
msg:"Tentative accès admin panel";
content:"/admin"; http.uri;
sid:1000001; rev:1;
)
```

* **alert** : actionI(alert, drop, pass, reject)
* **http** : protocole surveillé
* **content** : pattern recherché dans le trafic
* **sid** : identifiant unique de la règle

### Les actions possibles

* **alert** : génère une alerte (mode IDS) 🔔
* **drop** : bloque ET alerte (mode IPS) 🚫
* **pass** : laisse passer sans alerter ✅
* **reject** : bloque ET envoie un RST/ICMP au client ❌

_En mode IDS, **drop** se comporte comme **alert** (on ne peut pas bloquersi on n'est pas inline)_

### Les sources de règles 📚

* **ET Open** (Emerging Threats) : gratuites, communautaires
* **ET Pro** : payantes, plus réactives
* **Règles custom** : adaptées à votre environnement

L'outil suricata-update gère la mise à jour automatique des règles 🔄

### Les logs EVE JSON
Suricata produit des logs au format **EVE JSON** : un événement = une ligne JSON

```apache
{
    "timestamp": "2024-03-15T14:22:01",
    "event_type": "alert",
    "src_ip": "192.168.1.42",
    "dest_ip": "10.0.0.1",
    "alert": {
        "signature": "ET SCAN Nmap SYN Scan",
        "severity": 2
    }
}
```

Ce format est parfait pour être ingéré par un SIEM (Wazuh, ELK, Splunk...)🧠

### Question 🤔

_D'après vous, on démarre Suricata en mode IDS ou IPS sur un serveur de production ?_

**Réponse** : toujours en **IDS d'abord !** ⚠️

_On calibre les règles, on élimine les faux positifs et ENSUITE on passe en IPS. Sinon on risque de bloquer du trafic légitime !_

## Wazuh 🦉

_La plateforme HIDS / SIEM open-source_

### C'est quoi Wazuh ?
* Un **HIDS** (détection sur les machines) + SIEM intégré
* Open-source, fork d'**OSSEC** (le HIDS historique)
* Dashboard web complet (basé sur OpenSearch)

_**Analogie** : Wazuh c'est le médecin qui ausculte chaque patient (machine) et centralise tous les dossiers médicaux_ 🏥

### Pourquoi Wazuh ? 💡

* 🆓 **Gratuit et open-source** (vs CrowdStrike, SentinelOne ... )
* 🔍 **HIDS** : surveillance des fichiers, logs, processus
* 🧠 **SIEM** : corrélation des évènements, tableaux de bord
* 🤖 **Active Response** : réaction automatique aux menaces
* 📋 **Compliance** : PCI DSS, RGPD, HIPAA (rapports intégrés)

_Un seul outil pour faire HIDS + SIEM + compliance = rare et précieux !_

### L'architecture Wazuh

```apache
┌────────────┐  ┌────────────┐  ┌────────────┐
│ Agent 🐧   │  │ Agent 🪟  │  │ Agent 💻   │
│ (Linux)    │  │ (Windows)  │  │ (macOS)    │
└─────┬──────┘  └─────┬──────┘  └─────┬──────┘
      │               │               │
      └───────────┬───┘───────────────┘
                  │
          ┌───────▼───────┐
          │ Wazuh Manager │ (analyse + corrélation)
          └───────┬───────┘
                  │
          ┌───────▼───────┐
          │ Indexer       │ (OpenSearch)
          └───────┬───────┘
                  │
          ┌───────▼───────┐
          │ Dashboard     │ (interface web)
          └───────┬───────┘
```

## Les 3 composants

* **Wazuh Agent️** :shipit: : installé sur chaque machine, collecte les données
* **Wazuh Manager** 🧠 : reçoit, analyse, corrèle, déclenche les alertes
* **Wazuh Dashboard** 📊 : interface web pour visualiser et investiguer

_L'indexer (OpenSearch)stocke et indexe les données pour permettre la recherche rapide_

### Ce que surveille l'agent Wazuh 👁️

* 📁 **File Integrity Monitoring (FIM)** : détecte les modifications de fichiers critiques
* 📋 **Log collection** : collecte et analyse les logs système
* ⚙️ **System inventory** : packages installés, ports ouverts, processus
* 🔍 **Vulnerability detection** : détecte les CVE sur les packages installés
* 🛡️ **Security Configuration Assessment (SCA)** : vérifie les bonnes pratiques

### File Integrity Monitoring (FIM) 📁

_Quelqu'un a modifié / etc/passwd à 3h du matin ?_ 😱

Le FIM surveille les fichiers sensibles et alerte en cas de modification

Exemples de fichiers surveillés :

* /etc/passwd, /etc/shadow
* /ptc/ssh/sshd_config
* /var/www/ (détection de webshell !)

_Si un attaquant dépose un fichier dans /var/www → le FIM le voit immédiatement_ 🚨

### Active Response 🤖

Wazuh peut **réagir automatiquement** aux menaces détectées

* 🚫 Bloquer une IP (via iptables/firewalld)
* 🔒 Désactiver un compte utilisateur
* 📧 Envoyer une notification
* ⚙️ Exécuter un script personnalisé

_C'est comme un fail2ban... mais en beaucoup plus puissant et centralisé !_

### Les règles Wazuh 📋

Wazuh utilise des règles XML pour détecter les menaces :

```apache
<rule id="100001" level="10">
    <if_sid>5710</if_sid>
    <match>Failed password</match>
    <description>Brute-force SSH détecté</description>
    <group>authentication_failure</group>
</rule>
```
* **id** : identifiant unique
* **level** : sévérité (0-15)
* **match** : pattern dans les logs

_+4000 règles prédéfinies couvrant Linux, Windows, Apache, SSH, etc._

### Question 🤔

_C'est quoi la différence entre Wazuh et un SIEM comme Splunk ?_

Wazuh **fait les deux** : HIDS (agents sur les machines) + SIEM (corrélation centralisée)

_Splunk est un pur SIEM (il ingère des logs mais ne met pas d'agents). Wazuh est un HIDS
qui embarque aussi un SIEM. La force de Wazuh = les agents !_

### Suricata + Wazuh = combo gagnant 🏆

_NIDS + HIDS = couverture complète_

### Pourquoi les combiner ?

* 🌐 **Suricata** voit le **réseau** (paquets, connexions, protocoles)
* 💻 **Wazuh** voit les **machines** (fichiers, logs, processus)

Ensemble, on a une vision **360°** de la sécurité 🔄

_Suricata génère des alertes en EVE JSON_

### Comment ça s'intègre ?

1. **Suricata** génère des alertes en **EVE JSON**

2. **L'agent Wazuh** (sur la même machine) collecte ces logs

3. Le **Wazuh Manager** corrèle les alertes Suricata avec les autres Sources

4. Le **Dashboard** affiche tout dans une vue unifiée 📊

_Résultat : les alertes réseau ET système dans un seul tableau de bord !_

### Scénario d'attaque détectée 🚨

Un attaquant scanne votre réseau ...

1.	**Suricata** détecte le scan (regle ET SCAN) 🌐

2.	 L'attaquant exploite une faille web → upload d'un webshell

3.	**Wazuh FIM** détecte le nouveau fichier dans /var/www/ 📁

4.	**Wazuh Active Response** bloque l'IP de l'attaquant 🚫

5.	Le **SOC** voit toute la chaîne dans le dashboard et lance l'investigation 🔍

### Ce que voit le SOC dans le dashboard

* 🌐 Alertes réseau Suricata (scans, exploits, C2 ... )
* 📁 Alertes FIM (fichiers modifies/crees)
* 🔑 Alertes authentification (brute-force, login suspects)
* 🐛 Vulnérabilités detectees sur les machines
* 📋 Statut de conformité (PCI DSS, RGPD ... )

**Tout au même endroit** → c'est ça la force du combo !💪

## En production, les bonnes pratiques ⚙️

### Déploiement Suricata

* ⚠️ Commencer en mode **IDS** (jamais IPS direct !)
* 📝 Utiliser suricata- update Dour les règles ET Open
* 🔇 Calibrer : désactiver les règles trop bruyantes (faux positifs)
* 📊 Envoyer les logs EVE JSON vers Wazuh / SIEM

_Un IDS mal calibré = des milliers d'alertes inutiles = personne ne regarde = inutile_

### Déploiement Wazuh
* 📦 Installer le **Manager** sur un serveur dédié
* ️:shipit: Déployer les **agents** sur toutes les machines à surveiller
* 📁 Configurer le **FIM** sur les répertoires sensibles
* 🤖 Activer l'**Active Response** avec prudence
* 📊 Personnaliser le **Dashboard** selon vos besoins

_Conseil : testez l'Active Response en environnement de test avant la prod !_ ⚠️

### Les pièges à éviter 😬

* 🚫 Passer en IPS sans calibrage - on bloque du trafic légitime
* 🚫 Trop de règles activées - alert fatigue
* 🚫 Pas de monitoring des IDS eux-mêmes (qui surveille le surveillant ?)
* 🚫 Oublier les mises à jour des règles

_Un IDS qu'on ne regarde jamais, c'est comme une alarme incendie dont on a retiré les piles_ 🔋

### Récap' : qui fait quoi ? 📋

* 🌐 **Suricata** = NIDS/NIPS (surveille le réseau)
* 💻 **Wazuh Agent** = HIDS (surveille les machines)
* 🧠 **Wazuh Manager** = correlation + alertes
* 📊 **Wazuh Dashboard** = visualisation + investigation
* 🤖 **Active Response** = réaction automatique

### Le schéma complet
```apache
Internet
    │
┌───▼───┐
│Firewall│ (pfSense, iptables...)
└───┬───┘
    │
┌───▼──────────────┐
│ Suricata (NIDS)  │ → alertes réseau
└───┬──────────────┘
    │
┌───▼──────────────┐
│ Serveurs / Postes│
│ (Wazuh Agent)    │ → alertes système
└───┬──────────────┘
    │
┌───▼──────────────┐
│   Wasuh Manager  │ → corrélation
│   + Dashboard    │ → visualisation
└───┬──────────────┘
```

## Rappel des acronymes 📖

* **IDS** : Intrusion Detection System
* **IPS** : Intrusion Prevention System
* **NIDS** : Network IDS
* **HIDS** : Host IDS
* **FIM** : File Integrity Monitoring
* **SCA** : Security Configuration Assessment
* **EVE** : Extensible Event Format (logs Suricata)

Pas de challenge prévu pour cette session.

#

