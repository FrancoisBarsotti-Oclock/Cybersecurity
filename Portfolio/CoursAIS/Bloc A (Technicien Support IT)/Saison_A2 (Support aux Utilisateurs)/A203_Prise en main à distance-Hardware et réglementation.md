# 💻 A203: Prise en main à distance-Hardware et réglementation.

## Ping (Cours du 22-10)

8.8.8.8 c’est l’adresse DNS de Google qu’on peut utiliser pour pinguer et tester si nous avons de la connexion.

8.8.4.4 c’est l’adresse DNS de

1.1.1.1 DNS pour Cloudflare

9.9.9.9 DNS pour quad9

Avant de pinguer il faut manipuler le pare-feu

On manipule le pare-feu sur les règles de trafic entrant

on va manipuler ceux qui correspondent au type de réseau sur lequel on est connecté (public ou privé).

On choisit imprimante

# Contrôle à distance et procédures

(Voir Pascal ROBERT)

**Objectifs de la journée** :

- Acquérir les compétences nécessaires pour dépanner un utilisateur à distance.
- Rédiger des procédures et modes opératoires clairs et efficaces.
- Comprendre l’impact économique du dépannage à distance pour une entreprise.
- évaluer les avantages du dépannage à distance par rapport aux interventions sur site.

## C’est quoi le contrôle à distance ?

Le contrôle à distance, également appelé télémaintenance ou assistance à distance, est une méthode permettant de gérer, superviser et dépanner un équipement informatique à distance, sans être physiquement présent sur le site.

## Les avantages du contrôle à distance

- Réduction des coûts : moins de déplacements
- Gain de productivité : on peut résoudre plus de problèmes dans une même journée
- Flexibilité : on peut intervenir immédiatement

## Les équipements concernés

- N’importe quelle PC ou portable (Windows, MacOS, Linux)
- Les serveurs (Windows ou Linux)
- Y compris les tablettes/smartphones si besoin (moins courant)
- Accès aux périphériques (imprimantes, copieurs, etc.)

## Les types d’interventions

- Assurer une maintenance préventive (idéalement planifiée à l’avance)
- Faire une maintenance curative (dépannage)

	 - Résoudre un problème d’imprimante non détectée
	- Résoudre un problème de logiciel qui ne démarre pas/qui plante
	- Aider l’utilisateur dans une opération qu’il n’arrive pas à réaliser.
-  Installation d’un logiciel tiers (environnement particulier)

## Les coûts pour une entreprise

- Logiciels de contrôle à distance (acquisition ou abonnement)
- Infrastructure IT (outils/systèmes sécurisés)
- Sécurité et conformité (RGPD, protection des données)
- Formation du personnel aux outils

## Comparaison des coûts :

- Intervention sur site
	- Temps de déplacement
	- Frais de déplacement
	- Perte de temps d’attente
- Interventions à distance
	- Coût du logiciel
	- Intervention immédiate
	- Pas de contrainte géographique

## Les différentes solutions

- Outils natifs aux OS (SSH/Bureau à distance)
- Solutions tierces (Anydesk/Teamviewer)
- Contrôle cloud (Supervision dans le web)

## Prérequis pour utiliser une solution

- Nécessité d’une connexion internet fiable
- Analyse des risques pour la sécurité/confidentialité des données
	- Authentification forte recommandée
	- Chiffrement essentiel
- Dépendance aux outils de contrôle à distance
- Formation des utilisateurs/techniciens
- Une bonne communication avec les clients

# Les outils natifs Windows

## Le Bureau à distance de Windows : Connexion à distance (mstsc ou rdp)

Protocol RDP

- Activation via : Paramètres/Système / Bureau à distance
- Connexion via : Barre de recherche / Bureau à distance
**Inconvenients**
- Nécessité de se connecter en local
- Possibilité à distance (ouvertures de port, règles pare feu)

## L’assistance rapide Windows

- Accès via : Barre de recherche / Assistance rapide

- Une seule fenêtre pour aider / se faire aider

- Demande d’autorisation (confidentialité)
**Inconvénients**
- Nécessité de disposer d’un compte Microsoft (des deux côtés)

