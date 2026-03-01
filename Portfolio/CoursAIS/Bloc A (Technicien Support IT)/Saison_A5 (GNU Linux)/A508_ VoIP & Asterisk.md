# 🐧🗣️ Session 508. VoIP ((Voice Over Internet Protocol) et Asterisk.

## VoIP (Voice Over Internet Protocol) :

https://www.it-connect.fr/quest-ce-que-la-voip/ 

[Tuto] Simuler de la VOIP sur Cisco Packet Tracer (+ vidéo) : https://neptunet.fr/ 

La VOIP est une technologie permettant de transmettre des communications vocales et multimédias sur des réseaux IP.

En termes simples, elle permet de passer des appels téléphoniques via Internet plutôt que par les réseaux téléphoniques traditionnels (RTC, Réseau Téléphonique Commuté).
La voix est numérisée, compressée, et transmise sous forme de paquets de données via le réseau IP, puis recomposée à l'autre bout.

•	Utilisée pour des appels audio et vidéo (exemple : Skype, Zoom).

![01-Fonctionnement de la VoIP](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A5%20(GNU%20Linux)/images%20A5.md/images%20A508/A508_01-Fonctionnement%20de%20la%20VoIP.png)

L'évolution des communications
Du premier télégraphe de Chappe en 1790 au RTC actuel, l'histoire des communications a connu de grands moments et de grandes avancées, dues à l'ingéniosité de certains et aux progrès technologiques et électroniques.

https://fr.wikipedia.org/wiki/Code_Chappe 
Origines de la téléphonie analogique
•	1876 : Invention du téléphone par Graham Bell.
•	Téléphonie analogique : transmission continue des signaux vocaux via des fils de cuivre.

Historique et évolution de la téléphonie sur lP
Années 1970 : Les prémices de la VOIP

Les premières expérimentations avec la transmission de la voix sur des réseaux de données commencent.

Développement des premiers protocoles réseaux, notamment le TCP/IP, qui forment la base de l'Internet moderne.

