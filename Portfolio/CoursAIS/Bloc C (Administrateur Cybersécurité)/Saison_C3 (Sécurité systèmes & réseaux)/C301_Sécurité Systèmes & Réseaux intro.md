# 🔒 Session C301. Introduction à la Sécurité des Systèmes & Réseaux.

### Pourquoi ce module ?
On passe de « ça marche à « c’est sécurisé », même si la sécurité parfaite n’existe pas.
imaginez votre infra comme une maison
* Jusqu'ici, on a construit les murs et pose les portes
* Mais ... est-ce qu'on a mis des serrures ?
* Est-ce qu'on sait qui a les clés ?
Et si quelqu’un entre par la fenêtre ?

### Objectifs de cette session

À la fin de cette partie, vous saurez :
•	🧱Sécuriser un réseau et une infra simple
•	️️🛡️Déployer des briques de défense (pare-feu, VPN, IDS/IPS, SIEM)
•	🐧Durcir un serveur Linux
•	🔍Diagnostiquer une faille "terrain" et la corriger
•	💬Expliquer et justifier vos choix de sécurité

## 📋 Le Programme

### Épisode 1 ️🛡️

Intro, gouvernance, SOC/SIEM, attaques L2
Poser les bases: pourquoi et comment on organise la sécurité

### Épisode 2 🔐

ACL, 802.1X, RADIUS, sécurité WiFi
Contrôler qui accède à quoi sur le réseau

### Épisode 3 🧱

DMZ, pare-feu, VPN
Segmenter, filtrer, chiffrer : les 3 piliers du réseau sécurisé

### Épisode 4 🔍

Atelier IDS/IPS & SIEM
On met les mains dans Suricata et Wazuh !

### Épisode 5 🔍

Correction atelier, IDS/IPS + SIEM (suite)
On approfondit la détection et l'analyse

### Épisode 6 🐧

Sécurité Linux : pare-feu, SSH hardening
Le serveur Linux, on le blinde

### Épisode 7 🐧

Sécurité Linux : PAM, fail2ban, port-knocking
Encore plus de durcissement, encore moins de surface d'attaque

### Épisode 8 🌐

Sécurité applicative : reverse-proxy, HTTPS, load-balancing
Protéger ce qui est exposé au monde

### Épisode 9 🌐
SSO/IAM, WAF, CDN
Authentification centralisée et protection applicative

### Épisode 10 🎯

ECF + Bilan
On fait le point sur tout ce qu'on a appris!

---

### Les acronymes 🤯

* CIA, PDR — les fondamentaux
* CSIRT / CERT — les équipes de réponse
* SOC, SIEM, CTI, EDR, XDR — la surveillance
* ISO 27001, PCI DSS, HIPAA, RGPD — les normes
* IDS, IPS, WAF, CDN — les outils de défense

### Outils & environnement 🧰

* 🖧 Packet Tracer / Proxmox
* 🔥pfSense, Suricata, Wazuh
* ⚙️HAProxy, Nginx
* 🐧Un peu (beaucoup) de Linux en ligne de commande

# Gouvernance & normes 📜

Pourquoi on en parle avant la technique

La sécurité, c'est aussi des règles
Imaginez un chantier sans plan d'architecte️

Sans cadre, on se retrouve à :
•	sécuriser au hasard
•	ne pas savoir "jusqu'où" aller
•	ne pas pouvoir justifier ses choix

La gouvernance, c'est ce qui donne la direction

C'est quoi, concrètement ?
•	Définir qui décide
•	Définir quoi protéger en priorité
•	Définir comment on vérifie

Bref , un cadre pour que la technique ait du sens.
La triade CIA
Le socle de toute réflexion en sécurité

![01-La Triade CIA](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C3%20(S%C3%A9curit%C3%A9%20syst%C3%A8mes%20%26%20r%C3%A9seaux)/images%20C3/images%20C301/C301_01-La%20Triade%20CIA.png)

Confidentialité
Seules les personnes autorisées accèdent à l'information
•	Chiffrement des données
•	Contrôle d'accès (mots de passe, MFA ... )
•	Classification de l'information