## Le powershell remoting (avec WinRM)

- Administration à distance d’un poste/serveur Windows via un invite de commande
- Nécessite une mise en place compliquée (ouverture de ports/règles de pare-feu)
- pas intéressant pour support à l’ultilisateur

# Les outils natifs Linux

## SSH

- Administration à distance via un shell Linux
- En général, utiliser pour manager des serveurs Linux
- Utilisation avec des outils Windows comme **Putty** par exemple

## Gnome remote desktop (ubuntu/debian)

Dans une distribution Linex, on y accède via **Paramètres / partage / Partage d’écrains**

- On peut utiliser avec l’outil **Bureau à distance de Windows**.
- On peut l’utiliser en mode partage d’écran ou en bureau à distance (deux onglets)
**Inconvenients**
- Nécessité de se connecter en local
- Possibilité à distance (ouvertures de port, règles pare-feu)

# Contrôle cloud (Supervision dans le web)

Les solutions cloud offrent une infrastructure permettant aux entreprises de gérer échelle ces services sans avoir à installer de matériel ou de logiciels sur site.
**Avantages**

- Accès universel
- Gestion centralisée
- Sécurité renforcée
- Scalabilité
- Maintenance préventive (sans intervention sur site)

## Et concrètement ?

- L’utilisateur à un raccourci sur son bureau pour accéder à l’application
- L’application est lancée dans un environnement virtuel (aussi appelé **appliance**)
- L’application n’est pas installée physiquement sur sa machine
- Avantages pour le technicien et l’utilisateur (on peut y accéder de n’importe où).

## Les différentes solutions Cloud

- Citrix Virtual Apps and Desktops (anciennement XenApp et XenDesktop)
- Microsoft Remote Desktop (RDS et Azure Virtual Desktop)

# Les solutions tierces

## VNC

A la fois un outil et un protocol… Une application de partage de bureau à distance (c’est un peu dangereux, à bien sécuriser)
- La solution historique
- Vieux protocole
- Nécessite une sécurité renforcée pour l’utiliser à distance (SSH par exemple)
- Fonctionne avec un logiciel serveur et un logiciel client

## Quelques solutions VNC

Voici quelsues solutions existantes

- RealVNC (la mieux maintenue, mais payante pour une bonne utilisation)
- Apple Remote Desktop
- KDE – KDRC (avec Linux)
- UltraVNC
- TighVNC (solution opensource / multiplateforme)

C’est des accès libres sans identifiant ni confirmation

C’est utilisé dans la black hacking pour rentrer

## Le cas de TighVNC

# Avec son propre port (TCP / )

## AnyDesk

Convient particulièrement aux entreprises nécessitant un outil de prise en main à distance peu coûteux mais performant.

- Accès à distance rapide
- **Sécurité renforcée** avec un chiffrement TLS 1.2
- **Support multiplateforme** : Compatible avec Windows, macOS, Linux, iOS et Android
- Licences dès 23 € par an en solo.

**Avantages AnyDesk**
- Très rapide et performant, même sur des connexions lentes
- Interface simple et facile à utiliser
- Modèle tarifaire compétitif pour les petites entreprises
**Inconvénients AnyDesk**
- Moins d’options de gestion centralisée pour les grandes entreprises
- Certaines fonctionnalités avancées nécesitent une licence professionnelle

## TeamViewer

TeamViewer est **la solution populaire de contrôle à distance** qui permet aux techniciens de se connecter à des machines distantes via Internet pour offrir un support intantané.

- Contrôle à distance
- **Compatibilité multiplateforme** : prend en charge Windows, macOS, Linux, iOS et Android
- **Session sécurisée** : Les connexions sont chiffrées de bout en bout pour garantir la confidentialité.
- **Collaboration** : Possibilité de partager l’écran ou de travailler à plusieurs techniciens sur une même session.

Les licences équipes : Attention à l’utilisation des **connexion simultanées supplémentaires** (à définir en fonction des besoins)

