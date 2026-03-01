# 📗 Session C101. Introduction à la Gestion de Projets.

Notions du jour
*	projet et gestion de projet
*	cycle V et cycles semi-itératifs
*	Agile et Scrum
*	structuration, planification, suivi de projet
*	gestion des risques d'un projet
*	menaces et vulnérabilités IT / SI
*	PCA / PRA

Quelques mots sur la saison
Les notions de projet, gestion de projet et sécurité sont fondamentales dans nos façons de travailler.

Nous devons souvent répondre à un besoin particulier dans un délai donné pour un coût établi. Pour répondre à ces objectifs, nous travaillons en mode projet. La réalisation d'un projet passe par différentes phases, selon la typologie du projet et selon le besoin.

La réalisation d'un projet doit également prendre en compte certaines spécificités, dont la gestion du risque.

Lorsque que le projet est terminé, que le produit est livré, le produit poursuit sa vie. Il peut évoluer et, dans tous les cas, nous ne devons pas oublier de le superviser !

Programme de la saison : https://gist.github.com/stephdl/92a7311117ca7ddbcf42b9a506eec707 

Programme de formation - Gestion de projet IT & Cybersécurité
Saison C1 - Semaine 1
Jour 1 : Introduction à la gestion de projet
Présentation du programme de la semaine Notion de projet : définition et types de projets IT Différence entre projet et activité classique
Gestion de projet : définitions, objectifs et historique Types de gestion de projet (cycle en V, méthodes semi-itératives) Triangle qualité/coûts/délais Focus sur le cycle en V

Jour 2 : Méthodologies Agile et outils
Correction du challenge Jour 1 Introduction aux méthodes Agile Analyse du Manifeste Agile Présentation de Scrum (rôles, sprints, cérémonies)
Outils de gestion de projet Structuration, planification et suivi Communication projet

Jour 3 : Gestion des risques
Correction du challenge Jour 2 Comprendre la notion de risque Identification et évaluation des risques Matrices d'évaluation (probabilité × impact)
Documentation et traitement des risques Introduction à la méthode EBIOS RM Application dans un contexte projet

Jour 4 : Sécurité et continuité d'activité
Correction du challenge Jour 3 Sécurité des systèmes d'information dans les projets Menaces, vulnérabilités et bonnes pratiques
Plan de Continuité d'Activité (PCA) Plan de Reprise d'Activité (PRA) Sauvegardes, redondance et supervision

Jour 5 : Évaluation et bilan
Correction du challenge Jour 4 Évaluation en Cours de Formation (ECF)
Bilan de la semaine Retour d'expérience et questions
Projet, définitions
Dessein, intention qu'on a de réaliser quelque entreprise, et qui prend en compte les moyens utiles à sa mise en œuvre ; ce que l'on se propose d'accomplir. - Académie française

Un projet est un ensemble unique de processus, constitués d'activités coordonnées et maîtrisées, ayant des dates de début et de fin, et entreprises pour atteindre les objectifs du projet. La réalisation des objectifs du projet requiert la fourniture de livrables conformes à des exigences spécifiques. – [ISO 10006](https://fr.wikipedia.org/wiki/ISO_10006) 

