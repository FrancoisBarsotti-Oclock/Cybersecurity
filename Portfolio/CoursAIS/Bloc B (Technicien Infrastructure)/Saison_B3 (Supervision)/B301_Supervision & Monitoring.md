# 🧐 Session B301. Supervision & Monitoring
_« Un outil pour les superviser tous »_ 🧐

### Notions du jour

* Supervision vs. Monitoring
* Intérêt de la supervision
* Métriques, alertes/seuils, logs
* Outils courants
* Méthodes de supervision

## 1. Introduction à la Supervision & Monitoring 🎯

### Qu'est-ce que la supervision ?
La supervision informatique consiste à surveiller en continu l'état des systèmes, des applications et des infrastructures. Elle permet de détecter les anomalies et d'assurer un fonctionnement optimal en évitant les interruptions de service.

### Différence entre supervision et monitoring 🔍

Le **monitoring** se concentre sur la collecte de données en temps réel pour suivre l'état des composants IT.

La **supervision**, quant à elle, analyse ces données pour prendre des décisions, déclencher des alertes et automatiser certaines actions correctives.

supervision, on rajoute la partie analyse suivi et action préventive ou correction, monitoring juste on récolte et on regarde

### Pourquoi superviser un système informatique ? ⚠️

Superviser un système permet d'identifier les problèmes avant qu'ils ne deviennent critiques. Cela améliore la réactivité des équipes IT, optimise l'utilisation des ressources et réduit les coûts liés aux interruptions de service.

Hors data, la température d'une salle serveur par exemple ça entre aussi dans ce cadre de monitoring et supervision.

### Les enjeux de la supervision 🔐

La supervision garantit la sécurité, la performance et la disponibilité des systèmes. Elle impacte directement la productivité des entreprises en minimisant les pannes et en assurant une expérience utilisateur fluide.

L'utilisateur ne s'attend pas à ce que la machine fonctionne correctement, il s'attend uniquement à ce qu'elle soit disponible.

### Historique et évolution des pratiques ⏳

Au fil du temps, la supervision est passée d'une simple surveillance manuelle à des solutions automatisées avancées utilisant l'intelligence artificielle et l'analyse prédictive.

Avant 2000, la supervision consistait uniquement à surveiller les systèmes en exploitation.

Entre 2000 et 2010, la supervision s'inspira des solutions de surveillance basées sur les logs.

Depuis 2010, la supervision s'inspira des solutions de surveillance basées sur les données en temps réel.

## 2. Les fondamentaux de la supervision 🏗️

### Les différentes couches supervisées 💻

La supervision s'effectue à plusieurs niveaux : le matériel (CPU, RAM, Disque), l'OS et les services système, les applications métiers et les infrastructures réseau. Une approche complète assure une meilleure visibilité sur l'ensemble du système.

→ Ce que vous faites tous avec vos ordinateurs, c'est surveiller la performance et la disponibilité de votre système.

### Supervision en temps réel ...

La supervision en temps réel permet de réagir immédiatement aux incidents en déclenchant des alertes instantanées.

Par exemple, si un utilisateur tente de s'identifier sur un compte sans avoir fourni son mot de passe, cela peut être déclenchant un incident en temps réel.

### ... Supervision différée ⏱️

La **supervision différée**, quant à elle, analyse les tendances et aide à comprendre les causes profondes des problèmes.

→ Elle permet de **déterminer les causes** des incidents, d'enregistrer les données et de les analyser.

Par exemple, des utilisateurs tentes de s'identifier sur leurs comptes a plusieurs reprises sans avoir fourni de mot de passe, et la supervision différée peut identifier les tendances et aider à comprendre les causes profondes des incidents.

## Notions d'indicateurs de performance 📊

Les **indicateurs clés de performance (KPIs)**, comme l'utilisation CPU, la mémoire disponible ou le temps de réponse, permettent d'évaluer l'état des systèmes.

Les **accords de niveau de service (SLAs)** définissent les seuils de performance acceptables.

### Les métriques essentielles 📡

Les principales métriques surveillées incluent le temps de réponse des applications, le nombre de connexions simultanées et l'utilisation des ressources système. Ces mesures permettent de détecter les anomalies et d'optimiser les performances.

### Types d'alertes et niveaux de criticité 🚨

Les alertes sont classées en trois catégories :

* **Alerte informative** : Simple notification pour suivi.
* **Alerte warning** : Problème potentiel nécessitant une attention.
* **Alerte critique** : Incident majeur demandant une intervention immédiate.

## 3. Modèles et stratégies de supervision 🔄

### Supervision proactive vs réactive

Une approche **proactive** permet d'anticiper les problèmes avant qu'ils n'affectent les utilisateurs. À l'inverse, une approche **réactive** consiste à intervenir après la survenue d'une panne.

