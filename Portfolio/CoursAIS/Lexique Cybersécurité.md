# 📖​ Lexique TSSR / AIS / Cybersécurité

## Objectif

Ce lexique regroupe les principaux termes techniques à maîtriser pour une soutenance TSSR/AIS, un entretien technique, ou une activité d’administration systèmes, réseaux et cybersécurité.

Il couvre :

* systèmes et réseaux
* Windows Server
* Linux
* virtualisation
* cloud
* supervision
* sécurité systèmes & réseaux
* conteneurs & orchestration
* pentesting
* gestion de projets
* centres de services
* scripting

# 1. Fondamentaux IT & Réseau

## Adresse IP

Identifiant numérique permettant à un équipement de communiquer sur un réseau.

## IPv4

Version 4 du protocole IP utilisant des adresses sur 32 bits.

## IPv6

Version moderne d’IP utilisant des adresses sur 128 bits.

## Masque de sous-réseau

Valeur permettant de distinguer la partie réseau et la partie hôte d’une adresse IP.

## Passerelle (Gateway)

Équipement permettant à un réseau d’accéder à d’autres réseaux.

## DNS (Domain Name System)

Service traduisant des noms de domaine en adresses IP.

## DHCP

Service attribuant automatiquement une configuration réseau aux machines.

## NAT

Technique traduisant des IP privées en IP publiques.

## VLAN

Segmentation logique d’un réseau permettant d’isoler des flux.

## Trunk

Lien transportant plusieurs VLANs.

## ACL

Liste de règles permettant d’autoriser ou bloquer du trafic réseau.

## Routage

Mécanisme permettant aux paquets IP de circuler entre réseaux.

## Switch

Équipement réseau connectant plusieurs machines sur un LAN.

## Routeur

Équipement réseau reliant plusieurs réseaux.

## Pare-feu

Système filtrant les communications réseau selon des règles.

## Proxy

Serveur intermédiaire entre un client et Internet.

## Reverse Proxy

Serveur recevant les requêtes clients puis les redirigeant vers des serveurs internes.

## DMZ

Zone réseau intermédiaire isolant les services exposés d’Internet.

## VPN

Tunnel chiffré permettant des communications sécurisées.

## VPN Site-à-site

VPN reliant deux réseaux distants.

## VPN Client-à-site

VPN permettant à un utilisateur distant d’accéder à un réseau interne.

## QoS

Mécanisme priorisant certains flux réseau.

## MTU

Taille maximale d’un paquet transmissible sur un réseau.

## ICMP

Protocole réseau utilisé notamment pour le diagnostic réseau (ping).

## TCP

Protocole orienté connexion garantissant la livraison des données.

## UDP

Protocole rapide sans garantie de livraison.

## Port réseau

Point logique utilisé par un service réseau pour communiquer.

## Socket

Association entre une adresse IP et un port.

## CIDR

Notation compacte utilisée pour représenter un réseau IP.

---

# 2. Windows Server & Active Directory

## Active Directory (AD)

Service Microsoft centralisant authentification, utilisateurs et ressources.

## Domaine

Ensemble logique d’objets gérés par Active Directory.

## Contrôleur de domaine (DC)

Serveur hébergeant Active Directory.

## OU (Organizational Unit)

Conteneur logique permettant d’organiser les objets AD.

## GPO

Objet de stratégie de groupe permettant d’appliquer des configurations automatiquement.

## LDAP

Protocole d’accès à l’annuaire Active Directory.

## LDAPS

Version chiffrée de LDAP utilisant TLS.

## Kerberos

Protocole principal d’authentification d’Active Directory.

## NTLM

Ancien protocole d’authentification Windows.

## DNS intégré AD

DNS utilisé par Active Directory pour localiser les services du domaine.

## FSMO

Rôles maîtres spécialisés dans Active Directory.

## SYSVOL

Partage contenant scripts et GPO du domaine.

## PowerShell

Shell d’administration orienté objets de Microsoft.

## WinRM

Service permettant l’administration distante via PowerShell.

## RDP

Protocole d’accès distant aux environnements Windows.

## WSUS

Serveur de gestion des mises à jour Windows.

## Domaine de confiance

Relation de confiance entre plusieurs domaines ou forêts.

## Forêt Active Directory

Ensemble de domaines partageant un schéma commun.

## Compte de service

