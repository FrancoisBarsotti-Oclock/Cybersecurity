# Challenge - Mise en place d'une supervision réseau

## Contexte professionnel

Vous venez d'être recruté(e) en tant qu'administrateur système junior chez **TechSecure**, une PME spécialisée dans l'hébergement d'applications web pour ses clients. 

Votre responsable, inquiet suite à plusieurs incidents non détectés la semaine dernière (serveur web inaccessible pendant 2h, saturation disque sur un serveur de base de données), vous confie une mission critique : **mettre en place les premières briques d'une infrastructure de supervision**.

L'objectif est double :
- Pouvoir interroger les équipements réseau via SNMP pour récupérer des informations vitales
- Déployer un outil de supervision centralisé capable de surveiller l'ensemble du parc informatique

## Objectifs du challenge

À l'issue de ce challenge, vous serez capable de :
- Configurer et interroger des équipements via le protocole SNMP
- Installer et configurer Zabbix comme solution de supervision
- Comprendre la différence entre supervision active et passive
- Identifier les métriques essentielles à surveiller

---

## ÉTAPE 1 - Configuration SNMP

### Objectif
Configurer un équipement réseau pour qu'il puisse être interrogé via SNMP.

### Travail à réaliser

**1.1** - Ouvrez Packet Tracer et créez une topologie simple avec un routeur

**1.2** - Sur ce routeur, activez SNMP v2c avec les paramètres suivants :
- Community (communauté) en lecture seule : `techsecure_ro`
- Location : `DataCenter-Paris`
- Contact : `admin@techsecure.fr`

**1.3** - Vérifiez que la configuration SNMP est bien active sur le routeur

---

## ÉTAPE 2 - Interrogation SNMP

### Objectif
Utiliser les outils SNMP en ligne de commande pour récupérer des informations depuis l'équipement réseau.

### Travail à réaliser

**2.1** - Sur votre machine Linux, installez les outils SNMP :
```bash
sudo apt update
sudo apt install snmp snmp-mibs-downloader
```

**2.2** - Testez la connectivité SNMP avec votre routeur (remplacez `IP_ROUTEUR` par l'IP de votre routeur) :
```bash
snmpwalk -v2c -c techsecure_ro IP_ROUTEUR system
```

**2.3** - Récupérez les informations suivantes et notez les résultats :
- Le nom du système (sysName)
- La description du système (sysDescr)
- La liste des interfaces réseau

**2.4** - Répondez à la question : *Pourquoi SNMP v2c est-il considéré comme peu sécurisé ? Quelle amélioration apporte SNMPv3 ?*

---

## ÉTAPE 3 - Préparation de l'environnement Zabbix

### Objectif
Préparer la VM Debian pour l'installation de Zabbix.

### Travail à réaliser

**3.1** - Assurez-vous que votre VM Debian est à jour :
```bash
sudo apt update
sudo apt upgrade -y
```

**3.2** - Installez les prérequis de base :
```bash
sudo apt install wget curl gnupg2 -y
```

**3.3** - Vérifiez que votre VM dispose d'au moins 2 Go de RAM :
```bash
free -h
```

---

## ÉTAPE 4 - Installation de Zabbix Server

### Objectif
Installer et configurer le serveur Zabbix avec sa base de données.

### Travail à réaliser

**4.1** - Ajoutez le dépôt officiel Zabbix pour Debian :
```bash
wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb
dpkg -i zabbix-release_latest_7.4+debian13_all.deb
apt update
```

**4.2** - Installez Zabbix server, frontend et agent :
```bash
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
```

**4.3** - Installez et configurez MySQL/MariaDB :
```bash
sudo apt install mariadb-server -y
sudo mariadb-secure-installation
```

**4.4** - Créez la base de données Zabbix :
```bash
sudo mysql -u root -p
```
Puis exécutez les commandes SQL suivantes :
```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'MotDePasseSecurise';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
SET GLOBAL log_bin_trust_function_creators = 1;
QUIT;
```

**4.5** - Importez le schéma initial de la base de données :
```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

**4.6** - Configurez le fichier de configuration Zabbix :
```bash
sudo nano /etc/zabbix/zabbix_server.conf
```
Modifiez la ligne suivante (décommentez et ajoutez votre mot de passe) :
```
DBPassword=MotDePasseSecurise
```

**4.7** - Démarrez les services Zabbix :
```bash
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```

**4.8** - Vérifiez que les services sont actifs :
```bash
sudo systemctl status zabbix-server
```

---

## ÉTAPE 5 - Configuration du frontend Zabbix

### Objectif
Finaliser l'installation via l'interface web et se connecter à Zabbix.

### Travail à réaliser

**5.1** - Accédez à l'interface web de Zabbix :
```
http://adresse_ip_de_votre_vm/zabbix
```

**5.2** - Suivez l'assistant de configuration :
- Vérifiez les prérequis PHP (tous doivent être OK)
- Configurez la connexion à la base de données avec les identifiants créés précédemment
- Définissez le nom du serveur Zabbix (ex: "TechSecure-Monitor")
- Validez la configuration

**5.3** - Connectez-vous avec les identifiants par défaut :
- Utilisateur : `Admin`
- Mot de passe : `zabbix`

**5.4** - **IMPORTANT** : Changez immédiatement le mot de passe admin :
- Allez dans *User settings* → *Profile*
- Changez le mot de passe

**5.5** - Explorez l'interface :
- Allez dans *Monitoring* → *Hosts*
- Vérifiez que "Zabbix server" apparaît avec un statut "Enabled"
- Cliquez sur "Zabbix server" puis sur *Latest data*
- Identifiez 3 métriques collectées (CPU, mémoire, disque, etc.)

---

## ÉTAPE 6 - Synthèse et réflexion

### Objectif
Consolider vos apprentissages et anticiper les prochaines étapes.

### Travail à réaliser

**6.1** - Répondez aux questions suivantes (3-4 lignes maximum par question) :

**Question 1 : Supervision vs Monitoring**
Expliquez avec vos propres mots la différence entre supervision et monitoring. Donnez un exemple concret pour illustrer.

**Question 2 : Méthodes de surveillance**
L'agent Zabbix installé sur le serveur utilise-t-il une méthode de supervision active ou passive ? Quel est l'avantage de cette approche ?

**Question 3 : Mise en production**
Citez 2 actions indispensables pour rendre cette infrastructure de supervision vraiment utile en production.

**6.2** - **BONUS** (si vous avez terminé en avance) :
- Dans l'interface Zabbix, explorez la section *Monitoring* → *Latest data* → sélectionnez "Zabbix server"
- Cherchez où voir les graphiques d'une métrique
- Notez ce qu'est un "template" Zabbix (indice : section *Data collection* → *Templates*)

---

## Ressources utiles

- **Documentation Zabbix** : https://www.zabbix.com/documentation/current/
- **Guide d'installation Debian** : https://www.zabbix.com/download?zabbix=7.4&os_distribution=debian&os_version=13&components=server_frontend_agent&db=mysql&ws=apache
- **Commandes SNMP** : `snmpwalk`, `snmpget`, `snmpstatus`
- **RFC SNMP** : RFC 1157 (SNMPv1), RFC 3416 (SNMPv2c)

---
