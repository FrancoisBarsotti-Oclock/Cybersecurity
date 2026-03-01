# Session C104. Sécurité & Continuité d'activité.

Notions du jour
•	Sécurité des systèmes d'information dans les projets Menaces, vulnérabilités et bonnes pratiques
•	Plan de Continuité d'Activité (PCA)
•	Plan de Reprise d'Activité (PRA)
•	Sauvegardes, redondance et supervision
Projet et SI
Nos projets s'inscrivent souvent dans le cadre plus général d'un système d'information.

Nous devons donc prendre en compte les menaces et vulnérabilités des SI dans l'apport de nos projets.
Système d'information
Un système d'information (SI) est l'ensemble des personnes, des outils et des processus qui permettent de collecter, stocker, traiter et transmettre l'information au sein d'une organisation.

Il comprend donc les personnes (direction, utilisateurs, techniciens, administrateurs ... ) et le système informatique (postes utilisateurs, serveurs, infrastructure, logiciels).
Identifier les menaces et vulnérabilités SI
Définitions
Menace : evenement ou action potentielle qui pourrait nuire au SI (cyberattaque, erreur humaine, ... )
Vulnerabilite : faiblesse du SI pouvant être exploitée par une menace (failles, mauvaise configuration, ... )
Incident : événement réel ayant un impact négatif (panne de serveur, ... )
Typologie des menaces IT
•	Humaines : erreur utilisateur, phishing, sabotage interne
•	Techniques : malware, ransomware, intrusion réseau
•	Physiques : panne électrique, incendie, inondation
•	Organisationnelles : absence de procédures, non-respect des politiques
•	Externes : législation, fournisseurs, événements naturels
Vulnérabilités courantes
•	failles OS
•	logiciels non mis à jour
•	configurations réseau incorrectes
•	absence de sauvegarde ou PRA incomplet
•	absence de supervision et monitoring
•	serveurs non redondés ou obsolètes
•	disques ou équipements de stockage non sécurisés
•	câblage et équipements réseau mal documentés ou non protégés
•	mots de passe faibles ou non changés
•	politique de mot de passe faible ou non appliquée
•	absence de chiffrement des données sensible
Que faire ?
Les listes des menaces et vulnérabilités sont longues !

On peut donc se référer à des bonnes pratiques et des référentiels.

On s'y réfère tant dans la réalisation d'un projet que lors de la maintenance des systèmes.

