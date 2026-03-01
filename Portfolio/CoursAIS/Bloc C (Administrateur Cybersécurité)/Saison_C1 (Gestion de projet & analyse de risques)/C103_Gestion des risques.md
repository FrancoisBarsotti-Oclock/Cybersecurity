# Session C103. Gestion des risques & EBIOS RM.

Notions du jour
•	Matrices d’évaluation (probabilité x impact)
•	Méthode EBIOS RM Application

Qu'est-ce qu'un risque ?
C'est un événement incertain, qui s'il survient, a un impact négatif sur :

•	le planning
•	le budget
•	la qualité
•	la sécurité
•	la disponibilité du SI

Le risque n'est pas un problème : c'est un problème potentiel.
Composantes
Le risque est composé de trois composantes :

•	la cause : l'origine possible
•	l'événement : ce qui pourrait se produire
•	la conséquence : l'impact sur le projet

Par exemple :
•	Cause : matériel vieillissant
•	Événement : panne du serveur
•	Conséquence : interruption du projet
Risque et incertitude
Le risque peut être mesuré, il peut être déterminé, analysé, prévisible. C'est un événement identifié.

L'incertitude signifie qu'il n'y a pas assez d'informations pour analyser, pour quantifier.

Par exemple l'usage d'une nouvelle technologie, mal maîtrisée, mal connue, sans retour d'expérience ne peut pas être maîtrise. Il y a donc une incertitude forte qui engendre un risque élevé.

Types de risques
Il y en a plein !
Risques organisationnels
•	retard fournisseurs
•	dépendances non gérées
•	changement d'exigences
Risques humains
•	absence d'un expert
•	erreurs de configuration
•	mauvaise communication
Risques techniques
•	panne matérielle
•	incompatibilité technologique
•	faille de sécurité
Risques extérieurs
•	cybersécurité
•	réglementation
•	coupure électrique / réseau
Méthodes
•	brainstorming (équipe projet)
•	retours d'expérience (REX/ incidents passés)
•	analyse documentaire (architecture, planning, literature ... )
•	checklists de risques IT (ANSSI, ISO 27001, ITIL ... )
•	interviews des parties prenantes
•	analyse par scénarios ("et si le serveur tombe en panne ?")

Idéalement combiner les méthodes et impliquer les parties prenantes pour avoir différents angles.
Catégories de risques dans un projet IT
•	matériel : serveurs, stockage, réseau
•	logiciel : OS, compatibilité, mises à jour
•	sécurité : attaques, vulnérabilités
•	planning : retard, dépehdances
•	budget : dépassement
•	ressources : disponibilité des compétences
•	conformité / réglementation
•	fournisseurs / prestataires

Exemples de risques courants en infrastructure
•	perte de données
•	retard livraison baie de stockage
•	configuration erronée du VLAN
•	erreur de firewall - coupure réseau
•	incompatibilité AD / application métier
•	cyberattaque pendant migration
•	absence expert pendant MEP
Bonnes pratiques
•	impliquer toutes les compétences
•	croiser technique / fonctionnel / sécurité
•	analyser tôt dans le projet
•	mettre à jour régulièrement
Évaluer les risques
Probabilité - Impact - criticité
On évalue un risque selon trois dimensions :

La probabilité : estimation de la fréquence d'apparition
L'impact : la conséquence sur le projet si le risque se produit
La criticité : importance d'un risque pour le projet
Probabilité
Pour chaque risque on doit analyser sa probabilité, l'estimation de sa fréquence d'apparition.

On classe la probabilité sur une échelle (qui dépend du projet). Par exemple :

•	1 : faible
•	2 : moyenne
•	3 : forte

Ex : qu'elle est la probabilité que le serveur tombe en panne ? Il est ancien, a beaucoup tourné : 3.
Impact
Pour chaque risque on doit analyser son impact, la conséquence sur le projet si le risque se produit.

On classe l'impact sur la même échelle que la probabilité :
•	1 : faible
•	2 : moyenne
•	3 : forte