**Avantages TeamViewer**
- Simplicité et rapidité de déploiement
- Pas de configuration spécifique
- Compatible avec de nombreaux sytèmes d’exploitation
**Inconvénients TeamViewer**
- Coût par utilisateur élevé
- Dépendance à la connexion Internet (performaces affectées en cas de mauvaise connexion).

Pour le challenge
![[Pasted image 20251031142235.png]]

TV::: YP00K3GZ4V1BYD5N
## RustDesk
C’est un produit OpenSource et gratuit pour un plan auto-hébergé. Elle ne dépend d’aucun service, ni logiciel, ni de société (voir son github).

# Communication efficace pendant le dépannage

## Gestion de la relation avec l’utilisateur

- Poser les bonnes questions pour identifier précisément le problème
- Adapter le discours technique au niveau de l’utilisateur
- Expliquer chaque étape et rassurer l’utilisateur, surtout sur la sécurité et la confidentialité des données lors de la prise de contrôle

## Communication spécifique selon l’OS
**
- Pour les utilisateurs macOS ou Linux, la terminologie peut être différente de celle utilisée pour Windows.
- Expliquer des étapes comme l’accès à l’utilitaire de disque sur macOS ou la ligne de commande sous Linux de manière simple (parler clairement et posé sur ce qu’il faut qu’il fasse sans essayer de lui transmettre de la connaissance technique, comme si l’on parlait à un enfant).

On ne pourra pas éduquer à toute la population de l’entrepirse

# Procédures écrites (en interne et avec les utilisateurs)

## Pourquoi rédiger des procédures et de modes opératoires ?

- Documenter les interventions permet de standardiser les pratiques et d’assurer une transmission efficace des connaissances au sein de l’entreprise.
- Gain de temps pour résoudre des problèmes récurrents
- Il faut adapter le vocabulaire selon l’intervenant qui sera lié à la procédure

## Structure d’une bonne procédure (méthode)

- **Titre et contexte :** Quel problème est résolu ?
- **Prérequis** : Les outils nécessaires, accès requis
- **Etapes détaillées** : instructions étape par étape avec captures d’écran, si possible, adaptées à l’OS en question.
- **Résultats attendus** : indications de ce que l’utilisateur doit observer à la fin
- **Solutions alternatives** : Options en cas d’échec d’une étape (notamment pour les différents environnements d’OS).

## Rédaction adaptée aux différents systèmes d’exploitation

- **Windows** : Inclure des captures d’écran de l’explorateur, de la gestion des périphériques, des services, etc.
- **MacOS** : Instructions spécifiques comme accéder aux « Préférences Système » ou utiliser le terminal.
- **Linux** : Privilégier la ligne de commande, donner des exemples concrets de commandes à utiliser.

## Les procédures pour des interventions à distance et les avantages

- **Documentation facilitant les futures interventions** : par exemple, une procédure pour la réinitialisation d’une connexion VPN peut être réutilisée et ajustée selon l’OS.
- **Réduction des coûts** : une bonne documentation permet aux utilisateurs de résoudre certains problèmes eux-mêmes sans avoir à solliciter systématiquement le support technique (FAQ sur un site/intranet par exemple).

    **Note** : Attention aux sites qui collectent tout dedans (genre clubc, 01.net, etc) car ils ont tendance à installer des malwares (voir videos Underscore_)

# Impact économique et décision sur le choix de l’intervention

## Comparaison entre dépannage à distance et intervention sur site

- **Quand privilégier une intervention à distance** (problèmes logiciels, configurations réseau, installation de logiciels) **vs une intervention sur site** (problèmes matériels, diagnostics physiques)
- **Avantages pour l’entreprise** en termes de coûts et de temps : la plupart des problèmes logiciels et de configuration peuvent être résolus à distance sans frais de déplacement.

## Exemple d’analyse coût /bénéfice

- **Exemple 1** : Un utilisateur a un problème de connexion réseau, la résolution à distance permet de restaurer la connexion en quelques minutes.
- **Exemple 2** : Un utilisateur rencontre un problème matériel (écran défectueux), une intervention sur site est nécessaire, mais les diagnostics à distance peuvent réduire le temps d’immobilisation en préparant l’intervention.

# Conclusion et synthèse