Passage au numérique
•	Années 1980 : Introduction des systèmes numériques comme l'ISDN (RNIS).
•	Meilleure qualité, plus de fonctionnalités (transfert d'appel, conférence).

Années 1990 : Naissance de la VOIP commerciale.

Les premières solutions commerciales de VOIP apparaissent.

VocalTec lance le premier logiciel de téléphonie sur Internet en 1995, permettant aux utilisateurs de passer des appels PC à PC. Les premiers produits VOIP sont limites en qualité et fiabilité, mais ils montrent le potentiel de la technologie.

Fin des années 1990 et début des années 2000 : Expansion et adoption

Amélioration de la qualité de service (QoS) et augmentation de la bande passante Internet.
Introduction de protocoles comme SIP (Session Initiation Protocol) et H.323, standardisant les communications VOIP.

Skype, lance en 2003, popularise la VOIP auprès du grand public avec des appels gratuits entre utilisateurs.

VOIP : Une technologie mainstream

•	Adoption massive de la VOIP par les entreprises pour réduire les coûts et améliorer la flexibilité.
•	Intégration de la VOIP dans des plateformes de communication unifiée comme :
•	Microsoft Teams
•	Slack
•	Zoom
•	Évolution des services VOIP avec des fonctionnalités avancées :
•	Vidéo
•	Messagerie instantanée
•	Outils de collaboration

La VOlP, toujours en évolution

•	Adoption généralisée de la VOIP à travers le monde.
•	Améliorations continues :
•	Qualité de service (QoS)
•	Sécurité renforcée
•	Nouvelles fonctionnalités innovantes
•	Technologies émergentes intégrées :
•	Intelligence artificielle pour :
•	Transcription automatique des appels
•	Suppression du bruit
•	Amélioration de l'expérience utilisateur

Pourquoi la téléphonie a évolué ?
•	Réduction des coûts
•	Convergence des services : voix, vidéo, données
•	Flexibilité : appels via internet partout dans le monde

Marques de téléphones lP
•	Alcatel
•	Yealink
•	Cisco
•	Poly (anciennement Polycom)

Ces marques dominent le marché des téléphones IP, offrant des solutions variées pour répondre aux besoins des entreprises.

## Mini Switch

![02-Mini Switch](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A5%20(GNU%20Linux)/images%20A5.md/images%20A508/A508_02-Mini%20Switch.png)

### Fonctionnement
•	Mini switch sous le téléphone IP :
•	Un port "Network" :
•	Connecte au switch principal pour accéder au réseau.
•	Un port "Computer" (optionnel) :
•	Permet de connecter un PC directement au téléphone.
### Avantages
•	Optimisation des prises réseau :
•	Une seule prise réseau est nécessaire pour connecter à la fois le téléphone et le PC.
•	Simplicité d'installation :
•	Moins de câblage nécessaire dans les environnements de bureau.
•	Gain d'espace :
•	Réduction des équipements réseau à déployer.

## Power Over Ethernet (POE)

![03-POE](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A5%20(GNU%20Linux)/images%20A5.md/images%20A508/A508_03-POE.png)

L'importance du POE
•	Switch POE recommandé :
•	Fournit l'alimentation au téléphone via le câble réseau.
•	Évite d'utiliser des adaptateurs d'alimentation externes.
En cas d'absence de switch POE
•	Alimentation obligatoire pour le téléphone IP :
•	Nécessite un adaptateur secteur dédié.
•	Moins pratique en termes d'installation et de câblage.
Avantages de la VOIP :
•	Coût faible pour les appels longue distance.
•	Intégration avec d'autres services (messageries, CRM).
•	Évolutivité et flexibilité.

inconvénients de la VOlP :
•	Dépendance à la qualité du réseau.
•	Sensibilité aux pannes et à la sécurité
Qualité de Service (QoS)
•	Définition : Ensemble de mécanismes pour garantir des performances spécifiques dans un réseau.
•	Objectif :
•	Prioriser certains types de trafic (par exemple, voix ou vidéo).
•	Réduire les problèmes de latence, de gigue et de perte de paquets.
Pourquoi la QoS est-elle essentielle ?
•	Les applications comme la VOIP ou la visioconférence nécessitent :
•	Un réseau stable.
•	Une transmission fluide et sans interruptions.
Paramètres clés de QoS
Facteurs influençant la QoS
•	Capacité disponible pour transmettre les données.
•	Latence :
•	Temps nécessaire pour qu'un paquet atteigne sa destination.
•	Jitter (gigue) :
•	Variation de la latence entre les paquets successifs.
•	Perte de paquets :
•	Données perdues pendant la transmission.
•	Bande passante :
Objectif de la QoS
•	Garantir une expérience utilisateur optimale en :
•	Minimisant la latence et la gigue.
•	Évitant les pertes de paquets.
Asterisk est là !
•	Définition : Asterisk est une plateforme open source pour la téléphonie IP.
•	Origine :
•	Créée en 1999 par Mark Spencer.
•	Distribuée sous licence GNU GPL.
•	Utilisation :
•	Système de téléphonie IP (PBX, Private Brand Exchange).
•	Plateforme pour appels VolP, conférence, IVR (Interactive Voice Réponse), etc.

Pourquoi choisir Asterisk ?
•	Flexibilité :
•	Compatible avec des protocoles comme SIP, IAX et H.323.
•	Personnalisation :
•	Scripts pour automatiser les fonctions (dialplans).
•	Coût réduit :
•	Solution sans licence propriétaire.
Fonctionnalités d’Asterisk
Les capacités d'Asterisk
•	PBX complet : https://www.cisco.com/c/fr_ca/solutions/small-business/resource-center/collaboration/what-is-a-pbx.html 
•	Gestion des appels entrants et sortants.
•	Conférences :
•	Création de salles de conférence audio.
•	Serveur VolP :
•	Intégration avec des téléphones IP et softphones.
•	Interactive Voice Response (IVR) :
•	Menus vocaux interactifs.
•	Enregistrement des appels :
•	Pour le support client ou la conformité.
SIP : Session Initiation Protocol
•	Définition : Protocole de signalisation utilisé pour établir, modifier et terminer des sessions multimédia en temps réel
•	Origine :
•	Développé par l'IETF (Internet Engineering Task Force).
•	Standardisé sous RFC 3261.
•	Utilisation :
•	VolP (Voice over IP).
•	Vidéoconférence.
•	Messagerie instantanée.
Fonctionnement de SIP
1. Établir une session :
•	Exemple : Appel vocal ou vidéo.
2. Modifier une session :
•	Ajout d'un participant ou changement de codec.

3. Terminer une session :
•	Fin d'un appel ou d'une conférence.
Avantages de SIP
•	Interopérabilité :
•	Compatible avec de nombreux appareils (téléphones IP, softphones, etc.).
•	Flexibilité :
•	Gère divers médias (audio, vidéo, texte).
•	Simplicité :
•	Utilise une architecture client-serveur.
Asterisk et le Protocole SIP
•	Asterisk agit comme un serveur SIP, gérant :
•	Enregistrements.
•	Transfert des appels.
•	Négociation des codecs.
Architecture d'Asterisk
Oui, il existe un rapport direct et fondamental : la VoIP est l'une des briques les plus complexes qu'un administrateur système et réseau (ASR) doit gérer au quotidien.

Voici pourquoi ce domaine est indissociable du métier d'ASR :

1. La gestion de l'infrastructure (Côté Système)
L'administrateur système installe et maintient les serveurs qui font tourner la téléphonie.

Serveurs IPBX : Il doit configurer des serveurs Linux (souvent sous Debian ou CentOS) pour héberger des logiciels comme Asterisk ou FreeSWITCH.

Virtualisation : Il gère les machines virtuelles ou les conteneurs (Docker) où résident ces services.

Bases de données : Il administre les bases de données qui stockent les journaux d'appels (CDR) et les comptes utilisateurs.
L'administrateur système et réseau installe et sécurise les serveurs Linux (comme Asterisk) qui gèrent les appels, tout en configurant la priorité du trafic réseau (QoS) pour garantir une voix fluide sans gigue ni coupures. C'est un métier qui fusionne la gestion de l'infrastructure logicielle et l'optimisation matérielle pour assurer des communications stables. finalement la VOIP a un vrai lien avec Linux
Structure modulaire
•	Core téléphonie :
•	Gère les appels, la signalisation et la commutation.
•	Modules complémentaires :
•	Protocoles : SIP, IAX, H.323.
•	Fonctions : Enregistrements, conférences, etc.
•	Dialplans :
•	Scripts pour définir le comportement des appels.
•	API (une interface pour communiquer avec une application) : 
•	Intégrations possibles via AMI (Asterisk Manager Interface) et AGI (Asterisk Gateway Interface).
Exemple de flux d'appel
1. Appel entrant via SIP.
2. Traitement par le dialplan.
3. Acheminement vers un téléphone IP, un IVR ou une boîte vocale.
Le Dialplan dans Asterisk
•	Définition :
•	Ensemble de règles qui définissent comment Asterisk traite les appels.
•	Écrit dans le fichier extensions.conf.
•	Rôle :
•	Décider du routage des appels (entrant ou sortant).
•	Déclencher des actions spécifiques (boîte vocale, IVR, etc.).
Structure d'un Dialplan
•	Un dialplan est composé de :
•	Contextes :
•	Groupes logiques pour séparer les appels (exemple : interne, externe).
•	Extensions :
•	Numéros ou actions à exécuter.
•	Priorités :
•	Ordre dans lequel les étapes sont exécutées.
•	Applications :
•	Actions spécifiques, comme Dial, Voicemail, ou Playback.

### Exemple de Dialplan
```
[internal]
exten => 100,1,Answer()
exten => 100,2,Playback(hello-world)
exten => 100,3,Hangup()
```

## Cas d’utilisation d’Asterik
### Applications courantes
•	Entreprises :
•	Gestion centralisée des appels avec un PBX.
•	Centres d'appels :
•	Routage intelligent et intégration CRM.
•	Opérateurs télécom :
•	Fourniture de services VolP.
•	Projets loT :
•	Communication vocale entre machines.

### Exemples concrets
•	Intégration d'Asterisk avec :
•	FreePBX pour simplifier l'administration : https://www.freepbx.org/ 
•	CRM comme Salesforce pour un suivi client.
•	Plateformes comme Zoom pour les conférences hybrides.

![04-Ex Asterisk](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A5%20(GNU%20Linux)/images%20A5.md/images%20A508/A508_04-Ex%20Asterisk.png)

La doc d’Asterisk : https://www.asterisk.org/ 

https://doc.ubuntu-fr.org/asterisk 

------------------------
nproc : pour voir les nombres de cœurs de la machine
make -j$(nproc) : lance make en parallèle. -j : nombre de tâches simultanées. $(nproc) : nombre de cœurs CPU disponibles. Cela fait une compilation plus rapide, en utilisant tous les cœurs du processeur.
```
sudo make install-logrotate
```
![05-Asterisk installation](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20A%20(Technicien%20Support%20IT)/Saison_A5%20(GNU%20Linux)/images%20A5.md/images%20A508/A508_05-Asterisk%20installation.png)

123 = le numéro de tél
1, 2, 3 et 4 les étapes

sudo systemctl status … pour voir le statut d’un système ou application
sudo systemctl start … pour activer un système ou application

pjsip set logger on : active les logs SIP détaillés dans Asterisk (PJSIP)

Concrètement :
•	affiche tous les échanges SIP (REGISTER, INVITE, 200 OK, etc.)
•	en temps réel dans la console Asterisk
•	sert au debug des appels et de l’enregistrement SIP
Pour l’arrêter serait pjsip set logger off

sudo netstat -anup affiche les connexions réseau et ports ouverts de la machine
Détail :
•	sudo : nécessaire pour voir les processus
•	-a : toutes les connexions (écoute + actives)
•	-n : adresses et ports en numérique (pas de résolution DNS)
•	-u : connexions UDP
•	-p : programme / PID associé
👉 Utile pour voir quel service utilise quel port UDP et détecter des services actifs ou suspects.
------------------------