Compte utilisé par une application ou un service.

## Domain Admin

Compte disposant des privilèges administrateur du domaine.

## SID

Identifiant unique attribué aux utilisateurs et groupes Windows.

## NTDS.dit

Base de données principale d’Active Directory.

---

# 3. Linux

## Linux

Système d’exploitation basé sur le noyau Linux.

## Distribution Linux

Version de Linux intégrant noyau, paquets et outils.

## Debian

Distribution Linux réputée pour sa stabilité.

## Ubuntu

Distribution basée sur Debian orientée simplicité.

## Kernel

Noyau du système d’exploitation.

## Shell

Interface en ligne de commande.

## Bash

Shell Linux très répandu.

## Processus

Programme en cours d’exécution.

## PID

Identifiant unique d’un processus.

## Daemon

Service fonctionnant en arrière-plan.

## systemd

Système d’initialisation moderne de Linux.

## Journalctl

Commande de consultation des logs systemd.

## Permissions Linux

Droits d’accès lecture, écriture et exécution.

## chmod

Commande modifiant les permissions.

## chown

Commande modifiant le propriétaire d’un fichier.

## sudo

Commande permettant d’exécuter des actions administrateur.

## Root

Compte administrateur principal sous Linux.

## SSH

Protocole d’administration distante sécurisé.

## SCP

Copie sécurisée de fichiers via SSH.

## Rsync

Outil de synchronisation de fichiers.

## Cron

Planificateur de tâches Linux.

## PAM

Framework d’authentification Linux.

## SELinux

Mécanisme de contrôle d’accès renforcé sous Linux.

## AppArmor

Mécanisme de confinement d’applications Linux.

## Netfilter

Moteur de pare-feu intégré au noyau Linux.

## iptables

Outil historique de configuration du pare-feu Linux.

## nftables

Successeur moderne d’iptables.

## Fail2ban

Outil bannissant automatiquement des IP malveillantes.

---

# 4. Virtualisation

## Virtualisation

Technique permettant d’exécuter plusieurs systèmes sur un même matériel physique.

## Hyperviseur

Logiciel permettant de gérer des machines virtuelles.

## Hyperviseur Type 1

Hyperviseur fonctionnant directement sur le matériel.

## Hyperviseur Type 2

Hyperviseur fonctionnant au-dessus d’un système hôte.

## Machine virtuelle (VM)

Système virtualisé isolé.

## Snapshot

Capture de l’état d’une VM à un instant donné.

## Clone

Copie d’une machine virtuelle.

## Proxmox

Plateforme de virtualisation basée sur KVM et LXC.

## VMware Workstation

Hyperviseur desktop de VMware.

## KVM

Hyperviseur intégré au noyau Linux.

## ESXi

Hyperviseur bare-metal de VMware.

## vCPU

Processeur virtuel attribué à une VM.

## Ballooning

Technique d’optimisation mémoire en virtualisation.

## Bridge réseau

Pont réseau reliant VM et réseau physique.

## NAT virtualisé

Accès Internet des VM via traduction d’adresse.

## LXC

Technologie de conteneurisation système Linux.

## Namespace

Mécanisme d’isolation Linux.

## Cgroups

Mécanisme limitant les ressources d’un processus ou conteneur.

---

# 5. Cloud Computing

## Cloud Computing

Accès à des ressources informatiques à la demande via Internet.

## Cloud public

Infrastructure mutualisée gérée par un provider.

## Cloud privé

Infrastructure dédiée à une seule organisation.

## Cloud hybride

Architecture combinant cloud privé et cloud public.

## IaaS

Modèle cloud fournissant infrastructure et virtualisation.

## PaaS

Modèle cloud fournissant plateforme et environnement d’exécution.

## SaaS

Logiciel accessible directement via Internet.

## Scalabilité

Capacité d’augmenter ou réduire les ressources.

## Élasticité

Adaptation automatique des ressources selon la charge.

## Haute disponibilité

Architecture garantissant la continuité de service.

## SLA

Contrat définissant le niveau de disponibilité garanti.

## Multi-tenant

Architecture où plusieurs clients partagent une infrastructure.

## OpenStack

Plateforme open-source de cloud privé.

## Nova

Service OpenStack de gestion des VM.

## Neutron

Service OpenStack de gestion réseau.

## Keystone