Bonnes pratiques de sécurité (ANSSI)
ANSSI
Autorité nationale de la sécurité des systèmes d'information. L'ANSSI vient de la création, en 1943, de la Direction technique du chiffre. Cette entité, créée par le Conseil de défense de la France libre, avait pour but de protéger les informations échangées et casser des codes ennemis.
La vocation militaire initiale a évolué vers une mission de défense des systèmes d'information de l'État et une mission de conseil et de soutien aux administrations et aux opérateurs d'importance vitale.
Elle émet recommandations, guides, labels accessibles à tous.
Quelques guides utiles
- [Guide d'hygiène informatique](https://gist.github.com/stephdl/7674ba6f66f9bbb408f2e6bfb25dc410) 

- [Anticiper et gérer une crise Cyber](https://gist.github.com/stephdl/0fd618ab6be5f4c56c319b1c32505e33) 

- [Recommandations relatives à l'authentification multifacteur et aux mots de passe]( https://messervices.cyber.gouv.fr/guides/recommandations-relatives-lauthentification-multifacteur-et-aux-mots-de-passe) 

- [Recommandations relatives à l’interconnexion d’un système d’information à internet]( https://messervices.cyber.gouv.fr/documents-guides/anssi-guide-passerelle_internet_securisee-v3.pdf)

Sécurité technique
•	Séparer les environnements (prod, test, dev)
•	Mettre à jour OS et logiciels régulièrement
•	Configurer firewall, segmentation réseau et VLANs
•	Sauvegarder et tester les backups
•	Mettre en place des comptes utilisateurs avec droits minimum (principe du moindre privilège)
•	Authentification forte (MFA, mot de passe complexe)
•	Journalisation et supervision des événements et incidents
Conception et planification projet
•	Intégrer la sécurité dès la conception (security by design)
•	Évaluer les risques sur chaque phase du projet
•	Définir des jalons de sécurité dans le planning (revue des vulnérabilités, tests de conformité)
•	Prendre en compte la continuité d'activité dans le planning (plan de reprise après incident, redondances)
•	Prévoir des environnements temporaires pour tests et validation sécurisée
Gestion des changements et développement
•	Validation des changements par revue de sécurité avant mise en production
•	Automatiser les déploiements pour réduire les erreurs humaines
•	Documenter les configurations, scripts et procédures
•	Suivre et tester les évolutions logicielles (patch management, versioning)
Pilotage et gouvernance
•	Identifier un responsable sécurité pour le projet (RSSI ou référent sécurité)
•	Communiquer régulièrement sur les risques et mesures avec l'équipe projet et la MOA
•	Définir les indicateurs de sécurité à suivre dans le tableau de bord projet (ex. % serveurs patchés, incidents détectés, disponibilité backups)
•	Former et sensibiliser les équipes projet aux bonnes pratiques de sécurité
Fournisseurs et services externes
•	Évaluer la sécurité des prestataires avant intégration (cloud, SaaS, services tiers)
•	Définir des [SLA]( https://fr.wikipedia.org/wiki/Service-level_agreement) de sécurité et de disponibilité contractuels
•	Suivre les fournisseurs et leurs mises à jour de sécurité
ISO 27001 : normes et cadre de management
https://messervices.cyber.gouv.fr/documents-guides/anssi-guide-admin_securisee_si_v3-0.pdf 
ISO 27001
Norme internationale de définition des exigences pour la mise en place d'un système de management de la sécurité de l'information (SMSI).

Objectif : protéger les actifs informationnels
Que propose l'ISO 27001 ?
La norme indique les phases :

•	d'établissement d'un SMSI
•	d'implémentation
•	de maintien
•	d'amélioration
Établissement
L'établissement du SMSI se déroule en 4 étapes :
•	définir la politique et le périmètre du SMSI
•	identifier et évaluer les risques liés à la sécurité et élaborer la politique de sécurité
•	traiter le risque et identifier le risque résiduel par un plan de gestion
•	choisir les mesures de sécurité à mettre en place
Maintien
Le maintien se fait en 4 étapes :

•	établir un plan de traitement des risques
•	déployer les mesures de sécurité
•	générer des indicateurs
•	de performance pour savoir si les mesures de sécurité sont efficaces
•	de conformité qui permettent de savoir si le SMSI est conforme à ses spécifications
•	former et sensibiliser le personnel
Amélioration
Le système d'information doit continuellement amélioré par :
•	des actions correctives : agir sur les problèmes et les corriger
•	des actions préventives : agir sur les causes avant que l'incident ne se produise
•	des actions d'amélioration : amélioration la performance du système
Sauvegardes, redondance et supervision
Dans le cadre d'un projet
Si le système d'information sur lequel porte le projet est géré par un SMSI, il faut s'y conformer.

On doit, entre autres :
•	définir politique de sécurité en lien avec le SMSI
•	identifier les actifs critiques (serveurs, données, NAS)
•	évaluer les risques cyber
•	définir et suivre les mesures de sécurité
Conclusion
Dans le cadre d'un projet, il est important de prendre en compte les bonnes pratiques dès le début du projet (security by design).

À terme le produit pourra intégrer un système complexe, il ne doit pas compromettre ce système.
PCA / PRA
Comment poursuivre l'activité d'une organisation lorsqu'un système ne fonctionne plus ?

Comment reconstruire et remettre en route le système défaillant ?

Ce sont les rôles des PCA et PRA.

PCA (plan de continuité d'activité) : Maintenir les activités critiques malgré un incident

PRA (plan de Reprise d'Activité) : Reprendre l'activité après un incident majeur, restauration technique.
PCA
Un plan de continuité d'activité a pour but de garantir la survie de l'organisation en cas de sinistre important touchant le système informatique.

Le PCA est un élément essentiel de la politique de sécurité informatique.
PCA : mise en place
•	identifier les activités critiques
•	définir les objectifs de continuité
•	établir un plan d'organisation humaine et des responsabilités
•	composition et rôles des équipes de pilotage du plan de reprise direction, communication, coordination, équipes opérationnelles
•	préparer les procédures de mises en œuvre de la stratégie
•	préparer les procédures de reprise des services essentiels
•	elles doivent être accessibles quel que soit le sinistre
•	former et sensibiliser les équipes
•	tester et mettre à jour régulièrement
PCA : exercices et maintenance
Le plan doit être testé régulièrement.

Ça peut être via une revue des procédures ou par des exercices de simulation.

Dans tous les cas, il faut :

•	vérifier que les procédures permettent d'assurer la continuité d'activité
•	vérifier que le plan est complet et réalisable
•	maintenir un niveau de compétence suffisant parmi les équipes de pilotage
•	évaluer la résistance au stress des équipes de pilotage

Le PCA doit être vérifié au moins une fois par an et mis à jour si nécessaire (au niveau de DSI).
PRA
Un plan de reprise d'activité est un ensemble de procédures (techniques, organisationnelles, sécurité) permettant à une organisation de prévoir les mécanismes pour redémarrer un système
après un sinistre ou un incident critique.

Alors que le PCA permet de poursuivre l'activité, le PRA permet de reconstruire tout ou partie système.

Les deux sont donc complémentaires.
PRA : mise en place

•	définir le périmètre savoir quoi protéger et pourquoi
•	identifier les risques et scénarios de sinistre
•	définir les objectifs de reprise (RTO & RPO)
•	RTO (Recovery Time Objective) : délai maximum pour restaurer le service
•	RPO (Recovery Point Objective) : perte de données maximum acceptable
•	inventaire des ressources et dépendances savoir ce qu'il faut restaurer et dans quel ordre
•	définir les stratégies de reprise
•	rédiger les procédures de reprise
•	tester et valider le PRA
PRA : tester et ajuster
Certains éléments constitutifs du PRA doivent être testés régulièrement.

Par exemple la sauvegarde et la restauration des données doivent fonctionner en cas sinistre. Il faut donc vérifier ces éléments régulièrement, et ajuster la mise en cas de dysfonctionnement.

Le PRA doit également être mis à jour après toute modification du SI (migration, modification de l'infra, changement de contacts, de mot de passe ... )
Types de sinistres informatiques
•	matériels : incendie (extinguer avec du gas), inondation (mettre tout sur pilori, rondes contrôle de fuites), panne de courant 
•	techniques : panne serveur (faut un cluster, redondance), crash disque (faire du RAID 5), erreur config, perte réseau (appliance de plusieurs cartes réseau)
•	cyber : ransomware (former les users après restauration), intrusion, DDoS (se protéger de DDoS avec mod evasive de apache, honeypot, limiter les requêtes, filtrer par des entreprises qui offrent ce service -genre cloudflare).
•	humains : erreur (écrire de la doc), suppression accidentelle (gérer les droits)
•	organisationnels : absence procédures, dépendance personne
•	fournisseurs : panne cloud, rupture service
•	externes : grève, sabotage, catastrophes naturelles

Note : on peut également les classer en catastrophes naturelles, sinistres sur les installations et cybercriminalité et malveillance.
Conséquences
•	indisponibilité du SI
•	perte de données
•	arrêt de production
•	impacts financiers
•	atteinte à l'image
•	mise en conformité RGPD compromise

### Synthèse PCA / PRA

| **Aspect** | **PCA – Continuité d’activité** | **PRA – Reprise d’activité** |
| --- | --- | --- |
| Objectif | Maintenir les activités critiques en

fonctionnement | Restaurer l’IT et les services après

incident |
| Portée | Processus métier et organisation | Systèmes, applications, données |
| Temporalité | Pendant l’incident | Après l’incident |
| Exemple | Basculer sur un site secondaire pour
continuer les opérations | Restaurer les serveurs et bases de
données sur le site principal |

PCL / PRI
Dans certaines organisations, les PCA et PRA portent sur l'activité complète de l'organisation.

Dans ce cas pour le niveau informatique, on parle de Plan de continuité informatique (PCI) et Plan de reprise informatique (PRI) qui sont intégrés au PCA et PRA.

Prévenir

Pour prévenir des sinistres, il y a des actions à mettre en place de façon systématique :
-	sauvegarde et redondance des données
-	supervision des systèmes
Sauvegarde et redondance
Aucun système n'est infaillible : panne matérielle, ransomware, erreur humaine, corruption de données.

La seule manière d'assurer la continuité d'activité est de combiner sauvegardes et redondance, car elles répondent à deux objectifs différents.
Sauvegarde
C'est une copie des données à un instant T, stockée dans un emplacement distinct, permettant une restauration même en cas de destruction totale du système.
Sauvegarde : bonnes pratiques

•	règle 3-2-1 :
•	3 copies des données
•	2 supports différents
•	1 copie externalisée (hors site / cloud)
•	sauvegardes automatiques et régulières
•	tests de restauration réguliers (souvent oublié)
•	chiffrement des sauvegardes
•	rétention adaptée (journalière, hebdo, mensuelle, annuelle)
Sauvegarde : limites
•	pas de continuité immédiate : on restaure après coup
•	peut entraîner plusieurs heures / jours d'interruption
•	toujours dépendant de la qualité de la dernière sauvegarde
Redondance
La redondance consiste à dupliquer des ressources pour permettre au service de continuer même en cas de panne d'un élément.
Types de redondance
•	matérielle
•	serveurs en cluster (redondance)
•	disques RAID
•	alimentations redondées
•	liens réseau multiples

•	logicielle
•	load balancers
•	réplication de bases de données
•	containers multi-nœuds (Kubernetes, orchestrateur)

•	Géographique
•	datacenters (ou salles informatiques) multiples
•	cloud et site physique
Redondance : avantages et limites
Avantages

•	continuité immédiate
•	moins d'indisponibilités pour les utilisateurs
•	réduit fortement les incidents critiques

Limites

•	coûteux (matériel, licences, énergie)
•	complexité de gestion
•	n'évite pas la corruption de données (d'où besoin des sauvegardes !)
Sauvegardes et redondance : synthèse
Les sauvegardes protègent les données (3-2-1, tests, externalisation).

La redondance protège la disponibilité (cluster, RAID, double site).

L'un sans l'autre laisse un angle mort.

Un bon PRA/PCA repose sur les deux.
Supervision
La supervision consiste à surveiller en temps réel l'état et le fonctionnement du système d'information (serveurs, réseaux, applications, services).

Objectif : détecter rapidement les anomalies avant qu'elles ne deviennent des incidents.
Pourquoi superviser ?

Prévenir les pannes (disque plein, surcharge CPU ... )

Réduire les interruptions en détectant tôt les problèmes

Optimiser le SI (performances, capacité, ressources)

Aider au diagnostic grâce aux historiques

Améliorer la sécurité (détection d'activités anormales)

Que supervise-t-on ?
Technique
•	état des serveurs (CPU, RAM, disques, services)
•	réseau (bande passante, disponibilité, latence)
•	bases de données
•	applications / services métier
•	sauvegardes (succès/échecs)
Sécurité
•	tentatives d'intrusion
•	modifications suspectes
•	activité inhabituelle sur les comptes
•	événements issus des logs
Supervision : bonnes pratiques
•	définir un périmètre clair : qu'est-ce qu'on surveille ?
•	éviter le "bruit" : limiter les alertes inutiles
•	centraliser les journaux (SIEM, syslog, ELK ... )
•	vérifier régulièrement les seuils et les alertes
•	documenter les actions à réaliser en cas d'alerte
•	intégrer la supervision au PRA/PCA
•	tester périodiquement les alertes
Supervision : synthèse
La supervision permet de voir, comprendre et anticiper ce qui se passe dans le SI pour éviter les incidents et garantir la disponibilité.
Dans le cadre d'un projet ?
Un projet informatique s'inscrit toujours dans un système d'information existant.

Il doit donc respecter, renforcer ou étendre les mécanismes de continuité, de secours et de supervision déjà en place.
Projet et PCA
Objectif SI
maintenir les activités essentielles même en cas de crise majeure
Dans le projet
•	intégrer dès la conception les exigences de continuité (ex : cluster, redondance réseau, double site)
•	définir les niveaux de service attendus (RPO/RTO)
•	s'assurer que les choix techniques du projet sont compatibles avec le PCA existant
•	prévoir les procédures manuelles de continuité si des interruptions sont possibles
Projet et PRA
Objectif SI
redémarrer le système après un incident majeur
Dans le projet
•	documenter comment le nouvel équipement / logiciel sera restauré
•	prévoir les scripts, images, templates nécessaires au redémarrage
•	intégrer la nouvelle brique du projet dans :
•	le plan de reprise global
•	les tests PRA (simulation de panne)
•	vérifier que les dépendances (réseau, stockage DNS ... ) sont bien intégrées au PRA
Projet et sauvegardes
Objectif SI

restaurer les données en cas de corruption ou perte

Dans le projet
•	définir ce qui doit être sauvegardé :
•	données
•	configurations
•	machines virtuelles
•	bases de données
•	prévoir la stratégie de rétention
•	automatiser les sauvegardes
•	inclure des tests de restauration dans le planning du projet
Projet et redondance
Objectif SI
éviter les interruptions en éliminant les points critiques.
Dans le projet
•	identifier les SPOF (Single Point of Failure)
•	Décider :
•	redondance matérielle (alimentation, disques RAID ... )
•	redondance réseau (deux liens fibre, deux switches ... )
•	redondance applicative (cluster, load balancer)
•	intégrer la redondance dans le dimensionnement et le budget
•	tester le basculement avant la mise en production
Projet et supervision
Objectif SI
détecter rapidement les anomalies pour éviter les incidents
Dans le projet
•	définir ce qui doit être supervisé dans les éléments livrés
•	ajouter les nouveaux équipements dans la supervision existante
•	produire des seuils d'alerte, des graphes et des dashboards
•	transmettre au RUN l'ensemble des alertes et procédures
Conclusion
Dans un projet, PCA, PRA, sauvegardes, redondance et supervision ne sont pas des options : ce sont des exigences structurantes qui garantissent la continuité, la sécurité et la résilience du système d'information.

Ils doivent être prévu dès la conception, testes avant la mise en production, et intégrés entièrement au SI existant.