### Approche basée sur les logs vs métriques 📜

La **supervision basée sur les logs** analyse les évènements enregistrés pour identifier des comportements anormaux. La **supervision basée sur les métriques** suit des indicateurs chiffres pour détecter des variations inhabituelles.

## Monitoring centralisé vs décentralisé 🏢🏠

Le **monitoring centralisé** (comme Zabbix) regroupe toutes les données dans un seul outil de supervision, tandis que le **monitoring décentralisé** (comme Nagios) distribue l'analyse entre plusieurs systèmes pour réduire la charge et améliorer la résilience.

### Corrélation d'événements et analyse de tendances 📈

L'analyse des évènements sur le long terme permet d'identifier des motifs récurrents et d'anticiper les incidents. La corrélation des événements aide à comprendre l'impact d'un problème sur l'ensemble du système, il faut de la data pour analyser de la data.

### Supervision en environnement cloud et hybride ☁️

Les architectures utilisent des environnements cloud et hybrides, ce qui complexifie la supervision. Il est essentiel d'adopter des solutions capables de surveiller des infrastructures distribuées. Par exemple, **Azure Monitor**.

## 4. Collecte et traitement des données 📡

### Méthodes de collecte des données ⏳➡️

Les données peuvent être collectées par **polling**, où le système interroge périodiquement les équipements, ou par **push**, où les équipements envoient automatiquement leurs informations en cas de changement. Nous allons faire du push dans nos cours.

### Protocoles utilisés 🔌

Plusieurs protocoles sont utilisés pour la supervision : **SNMP** pour la surveillance réseau, **Syslog** pour la gestion des logs et **API REST** pour récupérer des données en temps réel.

### Agrégation et normalisation des données 🔄

L'agrégation permet de consolider des données provenant de différentes sources. **La normalisation** assure une interprétation homogène, facilitant l'analyse des tendances.

### Stockage et rétention des données supervisées 

Les bases de données spécialisées, comme les **Time Séries Databases (TSDBs)**, permettent de stocker et analyser efficacement les métriques de supervision.

