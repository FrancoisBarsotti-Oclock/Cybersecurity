# 🏆 Challenge B301 (CP7): Supervision réseau.
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous venez d'être recruté(e) en tant qu'administrateur système junior chez TechSecure, une PME spécialisée dans l'hébergement d'applications web pour ses clients.
>
>Votre responsable, inquiet suite à plusieurs incidents non détectés la semaine dernière (serveur web inaccessible pendant 2h, saturation disque sur un serveur de base de données), vous confie une mission critique : **mettre en place les premières briques d'une infrastructure de supervision**.
>
>L'objectif est double :
>
>* Pouvoir interroger les équipements réseau via SNMP pour récupérer des informations vitales
>* Déployer un outil de supervision centralisé capable de surveiller l'ensemble du parc informatique
>

👉 [Énoncé complet du challenge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Supervision/EnoncEs%20de%20challenges%20B3/Pour%20B301_Supervision%20r%C3%A9seau.md)

Voir 👉 [Cours B301](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B3%20(Supervision)/B301_Supervision%20%26%20Monitoring.md)

## Étape 1: Architecture

Dans un environnement virtuel Proxmox avec :

| **Élement** | **OS** | **IP** | **Rôle** |
| :--: | :--: | :--: | :--: |
| **Zabbix** | Debian 13.1.2 | `10.0.0.100/16` | Serveur Zabbix |
| -- | -- | -- | -- |

## Étape 2: sudo & préparation pour Zabbix
 
 Important pour ne pas avoir à administrer la VM en tant que `root`

 ```nginx
adduser francois
apt update
apt install sudo -y
usermod -aG sudo <nom_utilisateur>
su - <nom_utilisateur>
```

Mise à jour de la VM pour préparer l'environnement Zabbix

 ```nginx
# maj de la Debian
sudo apt update
sudo apt upgrade -y

# Installation des prérequis de base
sudo apt install wget curl gnupg2 -y

# Vérification de la RAM
free -h
 ```

## Étape 3: Installation de Zabbix Server

Pour l'installation et configuration du serveur Zabbix avec sa base de données, on se réfère à la [documentation officielle](https://www.zabbix.com/fr/integrations) et chercher la configuration souhaitée selon plateforme: `Get Zabbix > Packages Zabbix` → J’ai favorisé Zabbix 7.4 car la version 8.0 est toujours en beta.

![01-Choix plateforme Zabbix](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_01-Choix%20plateforme%20Zabbix.png)





### 🚧 En construction 🚧

---

### 📚 Ressources & liens utiles:

* [documentation officielle Zabbix](https://www.zabbix.com/fr/integrations) 