Service OpenStack d’authentification.

## Swift

Service OpenStack de stockage objet.

## Cinder

Service OpenStack de stockage bloc.

## Terraform

Outil d’Infrastructure as Code.

## Infrastructure as Code (IaC)

Gestion d’infrastructure via du code automatisé.

---

# 6. Supervision & Monitoring

## Supervision

Surveillance des systèmes, services et réseaux.

## Monitoring

Collecte et analyse d’informations techniques.

## Zabbix

Plateforme de supervision open-source.

## Agent de supervision

Logiciel installé sur une machine pour remonter des informations.

## SNMP

Protocole de supervision réseau.

## OID

Identifiant d’objet SNMP.

## Trap SNMP

Notification envoyée automatiquement lors d’un événement.

## Dashboard

Interface de visualisation des métriques.

## Alerte

Notification déclenchée lorsqu’un seuil est dépassé.

## SLA Monitoring

Surveillance des engagements de disponibilité.

## Logs

Fichiers enregistrant les événements système.

## Syslog

Standard de centralisation des logs.

## Grafana

Outil de visualisation de métriques.

## Prometheus

Système de collecte de métriques orienté time-series.

## ELK Stack

Suite Elasticsearch + Logstash + Kibana.

## Disponibilité

Temps pendant lequel un service reste opérationnel.

---

# 7. Sécurité Systèmes & Réseaux

## CIA

Modèle sécurité basé sur Confidentialité, Intégrité et Disponibilité.

## Hardening

Durcissement d’un système pour réduire la surface d’attaque.

## Surface d’attaque

Ensemble des points exploitables d’un système.

## Segmentation réseau

Isolation logique ou physique des réseaux.

## IDS

Système détectant les intrusions.

## IPS

Système détectant et bloquant les intrusions.

## SIEM

Plateforme centralisant et analysant les logs de sécurité.

## WAF

Pare-feu protégeant les applications web.

## Suricata

IDS/IPS open-source.

## Wazuh

Plateforme open-source de supervision sécurité.

## IAM

Gestion des identités et des accès.

## MFA

Authentification multi-facteurs.

## SSO

Authentification unique multi-applications.

## OAuth 2.0

Framework d’autorisation basé sur des tokens.

## OpenID Connect

Couche d’authentification basée sur OAuth2.

## JWT

Token signé contenant des informations d’identité.

## TLS

Protocole sécurisant les communications réseau.

## Certificat TLS

Document numérique permettant d’authentifier un service.

## PKI

Infrastructure de gestion des certificats numériques.

## Hachage

Transformation irréversible d’une donnée.

## Chiffrement symétrique

Chiffrement utilisant la même clé pour chiffrer et déchiffrer.

## Chiffrement asymétrique

Chiffrement utilisant une clé publique et une clé privée.

## Zero Trust

Modèle de sécurité où aucun accès n’est implicitement approuvé.

---

# 8. Conteneurs & Orchestration

## Conteneurisation

Exécution d’applications dans des environnements isolés.

## Docker

Plateforme de gestion de conteneurs.

## Image Docker

Modèle servant à créer des conteneurs.

## Conteneur Docker

Instance en cours d’exécution d’une image.

## Dockerfile

Fichier décrivant la construction d’une image Docker.

## Docker Compose

Outil décrivant des architectures multi-conteneurs.

## Volume Docker

Stockage persistant pour conteneurs.

## Registry

Dépôt d’images Docker.

## DockerHub

Registry public d’images Docker.

## Orchestration

Gestion automatisée de conteneurs sur plusieurs machines.

## Docker Swarm

Solution d’orchestration intégrée à Docker.

## Kubernetes

Plateforme d’orchestration de conteneurs.

## Pod

Plus petite unité déployable Kubernetes.

## Cluster Kubernetes

Ensemble de nœuds exécutant Kubernetes.

## Node

Machine participant à un cluster Kubernetes.

## Deployment

Objet Kubernetes gérant des réplicas d’applications.

## ReplicaSet

Objet assurant le nombre voulu de pods.

## Service Kubernetes

Objet exposant des applications réseau.

## Ingress

Mécanisme d’exposition HTTP/HTTPS dans Kubernetes.

## Helm

Gestionnaire de paquets Kubernetes.

---

# 9. Pentesting & Cybersécurité offensive

## Pentest