Voir sur [Timestamp]( https://en.wikipedia.org/wiki/Timestamp) (l’horodatage) 

Ainsi que [ses convertisseurs](https://www.timestamp.fr/) 

### Visualisation et reporting des performances 📊

Les tableaux de bord interactifs et les rapports d'analyse aident à interpréter les données et à prendre des décisions informées pour optimiser les performances.

## 5. Gestion des incidents et alerting 🚦

### Détection et classification des incidents ⚠️ 

Chaque incident est classe en fonction de son impact, ce qui permet de prioriser les interventions et d'optimiser les ressources des équipes IT (criticité).

### Gestion des seuils 📢

Définir des seuils pertinents est essentiel pour éviter les fausses alertes tout en assurant une réactivité adaptée aux incidents. C’est l’un des critères les plus importants dans le processus de monitoring. Il faudra les réévaluer régulièrement.

### Escalation et automatisation des alertes 🔁

Les systèmes de supervision avances automatisent certaines actions, comme le redémarrage d'un service défaillant ou la notification aux équipes concernées.

## 6. Bonnes pratiques et challenges 🔐

### Sécurité et protection des données supervisées 🔐

La supervision doit intégrer des mécanismes de chiffrement et de contrôle d'accès pour garantir la confidentialité des données.

### Supervision intelligente et évolutive 🚀
L'intelligence artificielle et le machine learning permettent d'améliorer la supervision en anticipant les incidents et en proposant des actions correctives automatiques.

## 7. Conclusion 🔮

La supervision et le monitoring sont des éléments clés pour garantir la performance et la disponibilité des infrastructures IT.

Avec l'évolution des technologies, l'automatisation et l'intelligence artificielle deviennent des leviers stratégiques pour une supervision efficace.

# SNMP - Simple Network Management Protocol

_"Le langage universel de la supervision réseau"_

## 1. Introduction à SNMP 📡

### Qu'est-ce que SNMP ? 🤔

**SNMP** (Simple Network Management Protocol) est un protocole standardisé permettant de superviser et gérer les équipements réseau à distance.

Il permet de collecter des informations sur l'état des équipements (routeurs, switches, serveurs, imprimantes ... ) et de les configurer à distance.

### Pourquoi SNMP ? 🎯

Sans SNMP, il faudrait se connecter manuellement sur chaque équipement pour vérifier son état !

SNMP permet une **supervision centralisée** et **automatisée** de l'ensemble de l'infrastructure réseau.

→ Imaginez devoir vérifier 100 switches un par un... 😰

### Les origines de SNMP ⏳

* **1988** : Création de SNMPv1 (RFC 1157)
* **1993** : SNMPv2c avec amélioration des performances
* **1998** : SNMPv3 avec ajout de la sécurité

Aujourd'hui, SNMPv2c reste le plus utilise malgré ses faiblesses de sécurité.

## 2. Architecture et composants SNMP 🏗️

### Les 3 composants principaux 🔧

1. **Manager SNMP** (superviseur) : Collecte et analyse les données
2. **Agent SNMP** : Installé sur les équipements supervisés
3. **MIB** (Management Information Base) : Base de données des informations disponibles

### Le Manager SNMP 👨‍💼

Le manager est le **serveur de supervision** qui :
* Interroge les équipements (polling)
* Reçoit les alertes (traps)
* Affiche les données collectées
* Génère des rapports

**Exemples** : [Zabbix]( https://www.zabbix.com/fr) (communautaire + open source), [PRTG](https://www.paessler.com/fr/lp/network-monitoring-tool-prtg?utm_term=prtg&utm_campaign=344772244&utm_content=&utm_source=google&utm_medium=cpc&utm_adgroup=24349420204&utm_device=c&gad_source=1&gad_campaignid=344772244&gbraid=0AAAAADmqWMiCJahuZQU9TSUyuicKGI4N7&gclid=CjwKCAjwjtTNBhB0EiwAuswYhm3oYYi8XY5pOjW9io1Ri9gEHPfvACuZGICVyBucOoVeIjAzOWqGPxoCHakQAvD_BwE), [Centreon]( https://www.centreon.com/fr/centreon-infra-monitoring/) (open source) et [Nagios]( https://www.nagios.org/) (gratuit).

### L'Agent SNMP 🤖

L'agent est un **logiciel installé** sur chaque équipement supervisé qui :

* Écoute les requêtes du manager
* Répond avec les informations demandées
* Envoie des alertes (traps) en cas de problème
* Peut modifier la configuration (si autorisé)

### La MIB (Management Information Base) 📚

La MIB est une **base de données hiérarchique** qui définit :

* Quelles informations peuvent être récupérées
* Comment elles sont organisées
* Leur type de données

Chaque information possède un **OID** (Object Identifier) unique.

### Exemple d'OlD 🔢

```nginx
1.3.6.1.2.1.1.1.0 = sysDescr (description du système)
1.3.6.1.2.1.1.5.0 = sysName (nom du système)
1.3.6.1.2.1.2.2.1.2 = ifDescr (description des interfaces)
```

→ Les OID suivent une structure arborescente standardisée ISO

Regarder la section qu'it-connect a dédié à ceci : 👉 [SNMPv2c vs SNMPv3 : comparaison de la sécurité avec Wireshark](https://www.it-connect.fr/snmpv2c-vs-snmpv3-comparaison-de-la-securite-avec-wireshark/)

## 3. Fonctionnement de SNMP 🔄

### Les opérations SNMP 📨

SNMP utilise principalement **5 opérations** :

1. **GET** : Récupérer une valeur
2. **GET-NEXT** : Récupérer la valeur suivante
3. **GET-BULK** : Récupérer plusieurs valeurs (v2c/v3)
4. **SET** : Modifier une valeur
5. **TRAP** : Alerte envoyée par l'agent

### GET Request 📥

Le manager **demande** une information spécifique :

```nginx
Manager → Agent : "Donne-moi le nom du système (OID 1.3.6.1.2.1.1.5.0)"
Agent → Manager : "Router-Paris-01"
```

➡️ Communication **synchrone** : le manager attend la réponse.

### TRAP Notification 🚨

L'agent **envoie spontanément** une alerte :

```nginx
Agent → Manager : "ALERTE ! Interface GigabitEthernet0/1 est DOWN !"
```

➡️ Communication **asynchrone** : pas d'attente de réponse.

### GET vs TRAP : quelle différence ? 🤔

| **GET (Polling)** | **TRAP (Alerting)** |
| :--: | :--: |
| Interrogation régulière | Notification immédiate |
| Consomme de la bande passante | Économe en ressources |
| Peut manquer des événements courts | Capture tous les événements |
| Manager contrôle la fréquence | Agent décide quand envoyer |

➡️ Une bonne supervision combine les deux approches !

### Les ports SNMP 🔌

* **Port 161 UDP** : Manager → Agent (GET, SET)
* **Port 162 UDP** : Agent → Manager (TRAP)

⚠️ SNMP utilise **UDP** (non fiable) plutôt que TCP pour des raisons de performance.

## 4. Les versions de SNMP 📋

### SNMPv1 - La première version (1988) 🏦

**Avantages** :
* Simple à implémenter
* Léger et rapide

**Inconvénients** :
* Aucune sécurité (texte clair)
* Pas d'authentification réelle
* Fonctionnalités limitées

➡️ Obsolète aujourd'hui mais encore présent sur du matériel ancien.

### SNMPv2c - L'amélioration (1993) ⚡

**Nouveautés** :

* GET-BULK pour récupérer plusieurs valeurs d'un coup
* Meilleurs codes d'erreur
* Performances améliorées

**Problème** : Toujours **aucune sécurité** !

➡️ Version la plus utilisée actuellement par simplicité.

### SNMPv3 - La version sécurisée (1998) 🔐

**Nouveautés majeures** :

* **Authentification** : vérification de l'identité
* **Chiffrement**: protection des données
* **Intégrité** : détection des modifications

**Inconvénient**: Plus complexe à configurer

➡️ Recommandé pour les environnements de production.

### Comparaison des versions 📊

| **Caractéristique** | **v1** | **v2c** | **v3** |
| :--: | :--: | :--: | :--: |
| Sécurité | ❌ | ❌ | ✅ |
| Performances | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Complexité | Simple | Simple | Complexe |
| Usage actuel | Rare | Courant | Croissant |

## 5. Sécurité 🔒

### Le concept de "Community String" 🔑

La community est comme un **mot de passe partagé** :

* **Public** (lecture seule) : permet GET
* **Private** (lecture/écriture) : permet GET + SET

⚠️ Problème : transmis en **clair** sur le réseau !

### Les risques de sécurité Avec SNMPv1/v2c : 😱

* Community string visible en clair → capture réseau
* Aucune authentification forte
* Possibilité de modifier la config (si community RW)
* Énumération du réseau

➡️ Un attaquant peut **espionner** et **modifier** les équipements !

### Bonnes pratiques de sécurité 🛡️

1. **Changer les communities par défaut** (public/private)
2. **Limiter les accès par ACL** (Access Control List)
3. **Utiliser des communities complexes** (comme des mots de passe)
4. **Séparer les communities RO et RW**
5. **Privilégier SNMPv3** quand c'est possible
6. **Utiliser un VLAN dédié** à la supervision

### SNMPv3 : la solution 🔐

**Les 3 niveaux de sécurité** :

1. **noAuthNoPriv** : Aucune sécurité (comme v2c)
2. **authNoPriv** : Authentification uniquement
3. **authPriv** : Authentification + Chiffrement ✅ 

➡️ Toujours utiliser **authPriv** en production !

## 6. SNMP dans un contexte de supervision 🏢

### Intégration avec les outils de supervision 🔗

Voir la doc de [Zabbix](https://www.zabbix.com/documentation/8.0/en/manual/appliance) 

Les outils comme **Zabbix**, **Nagios**, **PRTG** utilisent SNMP pour :

* Découvrir automatiquement les équipements
* Collecter les métriques (CPU, RAM, interfaces ... )
* Générer des alertes
* Creer des graphiques

## Métriques courantes supervisées via SNMP 📈

* **Système** : CPU, RAM, disque, température
* **Réseau** : Trafic entrant/sortant, erreurs, paquets perdus
* **Interfaces** : Etat (up/down), bande passante utilisée
* **Environnement** : Température, alimentation, ventilateurs.

### Avantages de SNMP en supervision 👍

* ✅ **Standardisé** : fonctionne sur tous les équipements réseau
* ✅ **Léger** : peu de charge sur les équipements
* ✅ **Universel** : supporte de nombreux constructeurs
* ✅ **Temps réel** : collecte rapide des données
* ✅ **Sans agent** : pas besoin d'installer de logiciel supplémentaire

### Limites de SNMP ⚠️

* ❌ **Sécurité** : v1/v2c non sécurisés
* ❌ **UDP** : perte de paquets possible
* ❌ **Complexité** : MIB et OID difficiles à maîtriser
* ❌ **Pas de garantie de livraison** : TRAPs peuvent être perdus
* ❌ **Limité au réseau** : peu adapté aux applications métier

➡️ SNMP est excellent pour le réseau, mais insuffisant seul pour une supervision complète.

## 12. Conclusion 🎓

## Ce qu'il faut retenir 🧠

* ✅ SNMP est le **protocole standard** de supervision réseau
* ✅ Il repose sur Manager, Agent et MIB
* ✅ SNMPv2c est simple mais **non sécurisé**
* ✅ SNMPv3 apporte **authentification et chiffrement**
* ✅ SNMP combine polling (GET) et alerting (TRAP)
* ✅ Essentiel pour superviser les équipements réseau

### Limites et alternatives 🔄

SNMP ne suffit pas pour une supervision complète :

* **NetFlow/sFlow** : analyse approfondie du trafic
* **API REST** : supervision d'applications
* **Agents** : supervision système (CPU, RAM, services)
* **Logs** : analyse comportementale

➡️ Une stratégie de supervision efficace **combine plusieurs approches**.

 Challenge du jour 👉 [Challenge B301](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/Challenge_B301.md) : supervision réseau 

 ---