Analogie : une lettre dans une enveloppe scellée
Intégrité
L'information n'est pas modifiée sans autorisation
•	Hachage (vérifier qu'un fichier n'a pas changé)
•	Signatures numériques
•	Contrôle de version
Disponibilité
L'information est accessible quand on en a besoin
•	Redondance, haute disponibilité
•	Sauvegardes régulières
•	Plan de reprise d'activité

Analogie : un hôpital qui ne peut jamais fermer
Question interactive
Un ransomware chiffre tous vos fichiers et demande une rançon ...

Ça touche quel(s) pilier(s) de la CIA ?
•	Confidentialité ? Possible (exfiltration)
•	Intégrité ? Oui (données chiffrées/altérées)
•	Disponibilité ? Oui ! (plus d'accès aux données)

Réponse : potentiellement les 3 ! C'est pour ça que c'est si redouté

## PDR : Plan de Reprise 🔄
On ne vise pas le zéro incident. On vise :
•	Repartir vite
•	Repartir propre
•	Limiter la casse

Les questions à se poser
•	Que fait-on si tout tombe ?💥
•	Qui décide quoi ?
•	En combien de temps on repart ?

Si vous ne pouvez pas répondre à ces question aujourd’hui, c’est que vous avez besoin d’un PRA

### CSIRT / CERT 🚨
Les équipes de réponse aux incidents
À retenir
•	CERT : veille + réponse aux incidents (souvent national) Ex: CERT-FR de l'ANSSI
•	CSIRT : équipe interne ou dédiée à la gestion d'incidents. Ex: le CSIRT de votre entreprise
Dans la vraie vie : on veut des procédures, pas juste des outils

### MITRE ATT&CK ⚔️

Une grille de lecture des attaques
•	Des tactiques : le "pourquoi" (ex: accès initial, persistance ... )
•	Des techniques : le "comment" (ex: phishing, brute-force ... )
•	Des procédures : le "qui l'a fait" (groupes d'attaquants connus)

À quoi ça sert ?
*	🔍Analyser un incident
*	🎯Couvrir ses angles morts
*	💬Parler un langage commun entre équipes

Lien de [MITRE ATT&CK](https://attack.mitre.org/)

## Les normes et standards 📚

### ISO 27001/ 27002
•	27001 : exigences pour un SMSI (Système de Management de la Sécurité de I'Information)
•	27002 : catalogue de mesures et bonnes pratiques

Objectif : organiser la sécu dans la durée

### PCI DSS 💳
Normes pour protéger les données de cartes bancaires
•	Segmentation réseau
•	Chiffrement
•	Contrôle d'accès
•	Journalisation (logs)
Obligation légale pour tout commerçant qui traite des paiements par carte

### HIPAA 🏥
Cadre américain pour la donnée de santé
•	Confidentialité des dossiers patients
•	Traçabilité des accès
•	Contrôle d'accès strict

Même si on n'est pas aux US : la logique est universelle

### RGPD 
Le cadre européen pour les données personnelles
•	Minimisation des données collectées
•	Transparence envers les utilisateurs
•	Sécurité des traitements
•	Droits des personnes (accès, suppression ... )

En cas de non-respect ? Amendes jusqu’à 20M€ ou 4% du CA mondial💸

### Exemples concrets d'amendes CNIL 🔨
•	Google : 150 M€ (cookies, 2022)
•	Amazon : 746 M€ (ciblage publicitaire, 2021)
•	Meta : 1,2 Md€ (transferts de données, 2023)

La conformité, ce n’est pas juste de la paperasse.

En résumé : gouvernance = décisions + preuves

Ce qu'on doit être capable de dire :
*	"Voici nos risques"
*	"Voici nos priorités"
*	"Voici nos mesures"
* "Voici comment on vérifie"

### Ce que ça change pour nous, terrain 🔧
•	On justifie une règle de pare-feu
•	On priorise les patchs
•	On structure un plan d'action

Bref : la technique a un sens

# SOC, SIEM, CTI, EDR, XDR 🔍
La surveillance qui sert vraiment

Pourquoi on en parle ?
Car prévenir ne suffit pas

### La réalité 
* Un incident finira par arriver💥
* Il faut détecter vite⏱️
* Comprendre ce qui se passe🔍
* Réagir proprement️ 🛡️

La question n'est pas "si", mais "quand"

### Le scénario

Il est 3h du matin. Une alerte se déclenche dans le SOC🚨

Un analyste regarde : une connexion suspecte depuis la Russie vers un serveur interne ...

Le SIEM a corrélé l'alerte avec d'autres événements : des tentatives de connexion échouées, puis une réussie😱.

### La réaction

L'analyste vérifie dans le CTI : I'IP source est connue comme malveillante⚠️

L'EDR sur le poste compromis a détecte un processus suspect – il l'isole automatiquement🔒

Le SOC lance la remédiation : changement de mots de passe, analyse forensique, rapport d'incident📋

### La morale

Sans SOC -> personne ne regarde à 3h du matin😴
Sans SIEM -> pas de corrélation, pas d'alerte🤷
Sans EDR -> le malware continue tranquillement🦠

Chaque brique a son rôle !

## C'est quoi un SOC ?
Security Operations Center, le centre de pilotage

C'est une équipe + un process

Analogie : les pompiers du SI 🚒

### Les missions d'un SOC
* 📡Collecte des logs
* 🔔Détection d'alertes
*	🚨Réponse aux incidents
*	📈Amélioration continue

Le mot-clé : **routine + méthode**

→ On peut avoir entre **10 000 et 50 000** alertes/jour !🤯
D'où l'importance de bien **filtrer** et **prioriser**

L'ennemi n°1 du SOC : l'**alerte fatigue** (trop d'alertes = on ne regarde plus rien)

### Les niveaux d’un SOC
•	N1 : tri et qualification (le "premier regard")👀
•	N2 : investigation approfondie🔍
•	N3 : expertise, forensique, remédiation complexe🧪

Pas besoin d'être 20 : un petit SOC bien cadré = grosimpact !

## C'est quoi un SIEM ? 🧠
Security Information and Event Management, c’est le cerveau des logs, le cerveau qui connecte touteslesinformations.

Ce que fait un SIEM
*	📥Centralise les logs de toutes les sources
*	🔗Corrèle les événements entre eux
*	🔔Déclenche des alertes selon des règles
*	🔍Aide à l'enquête (recherche dans les logs)

Sans SIEM : on a des logs partout, mais aucune vision globale

### Ce qu'un SIEM sait ... et ne sait pas
Il sait :
•	✅ Mettre en forme et normaliser
•	✅ Corréler des événements
• ✅	Notifier en temps réel

Il ne sait pas :
•	❌ Décider à votre place
•	❌ Corriger automatiquement (sauf intégration SOAR)

Le SIEM est un outil puissant, mais il a besoin d'humains pour l'exploiter !

## CTI : Cyber Threat Intelligence

L'idée c'est de connaître son ennemi avant qu'il attaque

* 📊Suivre les menaces actuelles
* 🎯Comprendre les campagnes d'attaque
* 🔍Enrichir la détection du SIEM

Concrètement
. IOCs (Indicators of Compromise) : hash de malwares, IPs malveillantes, domaines suspects
. Tactiques MITRE : savoir comment les groupes d'attaquants opèrent
. Flux de renseignement : feeds automatisés (STIX/TAXII)

### SIEM + CTI = détection plus fine 🎯
Sans CTI : on détecte des patterns génériques

Avec CTI :

. On sait qui attaquerait
· Comment ils procèdent
· Avec quels indicateurs les repérer

### EDR : la protection des postes 💻
C'est quoi un EDR ?
**E**ndpoint **D**etection and **R**esponse

Analogie : un antivirusintelligent qui observe les comportements🤖

Ce que fait un EDR
•	️👁️Surveille le poste **en continu**
•	🔍Détecte des **comportements suspects** (pas juste des signatures)
•	🔒Peut **isoler** un poste ou couper un processus
•	📋Enregistre une **timeline** pour l'investigation

L'antivirus classique cherche des signatures connues. L'EDR, lui, observe les comportements

Exemples d'EDR
•	CrowdStrike Falcon
•	Microsoft Defender for Endpoint
•	SentinelOne
•	Wazuh (open-source !🆓)

## XDR : la vision élargie 🌐

C'est quoi un XDR ?
**Ex**tended **D**etection and **R**esponse
•	Regroupe plusieurs couches : poste, réseau, cloud, mail ...
•	Corrèle les alertes entre les couches
•	Donne une vision globale de la sécurité
En résumé : **EDR++** avec corrélation multi-sources🚀

### EDR vs XDR
EDR : voit ce qui se passe sur un poste💻
XDR : voit ce qui se passe sur toutle SI🌐

Le XDR connecte les points entre le réseau, les postes, le cloud et les mails

## Récap' : qui fait quoi ? 📋

️SOC = équipe + process (les pompiers)
🧠SIEM = logs + corrélation (le cerveau)
️🕵️‍♀️CTI = renseignement sur les menaces
💻EDR = défense endpoint (l'antivirus intelligent)
🌐XDR = corrélation multi-défenses

Comment tout ça fonctionne ensemble
1.	Le SIEM détecte une connexion anormale
2.	Le CTI confirme que l'IP est connue malveillante
3.	L'EDR bloque le processus suspect sur le poste
4.	Le SOC prend la décision et lance la remédiation

Chaque brique enrichit les autres - c'est la force de l'écosystème !

## Rappel des acronymes 📖
•	SOC : Security Operations Center
•	SIEM : Security Information and Event Management
•	CTI : Cyber Threat Intelligence
•	EDR : Endpoint Detection and Response
•	XDR : Extended Detection and Response

La cyber adore les acronymes ... mais maintenant vous les connaissez !

# ARP Poisoning
### Qu'est-ce que ARP ?
•	ARP = Address Resolution Protocol
•	Sert à associer :
•	Adresse IP - comme une adresse postale
•	Adresse MAC - identifiant unique de la carte réseau

Exemple :
Le PC veut parler au routeur (192.168.1.1)
Il demande : "Qui a cette IP ? Donne-moi ta MAC !"
Le routeur répond avec sa MAC.

### Comment fonctionne ARP ?
•	Les appareils gardent une table ARP
•	Table ARP = liste IP MAC
•	Protocole sans vérification d'identité
•	Tout appareil peut envoyer des réponses ARP

### Le problème de sécurité
. ARP fait confiance à tout le monde
. Une machine peut mentir et dire :
"Je suis le routeur !"
· Résultat : possibilité de Man-in-the-Middle (MITM)

### Qu'est-ce que ARP Poisoning / Spoofing ?
•	Technique d'attaque réseau
•	Empoisonne les tables ARP des victimes
•	Objectif : faire croire que l'attaquant = le routeur
•	Permet à l'attaquant de :
•	Espionner le trafic
•	Voler des mots de passe (non chiffrés)
•	Modifier ou bloquer la communication

### Schéma simple - Situation normale

Victime ↔ Routeur ↔ Internet

### Après ARP Spoofing
Victime ↔ Attaquant ↔ Routeur ↔ Internet

### Exemple concret
•	Victime : 192.168.1.10
•	Routeur : 192.168.1.1
•	Attaquant : 192.168.1.50

Attaquant envoie :
```
"192.168.1.1 a la MAC
11:11:11:11:11:11"
```
La victime croit que cette MAC = routeur
→ Tout le trafic passe par l'attaquant

### Pourquoi ça marche ?
•	ARP n'authentifie pas les réponses
•	Accepte les réponses même non sollicitées
•	Met à jour automatiquement la table ARP

### Comment se protéger ?
1. 🔒Utiliser HTTPS / VPN - chiffrement du trafic
2. 🛡️Switch avec Dynamic ARP Inspection (DAI)
3. 📡Réseaux Wi-Fi sécurisés (éviter publics non protégés)
4. 🧰Surveiller la table ARP

### Résumé

•	ARP = "Qui a cette IP ?"
•	ARP Spoofing = "Je mens et je dis que je suis quelqu'un d'autre"
•	Conséquence : Attaquant MITM - espionnage, interception ou modification du trafic

### Pour aller plus loin (optionnel)
•	Analyse des paquets ARP avec Wireshark
•	Détection de spoofing sur réseau professionnel
•	Simulations en labo pour tests sécurisés

# Sécurité réseau : VLAN Hopping
### Introduction
### Segmentation réseau et VLANs
•	Les VLANs (Virtual LANs) segmentent le trafic pour : performance, organisation, sécurité
•	Risque : mauvaise configuration - VLAN Hopping
•	Objectif : comprendre l'attaque et se protéger

### Qu'est-ce que le VLAN Hopping ?
•	Technique permettant à un attaquant d'accéder à un VLAN différent de celui auquel il est connecté
•	Contourne les mécanismes de segmentation
•	Peut intercepter, modifier ou perturber le trafic d'autres VLANs

### Faiblesses exploitées :

. Ports de commutateurs mal configurés
· Protocoles VLAN activés par défaut (ex : DTP)
VLAN Hopping : Switch Spoofing
•	Exploite DTP (Dynamic Trunking Protocol) activé par défaut
•	Attaquant force un port à devenir un trunk - accès à tous les VLANs

### Scénario :

1. Attaquant connecte son équipement sur un port non sécurisé
2. Envoie des trames DTP pour activer le trunking
3. Accès à tous les VLANs transitant par le trunk

```
interface FastEthernet0/1
  switchport mode access
  switchport nonegotiate
  switchport access vlan 10
```
### VLAN Hopping : Double Tagging
. Exploite 802.1Q avec deux tags VLAN dans une seule trame
. Nécessite d'être sur le VLAN natif

### Scénario Double Tagging
•	Attaquant sur VLAN natif (souvent VLAN 1)
•	Envoie une trame avec : Tag externe = VLAN natif Tag interne = VLAN cible
•	Premier switch supprime le tag externe - switch suivant lit le tag interne et transmet la trame au VLAN cible Limitation : ne fonctionne que si le VLAN natif est mal configuré

### Ajout d'une couche de sécurité : VLAN natif
•	Par défaut, le VLAN natif est souvent VLAN 1 - cible privilégiée
•	Choisir un VLAN natif non utilise (ex : VLAN 99) pour les ports trunk

```
interface GigabitEthernet0/1
  switchport trunk native vlan 99
```
### Bonnes pratiques pour se protéger

* Forcer les ports en mode access
* Activer trunk uniquement sur les ports nécessaires
```
interface FastEthernet0/1
  switchport mode access
  switchport nonegotiate
  switchport access vlan 10
```
### Sécurité des ports

* Restreindre les adresses MAC autorisées par port
* Bloquer l'accès aux équipements non autorisés

```
interface FastEthernet0/1
  switchport port-security
  switchport port-security maximum 2
  switchport port-security violation restrict
  switchport port-security mac-address sticky
```
### Ports inutilisés

* Fermer tous les ports non utilisés

```
interface FastEthernet0/24
  shutdown
```

### VLANs privés

(PVLAN)

* Permettent d’isoler davantage les hôtes au sein d’un même VLAN
* Utile pour segmenter des machines sensibles sur un même VLAN

### Audit & Surveillance

* Revoir régulièrement les paramètres des switches
* Utiliser un SIEM pour détecter les anomalies
* Documenter et vérifier les configurations VLAN

## Conclusion

* VLAN Hopping = menace réelle pour les réseaux segmentés
* Une configuration négligée peut compromettre plusieurs VLANs
* Mesures efficaces :
  * Désactivation DTP
  * Modification VLAN natif
  * Sécurité des ports
  * Audit et surveillance continue

Voir 👉 [Challenge_C301](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/Challenge_C301.md) 👈

#