Test d’intrusion réalisé avec autorisation.

## Reconnaissance

Collecte d’informations sur une cible.

## Énumération

Identification détaillée des services et utilisateurs.

## Exploitation

Utilisation d’une vulnérabilité pour démontrer un impact.

## Post-exploitation

Analyse des impacts après compromission.

## Mouvement latéral

Déplacement d’un attaquant entre plusieurs systèmes.

## Escalade de privilèges

Obtention de droits plus élevés.

## Persistence

Techniques permettant de conserver un accès.

## OSINT

Collecte d’informations depuis des sources publiques.

## Nmap

Scanner réseau et de services.

## Wireshark

Analyseur de paquets réseau.

## Burp Suite

Proxy d’analyse des applications web.

## Metasploit

Framework d’exploitation de vulnérabilités.

## SQLMap

Outil d’exploitation d’injections SQL.

## BloodHound

Outil d’analyse de chemins d’escalade Active Directory.

## PingCastle

Outil d’audit sécurité Active Directory.

## Mimikatz

Outil d’extraction de mots de passe et tickets Kerberos.

## XSS

Injection de JavaScript dans une page web.

## SQL Injection

Injection de requêtes SQL malveillantes.

## CSRF

Attaque forçant un utilisateur authentifié à exécuter une action.

## SSRF

Faille forçant un serveur à envoyer des requêtes internes.

## LFI

Inclusion de fichiers locaux.

## RFI

Inclusion de fichiers distants.

## RCE

Exécution de code arbitraire à distance.

## EternalBlue

Exploit SMBv1 permettant une exécution de code à distance.

## WannaCry

Ransomware exploitant EternalBlue.

## Evil Twin

Faux point d’accès Wi-Fi imitant un réseau légitime.

## Kerberoasting

Attaque ciblant les comptes de service Kerberos.

## Pass-the-Hash

Utilisation d’un hash NTLM pour s’authentifier.

## Golden Ticket

Faux ticket Kerberos donnant un accès quasi illimité.

## DoS

Attaque visant à rendre un service indisponible.

## DDoS

Déni de service distribué utilisant plusieurs machines.

## SYN Flood

Attaque saturant les connexions TCP.

## Rate Limiting

Limitation du nombre de requêtes.

---

# 10. Scripting & Automatisation

## Script

Suite de commandes automatisant des tâches.

## Bash Script

Script shell Linux.

## PowerShell Script

Script PowerShell Windows.

## Python

Langage très utilisé en automatisation et cybersécurité.

## Variable

Valeur stockée et réutilisable dans un script.

## Fonction

Bloc de code réutilisable.

## Boucle

Instruction répétant des actions.

## Condition

Instruction exécutée selon un résultat logique.

## Regex

Expression régulière utilisée pour rechercher des motifs.

## API

Interface permettant à des applications de communiquer.

## JSON

Format de données structuré très utilisé par les APIs.

## YAML

Format lisible utilisé notamment dans Docker et Kubernetes.

## Git

Système de gestion de versions.

## GitHub

Plateforme d’hébergement Git.

## CI/CD

Automatisation des tests, builds et déploiements.

---

# 11. Gestion de projets & ITIL

## ITIL

Référentiel de bonnes pratiques de gestion des services IT.

## Incident

Interruption ou dégradation non prévue d’un service.

## Problème

Cause racine d’un ou plusieurs incidents.

## Changement

Modification contrôlée d’une infrastructure.

## SLA

Engagement contractuel sur la qualité de service.

## Centre de services

Point de contact entre utilisateurs et équipe IT.

## Ticketing

Système de gestion des demandes et incidents.

## GLPI

Outil ITSM et de gestion de parc informatique.

## CMDB

Base recensant les équipements et relations d’infrastructure.

## Escalade

Transmission d’un ticket à un niveau supérieur.

## KPI

Indicateur de performance.

## Gestion des actifs

Suivi du cycle de vie des équipements.

## CAB

Comité validant les changements importants.

## RCA (Root Cause Analysis)

Analyse des causes racines d’un incident.

---

Ce lexique peut encore évoluer avec :

* SOC & Blue Team
* Red Team
* DevSecOps
* sécurité Cloud avancée
* PKI avancée
* forensic
* malware analysis
* Kubernetes avancé
* sécurité Wi-Fi avancée
* Active Directory avancé

---