Récapitulatif des points abordés

- Maîtrise du dépannage à distance sur plusieurs systèmes d’exploitation (Windows, macOS, Linux)
- Compréhension des avantages économiques du dépannage à distance pour une entreprise.
- Importance de la communication efficace et de la rédaction de procédures claires.
- Méthodes de rédaction des procédures

# Incidents hardware et règlementation

## Objectifs de la journée

- Diagnostic et résolution de pannes matérielles
- Réglementation DEEE (Déchets d’Equipements Electriques et Electroniques)
- Réglementation RGPD (Règlement Général sur la Protection des Données)

## Introduction au dépannage (avec un focus sur le matériel)

- Vous ne serez pas (tout de suite) des experts à l’issue de cette introduction (il faut des années de pratique)
- Vous aurez une idée de comment diagnostiquer une panne
- Vous aurez la capacité d’échanger plus facilement avec un professionnel

## Petits rappels – les types de pannes

![[Pasted image 20251029220451.png]]
## Petite analogie

![[Pasted image 20251029220520.png]]
## En résumé
Si on compare un ordinateur à un corps humain
- le système sanguin permet de faire circuler l’énergie
- le système nerveux permet de faire circuler la data (les informations) entre le cerveau et les organes.
## Avant de démonter un ordinateur
Avant d’en arriver à une phase de diagnostic plus poussée, quelques opération simples à réaliser :
- Effectuer un nettoyage de poussière dans l’ordinateur (souvent responsable)
- Débrancher tous les périphériques (garder clavier/souris + écran seulement)
- Tester avec des câbles de branchement en bon état de fonctionnement (on y pense pas souvent)
- Faire un test de démarrage pour analyser le problème et avoir un premier diagnostic
## L’ordinateur
On a plusieurs composants

![[Pasted image 20251029220602.png]]
## La pile du BIOS

On oublie pas de débrancher…

![[Pasted image 20251029220635.png]]
## Si j’ai un écran noir, de quoi ça peut venir ?

![[Pasted image 20251029220700.png]]
## Les causes d’un écran noir
Voici les différentes sources à vérifier :
- le branchement des prises (écran, pc)
- changer les câbles électriques (extérieurs)
- changer l’alimentation de l’ordinateur
- débrancher la carte graphique (utiliser la sortie de la carte mère)
- vérifier le branchement de la RAM
- vérifier la pile du BIOS (la changer si besoin)
- tenter un clear CMOS (jump sur carte mère) : un cavalier ou un jumper, très souvent un bouton
## Les quatre causes d’un écran noir
Dans certains cas plus graves
- changer la carte mère par une autre (attention à la compatibilité processeur et RAM)
- changer le processeur
## Sons inhabituels

![[Pasted image 20251029220831.png]]
## J’ai un écran noir avec un message

Souvent, le disque dur en est la principale cause.