Un projet est une « entreprise temporaire initiée dans le but de fournir un produit, un service ou un résultat unique ». – [PMI]( https://fr.wikipedia.org/wiki/Project_Management_Institute) 

Un projet est « une organisation temporaire, créée en vue de livrer un ou plusieurs produits du projet conformément à un cas d'affaire convenu ». – [PRINCE2]( https://fr.wikipedia.org/wiki/PRINCE2) 

Définition simple : Un projet est un ensemble d'activités coordonnées visant à atteindre un objectif unique, dans un temps défini, avec des ressources limitées.

Éléments caractéristiques
*	un objectif / un but @
*	une date de début 17
*	une date de fin
*	une organisation
*	des livrables
*	un budget
*	un résultat unique

Pourquoi fait-on des projets ?
•	répondre à un besoin
•	améliorer le service
•	remplacer un matériel obsolète
•	renforcer la sécurité
•	passer à l'échelle

Ce qu'un projet n'est pas
•	une urgence (remettre un service en marche)
•	une tâche répétitive (gestion des comptes utilisateurs)
•	une idée floue (on pourrait changer nos serveurs)
•	juste "installer un logiciel" (sur un poste, monter une VM isolée)
•	un problème à résoudre (le serveur DHCP tombe souvent)

### Différence projet / activité courante 

| **Activité** | **Projet** |
| --- | --- |
| Répétitive | Unique |
| Sans début/fin précisés | Temps défini |
| Procédures établies | Démarche spécifique |
| Objectif stable | Objectif unique |

Projets IT
Quels sont les grandes familles de projets en informatique et leurs enjeux ?

Projets d'infrastructure
•	Installation de serveurs
•	Virtualisation (Hyper-V / VMware / ProxMox)
•	Installation réseau (switch, VLAN, Wi-Fi)
•	Déploiement d'un SAN ou NAS

Enjeux : performance, disponibilité, sécurité

Projets de migration
•	Migration OS (Debian 9 vers Debian 12)
•	Migration réseau (nouvelle architecture VLAN)
•	Migration vers le Cloud (O365, GSuite)
•	Migration d'infrastructure (changement de serveur)
•	Migration d'applications (Apache vers Nginx)

Enjeux : compatibilité, rollback, continuité de service


Éléments caractéristiques
•	un objectif / un but
•	une date de début
•	une date de fin
•	une organisation
•	des livrables
•	un budget
•	un résultat unique
(Revoir slide)

Projets de cybersécurité
•	Déploiement firewall / segmentation réseau
•	Mise en place MFA
•	Audit de sécurité
•	Gestion des vulnérabilités
Enjeux : réduction du risque, conformité ANSSI / ISO

Projets applicatifs
•	Déploiement d'une application métier
•	Mise en place d'outils collaboratifs
•	Installation d'un ERP
Enjeux : intégration, performance, support

Le prompt pour l’Ia : S’il te plait, vas lire la documentation (et aussi reformule) : https://www.acronymat.com/2024/12/29/cidi-frameworks/ 

![01-01-CIDI AI prompting](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_01-CIDI%20AI%20prompting.png)

Voir aussi [Risen](https://neworbit.co.uk/blog/post/AI/write-better-ai-prompts-with-risen-framework/) 

[Voir aussi](https://cold-reindeer-860.notion.site/77f8c6e0389748ae9dcefad180503c36?v=ac6bb15a6d9d421ebd30ec7c1245c033&p=8781a6e90bae49c9baff95924bf47c15&pm=s) 

Projets d'optimisation / transformation
•	Supervision (Centreon, Zabbix)
•	Automatisation (scripts, Ansible)
•	Refonte infrastructure vieillissante
Enjeux : réduction du coût, modernisation, résilience

Contraintes d'un projet IT
•	techniques (interopérabilité, versions)
•	humaines (utilisateurs, prestataires)
•	financières
•	de sécurité
•	d'exploitation
Exemples de gros projets
•	2012-2013 : datacenter de Facebook construit en 13 mois (Luleå, Suède)
•	conception, construction, migration de services
•	automatisation extrême
•	coordination très forte entre travaux, réseaux, électricité, cooling, plateformes applicatives

•	2008-2016 : migration de Netflix vers AWS
•	migration progressive de toute l'infrastructure vers AWS
•	haute disponibilité, tolérance aux pannes, chaos engineering
•	projet très complexe mené avec une approche incrémentale
Conclusion
•	Un projet est une démarche temporaire, unique et organisée
•	Les projets IT couvrent plusieurs familles :
•	Infrastructure
•	Migration
•	Cybersécurité
•	Applicatifs
•	Optimisation / transformation

Voir des [certificats gratuits](https://letsencrypt.org/).  Let's Encrypt is a Certificate Authority that provides free TLS certificates, making it easy for websites to enable HTTPS encryption and create a more secure Internet for everyone. Let's Encrypt is a project of the nonprofit Internet Security Research Group.

•	Vous serez souvent acteurs opérationnels dans ces projets
•	Comprendre les types de projets permet de :
•	mieux estimer le travail à réaliser
•	anticiper les contraintes
•	identifier les risques
•	communiquer efficacement

En informatique, un problème c’est quelque chose qui se répète.

Gestion de projet
Qu'est-ce que la gestion projet ?

La gestion de projet, la conduite de projet, l'ingénierie de projet, ou encore le management de projet est l'ensemble des activités visant à organiser le bon déroulement d'un projet et à en
atteindre les objectifs en temps et en heures selon les objectifs visés. Elle consiste à appliquer les méthodes, techniques, et outils de gestion spécifiques aux différentes étapes du projet, de
l'évaluation de l'opportunité jusqu'à l'achèvement du projet. - Wikipédia - Gestion de projet
Version simple
La gestion de projet est une démarche visant à organiser de bout en bout le bon déroulement d'un projet.
Pourquoi gérer un projet ?
•	structurer l'activité
•	mieux planifier les tâches
•	évaluer les risques
•	coordonner plusieurs acteurs (là on est aussi sur les tâches)
•	optimiser les ressources
•	réduire les imprévus (là on est aussi sur les risques)
•	documenter les choix
•	améliorer la qualité du résultat

Exemple de RACI (tâches)

![02-RACI tâches](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_02-RACI%20t%C3%A2ches.png)

### Les objectifs finaux :

•	améliorer la satisfaction du client
•	avoir une vision partagée
•	pouvoir arbitrer entre alternatives

### Gérer les contraintes qualité / coûts / délais

![03-QCD](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_03-QCD.png)

Conserver l'équilibre entre :
•	la qualité du projet
•	bonne expression des besoins
•	tout au long du projet
•	mécanismes de contrôle qualité
•	pas que sur le livrable final

•	le coût du projet
•	estimation réaliste puis ajustement
•	coûts d'études
•	coûts de réalisation
•	décisions rapides en cas de dépassement

•	le délai de réalisation du projet
•	souvent imposé par le client
•	doit être réaliste et validé par les différents acteurs
Avant le XXe siècle

Pas vraiment de gestion de projet mais ...

•	construction des pyramides
•	cathédrales médiévales
•	routes, ponts, aqueducs
•	grandes expéditions scientifiques ou militaires

Caractéristiques

•	approche empirique
•	organisation hiérarchique stricte
•	documentation faible
•	absence de méthode formelle
Débuts du XXe siècle
Les fondations

•	Frederick Taylor (1900-1910) - Organisation scientifique du travail 
•	chaque tâche doit être mesurée, optimisée, standardisée
•	influence majeure sur la planification et l'allocation des ressources

•	Karol Adamiecki (1866-1933) et Henry Gantt (1910-1915) - Diagramme de Gantt
•	outil visuel pour représenter les tâches dans le temps
•	un des premiers outils modernes de gestion de projet

Années 50
Méthodes de planification

•	Méthode CPM (Critical Path Method, 1957)
•	développée dans l'industrie chimique (DuPont)
•	objectif : optimiser les délais et les coûts

•	Méthode PERT (1958)
•	développée par l'US Navy
•	objectif : gérer des milliers d'activités interdépendantes
•	chemin critique
•	gestion des dépendances
Années 60-80
•	Arrivée des systèmes complexes, de l'informatique
•	la gestion de projet devient une discipline autonome

•	1956 : modèle de gestion de projet Waterfall (en cascade)
•	plusieurs phases successives

•	1969 : création du PMI
•	objectif : standardiser les pratiques

•	1970 : Winston W. Royce
•	décrit et critique le modèle Waterfall
•	présente le cycle V
•	et les modèles semi-itératifs

Années 90
Explosion de l'informatique et nouvelles méthodes.

Les projets IT deviennent plus rapides, plus changeants, plus complexes.

•	Premiers modèles itératifs et incrémentaux
•	1996 : [première version du PMBOK par le PMI](https://en.wikipedia.org/wiki/Project_Management_Body_of_Knowledge)

Années 2000

•	2001 : Manifeste Agile
•	Itérations
•	Collaboration
•	retours rapides
•	produit fonctionnel plutôt que documents

L'infrastructure commence à s'en inspirer.

Années 2010-2020
L'IT doit aller plus vite, plus automatisé, plus sécurisé :

•	DevOps
•	rapprochement Dev et Ops
•	automatisation (CI/CD)
•	monitoring, fiabilité

•	Cloud
•	projets hybrides

•	Gestion des risques et de la sécurité
•	EBIOS
•	ISO 27001
•	ANSSI
Aujourd'hui
Gestion de projet hybride / mixte car :

•	tout ne peut pas être Agile (ex : migration AD, changement de firewall)
•	tout ne peut pas être figé comme dans un cycle en V

Combinaison :

•	d'une structure traditionnelle (cycle en V)
•	de pratiques agiles (itérations, feedback)

Les étapes de la gestion de projet
•	Définition : savoir ce que veut le client
•	Conception : savoir ce que l'on va faire
•	Réalisation : faire ce que l'on doit faire
•	Validation : le résultat répond-il au besoin
•	Livraison : fournir les résultats du projet

## Cycle en V
Chaque phase de conception correspond à une phase de test.

![04-Cycle en V](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_04-Cycle%20en%20V.png)

### Avantages
•	très structuré
•	documentation complète
•	adapté aux projets d'infrastructure
### Limites
•	peu flexible
•	complexité si changement tardif
•	risque d'un effet tunnel

### Cycles semi-itératifs / Agile
Après la phase d'analyse, on produit par incrémentations courtes.

![05-Cycles semi-itératifs](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_05-Cycles%20semi-it%C3%A9ratifs.png)

Avantages
•	adapté aux environnements changeants
•	retours rapides
•	flexibilité
Limites
•	moins de documentation
•	nécessite une bonne maturité d'équipe
•	difficile à mettre en place pour les grands projets

### DevOps
Fusion des activités Dev et Ops pour livrer plus vite et mieux.

Note : Ce n'est pas vraiment de la gestion de projet, mais les principes sont très utilisés en Agile

![06-DevOps](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_06-DevOps.png)

### Piliers
•	automatisation
•	intégration continue
•	supervision
•	culture collaborative

DevOps n'existe pas sans une chaîne outillée cohérente : CI/CD, laC, monitoring, automatisation.
Avantages
•	qualité constante
•	rapidité
•	réduction du risque humain
Limites
•	nécessite outils et culture
•	pas adapté à tous les environnements
•	mise en place peut être complexe

## Conclusion
Un projet est une démarche organisée qui répond à un objectif unique dans un temps défini avec des ressources limitées.

L'IT demande une gestion rigoureuse parce que les projets sont complexes, impliquent plusieurs équipes, doivent garantir la disponibilité et la sécurité et évoluent très vite.

Une bonne gestion de projet permet de réduire les risques et d'avoir plus de réussite.

Gérer un projet, ce n'est pas seulement faire un planning. C'est anticiper, communiquer, documenter, et mener une équipe vers un objectif clair.

# Cycle en V
C'est quoi, le cycle en V ?
C'est un modèle d'organisation de projet qui améliore le cycle en cascade (waterfall).

![07-Cycle en V dévéloppé](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_07-Cycle%20en%20V%20d%C3%A9v%C3%A9lopp%C3%A9.png)

On retrouve les étapes classiques d'un projet, ces étapes peuvent avoir plusieurs sous-étapes :

* 📝**Définition** : savoir ce que veut le client
*	🧠**Conception** : savoir ce que l'on va faire
*	🔨**Réalisation** : faire ce que l'on doit faire
*	✅**Validation** : le résultat répond-il au besoin (ce qui n’est pas testé ne marche pas)
*	📦**Livraison** : fournir les résultats du projet

Cas d'usages généraux :

*	développement logiciel
*	industrie lourde (automobile, aviation, militaire)
*	certains projets IT / infrastructures

Dans l'IT, s'applique bien pour :

*	les migrations ou déploiements d'infrastructures (refonte d'un LAN, migration d'un cœur de réseau)
  - nécessite une bonne analyse du besoin, du mode opératoire, des tests complets et une validation stricte (latence, débit, redondance ... )
*	mise en place d'une infrastructure virtualisée
  - l'architecture doit être définie avant construction, intégration complexe, tests indispensables
* déploiement ou migration de solutions de téléphonie / VolP
  - contraintes importantes, tests indispensables avant bascule, déploiement par lots

Le cycle en V **fonctionne bien** pour l’infrastructure car :

| **Caractéristique** | **Pourquoi c’estidéal pour l’infra ?** |
| --- | --- |
| **Prévisibilité** | Matériel, délais de livraison, compatibilité connue |
| **Forte dépendance technique** | Nécessité d’un design préalable |
| **Faible incertitude fonctionnelle** | Besoins stables, peu changeants |
| **Tests structurés** | Recettes techniques et intégrations formelles |
| **Environnements complexes** | Préparation nécessaire pour minimiser les interruptions |

Globalement bonne méthode projet quand on a le besoin de :

•	planification stricte
•	documentation complète
•	validation et tests très cadrés
•	peu d'allers-retours fonctionnels

En pratique
Le cycle en V se déroule sous forme d'étapes, utilise des outils et chaque acteur du projet a un rôle.
Les acteurs / les rôles MOA

Maîtrise d'ouvrage, c'est le client (le demandeur du projet). Ses responsabilités :

•	détermine les objectifs du projet
•	estime et alloue un budget
•	fixe les délais de livraison
•	anime les différentes réunions
•	valide les étapes du projet
•	participe à la recette

La MOA connaît le métier, donc le besoin.

MOE
Maître d'œuvre, charge de la conception et de la réalisation du projet. Ses responsabilités :

•	assiste la MOA
•	sélectionne et gère l'équipe de réalisation
•	assure la qualité du produit
•	rend compte de l'avancement
•	tient les délais et le budget

La MOE connaît la technique.
AMOA / AMOE
Sur des projets complexes, chacune des parties peut être accompagnée par un intermédiaire chargé de fluidifier les relations et d'analyser en détail les contraintes, les risques ...
•	AMOA : assistant à maîtrise d'ouvrage
•	AMOE : assistant au maître d'œuvre

Dans une petite structure, petite DSI, classiquement :

•	MOA : direction / métier / responsable informatique
•	MOE : admins / ingénieurs / prestataires

Au sein de chaque partie, on peut définir des rôles spécifiques, dont :

•	sponsor : référent qui représente le client, fait le lien entre l'équipe projet et le client
•	responsable fonctionnel : chargé de la définition du projet
•	chef de projet : en charge de coordination, planification, du suivi
•	administrateur / ingénieur : réalise les installations, configurations, tests
•	RSSI : gère les aspects sécurité informatique et conformité
•	utilisateurs finaux : réalisent la recette, la validation

Une équipe bien structurée, avec des rôles et responsabilités bien établis, connus, acceptés et compris permet d'avancer sereinement.

L'ensemble des acteurs est appelé parties prenantes.
Les étapes et outils
Avant le projet
Avant le démarrage du projet, l'entreprise peut commencer à le définir de façon "macro" :

expression du besoin

•	le but / la raison d'être du projet
•	le contexte
•	le périmètre, entre autres, dates de début et fin du projet les délais et jalons clés les acteurs du projet
•	les contraintes et risques potentiels
•	les impacts éventuels (par exemple sur l'entreprise)
MOE
Maître d'œuvre, charge de le conception et de la réalisation du projet. Ses responsabilités :

· assiste la MOA
· sélectionne et gère l'équipe de réalisation
· assure la qualité du produit
. rend compte de l'avancement
· tient les délais et le budget

La MOE connaît la technique.

Avant le démarrage du projet, l'entreprise peut commencer à le définir de façon "macro" :

étude d'opportunité

•	analyse de la situation (stratégie de l'entreprise, facteurs clés de succès ... )
•	le contexte
•	les résultats attendus
•	les opportunités à saisir (nouveaux marchés ... )
•	options envisageables / scénarios
•	recommandations (pistes) issues de l'étude

Avant le démarrage du projet, l'entreprise peut commencer à le définir de façon "macro" :

étude de faisabilité

•	qualité à atteindre
•	délais à respecter
•	coûts
•	ressources et contraintes :
•	techniques
•	financières
•	temporelles

Avant le démarrage du projet, l'entreprise peut commencer à le définir de façon "macro" :

•	expression du besoin : ce que l'on veut
•	étude d'opportunité : pourquoi, ce que ça apporte
•	étude de faisabilité : est-ce réalisable
Livrable : note de cadrage

Cette phase est plus ou moins détaillée selon l'entreprise, le projet ...

La méthode QQOQCCP (Qui ? Quoi ? Où ? Quand ? Comment ?
Combien ? Pourquoi ?) peut aider lors de cette phase.

La note de cadrage peut contenir :

•	le but du projet
•	le contexte
•	le périmètre (ce qui est inclus et exclu du projet)
•	les acteurs identifiés
•	les contraintes
•	les risques (souvent au niveau de l'organisation plus que du projet)
•	les opportunités
•	les impacts

C'est un document d'avant-projet. Il permet d'avoir une base de travail pour le projet et ses orientations.

Définition du projet
Les MOA et MOE définissent plus précisément le projet en s'appuyant sur les parties prenantes.

Le but est de définir précisément :

•	les objectifs
•	la reformulation des besoins
•	l'analyse des risques
•	la planification
•	le budget
•	le cahier des charges en tant que document de synthèse
Définition du projet : les objectifs

Le but est de définir précisément :

les objectifs

•	prise en compte de la note de cadrage
•	échanges avec la direction
•	ateliers de réflexion
Définition du projet : reformulation des besoins

Le but est de définir précisément :

la reformulation des besoins

•	interview du client
•	ateliers de réflexions et d'idéation
•	observations sur le terrain

Définition du projet : analyse des risques

Le but est de définir précisément :

l'analyse des risques

•	évaluation des risques du projet
•	évaluation des critères de limitation des risques
•	matrice des risques

![08-Matrice de criticité](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C101/C101_08-Matrice%20de%20criticit%C3%A9.png)

### Définition du projet : planification

Le but est de définir précisément :

la planification

* [PBS](https://www.tldraw.com/) : découpage du produit (comprendre l’information telle qu’elle est) : pour désigner le problème
*	WBS : découpage des tâches du projet (ce qui sera fait)
*	matrice RACI : rôles et responsabilités des parties prenantes (qui fait quoi)
*	diagramme de Gantt : durée des tâches, enchaînement des tâches, jalons, assignation des personnes (qui fait quoi et quand)

### Définition du projet : le budget

Le but est de définir précisément :

le budget

•	budget détaillé
•	indicateurs de suivi

Définition du projet : le cahier des charges

Le but est de définir précisément :

le cahier des charges (contrat)

•	document exhaustif de définition du projet
•	livrable qui constitue le contrat entre le client et le fournisseur

Le cahier des charges est :

•	un document
•	un modèle de clarté et de précision
•	un contrat entre le client et le fournisseur

Conception du projet
Définition de toutes les informations nécessaires à la réalisation du projet. Le détail de la conception est très spécifique au domaine du projet !

On peut y trouver plusieurs livrables :

•	spécifications fonctionnelles
•	spécifications techniques
•	maquettage ou prototypage
Conception du projet : spécifications fonctionnelles

On peut y trouver plusieurs livrables :

spécifications fonctionnelles

•	description détaillée du besoin
•	cas d'usage / cas d'utilisation
•	scénarios utilisateurs / matériel
•	exigences de sécurité
•	exigences de performance
Conception du projet : spécifications techniques

On peut y trouver plusieurs livrables :

spécifications techniques

•	architecture détaillée (schéma réseau, système, de stockage ... )
•	choix techniques et justifications (tel firewall parce que ... )
•	caractéristiques matérielles (processeur, serveur, RAM)
•	caractéristiques logicielles (OS, versions minimales)
•	configuration réseau détaillée
•	configuration sécurité détaillée

Conception du projet : maquettage ou prototypage

On peut y trouver plusieurs livrables :

maquettage ou prototypage, qui permet de :

•	valider une architecture avant déploiement réel
•	tester une migration sans impacter la production
•	comparer plusieurs solutions techniques
•	vérifier les performances attendues
•	tester un plan de reprise ou de bascule
Réalisation du projet
On réalise le besoin. Cette phase dépend du secteur d'activité.

Là, c'est le cœur du métier !

On n'oublie pas de documenter ce que l'on fait ni d'appliquer les bonnes pratiques.

Validation du projet

On vérifie que le résultat correspond au besoin. Dépend du domaine
d'activité.

On peut y trouver les livrables :

· validation technique
· tests de charge / de performance / d'exploitation / de sécurité
· cahier de recette

Validation du projet

Les tests dépendent de la nature du projet. On peut y retrouver :

•	tests fonctionnels : valident la conformité par rapport aux spécifications fonctionnelles
•	création de compte AD
•	connexion VPN
•	accès réseau selon profil

•	tests techniques : valident le comportement technique de l'infrastructure
•	performance du stockage
•	test haute disponibilité
•	latence réseau

•	tests de sécurité : valident la conformité du système
•	tests d'authentification
•	scans vulnérabilités
•	contrôle des droits NTFS / ACL
Validation du projet
•	Le cahier de recette peut inclure :
•	plan de recette (tests)
•	scénarios de test
•	jeux de données
•	prérequis (réseaux, comptes, droits)
•	règles de validation
•	gestion des anomalies

Livraison du projet
On fournit le produit au client. Dépend du domaine d'activité.

On peut y trouver les livrables :

•	dossier d'architecture final (DAF)
•	dossier d'exploitation
•	manuel d'administration
•	documentation de bascule (pour PRA/PCA)
•	planning de maintenance
•	scripts, fichiers de configuration, supervision
REX
Lorsque le projet est terminé, il est intéressant de faire un retour d'expérience.

Il ne s'agit pas uniquement de se féliciter (ou l'inverse), mais surtout d'analyser ce qui s'est bien déroulé, mal dérouler et d'en comprendre les causes et les conséquences.

Le plus important c’est de retenir ce qui a été mal fait et comprendre le pourquoi, tout documenter. L'objectif est de s'améliorer sur le prochain projet. 
Synthèse
Le cycle en V c'est :

•	prévisibilité
•	structure
•	documentation
•	tests
•	peu de flexibilité

Ce sont aussi ces limites :

•	peu adapté aux besoins changeants
•	beaucoup de documentation nécessaire
•	effet tunnel important
•	validation tardive
Conclusion
Le cycle en V est un modèle structuré, prévisible et rigoureux. Il est bien adapté aux projets dont le besoin est clair et stable, où les contraintes sont techniques et fortes et pour lesquels la qualité et la sécurité sont non négociables.

Il permet une bonne maîtrise des risques, une documentation complète et une vision claire pour toutes les parties prenantes.

Mais il implique aussi moins de flexibilité, plus de formalisme et des cycles qui peuvent être longs.

Manifeste pour le développement Agile de logiciels

Nous découvrons comment mieux développer des logiciels par la pratique et en aidant les autres à le faire. Ces expériences nous ont amenés à valoriser :

•	L'es individus et leurs interactions plus que les processus et les outils
•	Des logiciels opérationnels plus qu'une documentation exhaustive
•	La collaboration avec les clients plus que la négociation contractuelle
•	L'adaptation au changement plus que le suivi d'un plan

Nous reconnaissons la valeur des seconds éléments, mais privilégions les premiers.
https://agilemanifesto.org/iso/fr/principles.html 

Pour challenge : La note de cadrage

contexte
objectifs
périmètres
inclus
exclus
partie prenante
Livrables principaux
Contraintes QCD (qualité, coût, délais
Méthode recommandée
Commentaire manifeste Agile