Ex : si le serveur tombe en panne, le projet s'arrête le temps d'avoir un nouveau serveur opérationnel. Donc dérive en temps et en coût : 3.
Criticité
On applique généralement la formule de calcul de la criticité : criticité = probabilité x impact

Ex : pour le risque de la panne du serveur, criticité = 9 (ici sur une échelle de 1 à 9)
Matrice du risque
On rapporte ces données sur une matrice :

![01-Matrice du risque](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/images%20C1/images%20C103/C103_01-Matrice%20du%20risque.png)

•	vert : risque acceptable
•	orange : risque à surveiller
•	en rouge : risque à action immédiate

Ex : avec une criticité de 9, notre risque mérite une action immédiate.
Conclusion
L'identification et l'évaluation des risques est une étape essentielle pour anticiper les problèmes potentiels et sécuriser le projet.

Il ne faut pas sous-estimer l'identification des risques, ne pas hésiter à combiner plusieurs méthodes et impliquer les toutes parties prenantes.

Cette approche permet de limiter les dérapages du projet et de sécuriser le produit final si, une fois identifiés et évalués, les risques sont documentés et traités.
Documenter et traiter les risques
Pourquoi ?
Les risques identifiés et évalués doivent être documentés, afin d'en garder trace, de pouvoir suivre l'évolution de leur gestion, et traités si nécessaire.
Documenter les risques…
La documentation des risques permet de centraliser, suivre et piloter les risques d’un projet. Elle est essentielle pour anticiper les problèmes et sécuriser l’avancement.
Registre des risques
Le registre des risques est un document central qui regroupe tous les risques.
Il permet d'avoir une vision claire des risques identifiés, de les prioriser et de suivre leur évolution. C'est donc un document clé pour assurer la traçabilité des risques. Il sert également d'outil de communication.
Le registre des risques est composé de fiches de risques.
Fiche de risque
La fiche de risque donne le détail d'un risque. On y trouve généralement :

•	identifiant
•	description
•	catégorie (technique, organisationnel, planning, sécurité ... )
•	causes
•	conséquences
•	probabilité
•	impact
•	criticité
•	actions prévues
•	responsable
•	statut
•	date de mise à jour

Exemple :

| **Champ** | **Contenu** |
| --- | --- |
| ID | 124 |
| Description | Erreur de configuration firewall |
| Catégorie | Technique |
| Cause | Manque de double vérification |
| Conséquence | Coupure réseau |
| Probabilité | 2 |
| Impact | 3 |
| Criticité | 6 |
| Action | Procédure d’installation + revue pair |
| Responsable | Admin réseau |
| Statut | En cours |
| Date màj | 28/11/2025 |

Bonnes pratiques
•	utiliser un outil collaboratif
•	mettre à jour après chaque réunion projet
•	affecter un responsable unique par risque
•	être concret et éviter les formulations floues
•	prioriser le registre
•	intégrer le registre dans le tableau de bord projet
•	conserver l'historique
Séparation des risques
Dans un projet IT, séparer :

•	risques projet (planning, budget, ressources)
•	risques sécurité (vulnérabilités, menaces, conformité)

Car les risques projet s'arrêtent avec le projet alors que les risques de sécurité doivent être communiqués et connus pour la maintenance.
Traiter les risques
Les risques importants doivent être évites. Il faut donc les traiter !
Avant la prise de décision
L'évaluation du risque permet de le classer par sa criticité.

Si le risque est important, on peut estimer le cout de la gestion du risque.

Quels sont les besoins humains, matériels, immatériel pour gérer le risque ? Cela permet d'avoir une connaissance du cout de la gestion du risque.

Ce coût peut être comparé au coût de la non-gestion du risque. Quelles sont les pertes si le risque n'est pas géré mais réalisé ?

Le résultat de cette évaluation permet de choisir la stratégie de traitement.
Stratégies de traitement
Les stratégies de traitement des risques sont définies par les normes