![[Pasted image 20251029220855.png#center]]
## J’ai une erreur Windows au démarrage

![[Pasted image 20251029220936.png#center|200]]
## Ici un échec lors de la dernière extinction
Essayez de démarrer le PC dans les différents modes de Windows
- mode normal
- mode sans échec
## Et si la machine s’allume, mais que j’ai d’autres erreurs ?
Il s’agit probablement d’un problème logiciel
- Virus
- problème de démarrage dû à une mauvaise mise à jour
- autres divers problèmes
## Résumé (partie 1)
En première étape, on identifie les symptômes
- Ecran noir ?
- Ecran comportant des erreurs
- Poussière accumulée ?
- Comportement anormal de l’ordinateur
- Sons inhabituels
- Analyse des messages d’erreurs du BIOS/UEFI
## Résumé (partie 2)
En deuxième étape, on recherche des causes possibles (pour cela, on isole le problème)
- Est-ce qu’un composant est en panne et empêche le démarrage ? (carte mère, processeur, RAM, alimentation électrique)
- On utilise des outils de diagnostics intégrés une fois qu’on a au moins accès à un écran BIOS
# Les outils de diagnostic/monitoring (favoriser les apps portables)

Ici, nous allons voir quelques outils de diagnostic/monitoring sur le matériel (dont beaucoup **gratuits**).

Cela permettra d’analyser les faiblesses en détail !

Cette étape doit être faite uniquement si le PC démarre !
## Le multimètre
Utiliser un multimètre peut permettre de vérifier correctement l’alimentation électrique.
![[Pasted image 20251029221109.png#center]]
## Le BIOS/UEFI [[BIOS, UEFI...]]
**POST (Power-On Self Test)**: comprendre les résultats du POST pour détecter les composants en panne avant le démarrage du système d’exploitation
![[Pasted image 20251029221159.png#center]]
## Le test du processeur
Le logiciel CPU-Z est la référence en la matière. Des sondes permettent de voir les températures/fréquences des composants.
## Le test de la carte graphique
Le logiciel **CPU-Z** est la référence en la matière. Des sondes permettent de voir les températures/fréquences des composants.

![[Pasted image 20251029221553.png]]
## La RAM uniquement
Les logiciels **MemTest64**, **MemTest86** ou **MemTest86+** (bootable) permettent de voir si la RAM va bien !
![[Pasted image 20251029221629.png]]
## Le stockage (disque mécanique/SSD)
Le logiciel **CrystalDiskMark** vous permet de faire des tests de performacnce de votre disque. Attention : l’était de santé n’y figure pas (on utilisera **SSD Life** ou **HWMonitor** dans ce cas). Il existe également d’autres outils (comme **chkdskg** en CMD).

 ![[Pasted image 20251029221701.png]]
## Les ventilateurs, vitesses, etc
Le logiciel **HWMonitor**, **Everst Ultimate**, **Aida 64** et **Speccy** vous permettent d’avoir ce genre de statistiques (très complet).

![[Pasted image 20251029222047.png]]
## L’alimentation
Le logiciel **OCCT** est très complet. Attention, il fera souffler les équipements pendant les tests !

![[Pasted image 20251029222127.png#center]]
## Les batteries
Le logiciel **BatteryBar** est gratuit et permet de faire des tests. Ce test est à réaliser sur 1 à 2 semaines.
![[Pasted image 20251029222214.png]]
## Tests approfondis des composants
Ici des exemples de ce qu’il est possible de faire une fois le composant isolé :
## Alimentation (PSU)
- Vérification des tensions : Utilisation d’un multimètre pour mesurer les différentes tensions de sortie (3.3V,5V, 12V) sur les différents connecteurs.
- Test de charge : Utilisation d’une alimentation Utilisation d’une alimentation de test pour vérifier la stabilité de l’alimentation sous charge.
- Détection de surchauffe ou d’usure : Utilisation d’un thermomètre infrarouge pour détecter la surchauffe.
## Mémoire RAM
- Test avec **MemTest86+** : Outils de diagnostic de la mémoire pour détecter des erreurs sur chaque barrette de RAM
- Test manuel : Enlever une barrette de RAM à la foi pour voir si l’ordinateur fonctionne correctement avec une seule barrette.
## Disques durs
- Utilisation de **CrystalDiskInfo** pour connaître l’état de santé du disque
- Test de surface du disque : Utilisation d’outils comme **HD Tune** pour vérifier la surface des disques et identifier les secteurs défectueux.
- Vérification des câbles SATA et alimentation : Rebrancher ou remplacer les câbles pour éliminer un problème de connexion.
## Carte mère et processeur
- POST et codes d’erreurs : Utilisation des bips ou des codes d’erreurs générés par la carte mère pour identifier des pannes liées au processeur ou à la carte mère
- Test avec une carte mère de remplacement : Si disponible, tester le processeur sur une autre carte mère pour éliminer l’hypothèse d’une panne de carte mère.
## Carte graphique
- Test dans un autre  ordinateur : Installer la carte graphique sur un autre ordinateur pour vérifier si le problème persiste.
- Test avec une autre carte graphique : Si possible, utiliser une autre carte graphique pour voir si le problème disparaît.
- Problèmes de pilotes : Vérifier si les pilotes de la carte graphique sont à jour ou mal configurés.

**Le cours qui suit: ** [[Réparation & Réglementation]]