[ISO 27005](https://fr.wikipedia.org/wiki/ISO/CEI_27005) : Gestion des risques dans le contexte de la Sécurité des systèmes d'information

[ISO 31000](https://fr.wikipedia.org/wiki/ISO_31000)  : Normes de gestion des risques
Stratégie 1 : suppression du risque
Si le risque est inacceptable pour l'organisation et ses impacts importants, alors il convient de le supprimer.

Selon la nature du risque, cela peut-être plus ou moins complexe.

Éventuellement la suppression d'un risque peut amener à l'annulation du projet !

Ex : remplacer un serveur trop ancien avant le démarrage des phases techniques.
Stratégie 2 : réduction du risque
Cette stratégie consiste à mettre en place des mesures qui permettent de diminuer la survenance de l'événement.

Des moyens humains et financiers doivent donc être engages pour rendre ce risque acceptable.

Ex : documentation, tests supplémentaires, redondance matérielle.
Stratégie 3 : partage du risque
Le risque peut être partagé avec un tiers. Sa gestion est donc assurée par les deux parties, il y a une responsabilité conjointe.

Ce tier peut-être un sous-traitant, un prestataire, une assurance.

Cela nécessite de la confiance et une bonne coordination.

Ex : infogérance partielle
Stratégie 4 : transfert du risque
Le risque est confié à un tiers capable de gérer ou d'accepter le risque. La responsabilité est donc déléguée à ce tiers.

Ce tier peut-être un sous-traitant, un prestataire, une assurance.

La responsabilité du risque est donc déplacée sur ce tiers.

Ex : assurance, outsourcing
Stratégie 5 : acceptation du risque
Un risque ne peut être ni supprimé, ni atténué, ni partagé, ni transféré.

C'est généralement un risque faible pour lequel le coût de traitement peut être supérieur aux pertes.

On accepte donc volontairement ce risque et ses conséquences.

Attention à bien continuer à surveiller le risque : accepter ce n'est pas ne rien faire ! On peut prévoir des actions de monitoring, de communication ...
Plan de traitement
Lorsque la stratégie de traitement du risque est décidée, selon la stratégie choisie il faut :

•	décrire les mesures à mettre en place 
•	affecter un responsable du traitement du risque
•	fixer un délai de mise en œuvre
•	évaluer le coût
•	affecter les ressources nécessaires
•	mettre en place les indicateurs de suivi

Mise à jour de la documentation
Lorsque le traitement du risque est réalisé, il faut recalculer la criticité pour vérifier l'efficacité et mettre à jour les documents.
Conclusion
Bien identifier un risque n'est pas suffisant : il faut le documenter, l'évaluer, choisir une stratégie de traitement, et assurer le suivi dans le temps.

Le registre est un véritable outil d'aide à la décision qui permet d'anticiper plutôt que de subir.

Un projet comporte toujours des risques. Ils doivent donc être visibles, priorisés et pilotés.

# EBIOS RM

[EBIOS RISK MANAGER](https://messervices.cyber.gouv.fr/documents-guides/250129_np_anssi_guide_ebios_fr_final_collection_WEB.pdf)

C'est quoi ?
EBIOS RM signifie Expression des besoins et identification des objectifs de sécurité - Risk Manager.

C'est une méthode d'évaluation des risques en informatique gérée par l'ANSSI.

Cette méthode permet d'identifier tous les types de risques d'un système d'information.

Elle est compatible avec [ISO 27005](https://fr.wikipedia.org/wiki/ISO/CEI_27005) et ISO 31000 (https://fr.wikipedia.org/wiki/ISO_31000).
La méthode [est disponible](https://cyber.gouv.fr/actualites/lanssi-met-%C3%A0-jour-la-m%C3%A9thode-ebios-risk-manager/) sur le site de l'ANSSI. 
Objectifs
Identifier les risques informatiques essentiels

Raisonner par scenarios de menace

Déterminer les mesures de sécurité prioritaires

Construire une vision partagée entre équipes techniques / direction

Étapes
EBIOS RM est une méthode permettant de construire une politique de sécurité en fonction de l'analyse des risques.

Cette méthode est basée sur 5 étapes (ateliers) découpées en activités.

Ces ateliers concernent les différents acteurs de l'entreprise ou du projet.
Étape 1 : cadrage et socle de sécurité
•	Objectif : définir le cadre de l'étude, son périmètre, les événements redoutés les besoins de sécurité.
•	Participants : direction, métiers, RSSI, DSI
•	Activités :
•	définir le cadre de l'étude
•	définir le périmètre métier et technique de l'objet étudié
•	identifier les événements redoutés et estimer leur niveau de gravité
•	déterminer le socle de sécurité et en évaluer la conformité

Il s'agit de définir le système étudié, les acteurs et les contraintes et objectifs.
Étape 2 : sources de risque
•	Objectif : identifier les sources de risque et leurs objectifs visés
•	Participants : direction, métiers, RSSI, spécialiste en analyse de menaces
•	Activités :
•	identifier les sources de risque et les objectifs visés
•	évaluer la pertinence des sources de risque et objectifs visés
•	sélectionner les couples sources de risque / objectifs visés jugés prioritaires

Il s'agir de d'identifier les attaquants, leurs motivations et leurs capacités.
Étape 3 : scénarios stratégiques
•	Objectif : disposer d'une vision claire de l'écosystème, afin d'identifier et de présenter les parties prenantes les plus menaçantes
•	Activités :
•	construire la cartographie de dangerosité des parties prenantes de l'écosystème et sélectionner les parties prenantes critiques
•	élaborer des scénarios stratégiques
•	définir des mesures de sécurité sur l'écosystème
•	Participants : métiers si besoin, services liés aux contrats (achats, juridiques), architectes fonctionnels, RSSI, spécialiste cyber

Il s'agit d'imaginer des scénarios d'attaque crédibles au niveau organisationnel et les mesures de sécurité.
Étape 4 : scénarios opérationnels
•	Objectifs : construire des scénarios opérationnels
•	Participants : RSSI, DSI, spécialiste cyber
•	Activités :
•	élaborer les scénarios opérationnels
•	évaluer leur vraisemblance

Il s'agit d'analyser les modes opératoires techniques (vulnérabilités, points d'entrée) et d'évaluer leur possibilité.
Étape 5 : traitement du risque
•	Objectif : réaliser une synthèse des scénarios de risque identifiés et de définir une stratégie de traitement du risque
•	Participants : direction, métiers, RSSI, DSI
•	Activités :
•	réaliser la synthèse des scénarios de risque
•	décider de la stratégie de traitement du risque
•	définir les mesures de sécurité
•	évaluer et documenter les risques résiduels
•	mettre en place le cadre de suivi des risques
•	mettre en place des mécanismes de surveillance

Il s'agit de définir et prioriser les mesures de sécurité.

exemple de rapport ebios : https://1drv.ms/b/c/192b0a3c2c138ff7/IQAmEumGIVZKTqoOqzqGtMQyAVlGaGq-5pQpBem980PfcEI?e=x8GCCQ

EBLOS RM dans un projet
EBIOS RM est orienté SI mais peut-être utilisée lors de gros projets.

Cas d'usage en projet :

•	identifier les risques liés aux nouveaux processus, applications ou infrastructures
•	intégrer la sécurité dès la conception
•	se conformer à des exigences réglementaires ou normatives (RGPD, ISO 27001)
•	sécuriser un projet sensible ou critique
•	réévaluer les risques en fonction d'un changement majeur induit par le projet
Quand ne pas utiliser EBIOS RM
Pour des projets très simples ou à très faible risque, une analyse rapide, une checklist ou une version "light" peut suffire car EBIOS RM demande du temps, de la rigueur et la participation de nombreux acteurs.
Conclusion sur EBIOS RM
EBIOS Risk Manager est une méthode complète et structurée permettant de comprendre qui peut attaquer, pourquoi, et comment, afin de décider quelles mesures de sécurité mettre en
place de manière prioritaire.

Même si elle demande du temps et la mobilisation de plusieurs acteurs, EBIOS RM apporte une rigueur et une traçabilité indispensables pour les projets critiques ou les systèmes sensibles.
#
