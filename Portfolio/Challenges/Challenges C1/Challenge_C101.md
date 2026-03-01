# 🏆 Challenge C101 - Fiche de cadrage: Modernisation de l'infrastructure IT du campus O'clock
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

> Ce challenge fait partie du projet "fil rouge", à être suivi tout au long de la saison C1.
> Vous êtes responsable de l’informatique au sein d’un campus de formation professionnelle (en présentiel 😁).Le campus compte en permanence environ 500 personnes, entre les salariés (une quinzaine), les formateurs (freelances, formateurs occasionnels) et les apprenants (formation continue et alternance).
>
>La direction vous demande de moderniser l’infrastructure IT du campus pour accueillir de nouveaux services numériques : serveurs fichiers, NAS, firewall, VLAN et accès sécurisé Wi-Fi.
>
Dans votre service, vous accueillez actuellement un alternant.
>
>### Consignes
>
>Rédigez la fiche de cadrage du projet comprenant :
>
> * Objectifs du projet
> * Périmètre et exclusions
> * Parties prenantes (interne / externe)
> * Livrables principaux
> * Contraintes Qualité / Coût / Délai
> ### Notes
>
> * Vous pouvez rédiger votre document avec l’outil de votre choix (Google Doc, fichier Markdown…)
> * Gardez bien le fichier : il servira pour la suite !
> * Prenez le temps de chercher de la documentation sur le sujet
> * On ne demande pas d’être exhaustif, chaque partie peut ne contenir que quelques points
> 
> À faire également Lire le Manifeste pour le développement Agile de logiciels: https://agilemanifesto.org/iso/fr/manifesto.html
>
> Commenter le Manifeste (questions, points positifs, critiques…) dans un fichier (libre choix de l’outil).

Voir 👉 [Cours C101](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C1%20(Gestion%20de%20projet%20%26%20analyse%20de%20risques)/C101_Gestion%20de%20Projets%20Intro.md) 👈

## Ressources recommandées: 
[éléments clés pour réussir](https://facility-project.com/note-cadrage-projet/)

[Exemple de note de cadrage](https://aidealareussite.unistra.fr/pluginfile.php/215862/mod_resource/content/2/Exemple%20-%20Note%20de%20cadrage.pdf): Objectifs, périmètre, risques, livrables.

[Méthode QQOQCCP](https://bpifrance-creation.fr/moment-de-vie/qqoqccp-outil-presenter-idee#:~:text=Utilis%C3%A9%20dans%20de%20nombreux%20domaines,projet%20de%20fa%C3%A7on%20quasi%20exhaustive.), pour clarifier besoins et responsabilités.

[Comprendre le Triagle QCD](https://organisologie.com/triangle-qcd/), pour arbitrage et priorisation.

[Cycle en V](https://fr.wikipedia.org/wiki/Cycle_en_V), utile pour infrastructures critiques

[Agile & Scrum](https://business.adobe.com/fr/blog/basics/agile-vs-scrum-project-management), pour les lots fonctionnels, feedbak utilisateurs

[ITIL 4](https://www.atlassian.com/fr/itsm/itil), pour mieux orienter la gouvernance, le service et l'exploitation.

#
### :memo: **1. Contexte**

Le campus accueille en permanence environ 500 personnes (salariés, formateurs internes/externes, apprenants).
L’infrastructure IT actuelle n’est plus adaptée aux besoins croissants en services numériques, sécurité et segmentation réseau.
La direction souhaite une modernisation globale, et le service IT intègre un alternant dans le projet.
#

### :dart: **2. Objectifs du projet** 

2.1. Mettre en place une infrastructure IT moderne, sécurisée et évolutive

2.2. Fournir des services numériques fiables (serveurs, stockage, réseau)

2.3. Sécuriser les accès filaires et Wi-Fi

2.4. Segmenter les usages via VLAN

2.5. Transférer des compétences à l’alternant
#

### :round_pushpin: **3. Périmètre du projet**

3.1. **Inclus** :

* Serveurs fichiers
* NAS
* Firewall
* VLAN (administration, formateurs, apprenants, invités)
* Wi-Fi sécurisé (authentification, segmentation)
* Documentation technique

3.2. **Exclus** :

* Applications métiers
* Support utilisateurs long terme
* Fourniture des postes clients
* Supervision avancée (SOC, SIEM)
#

### :school: **4. Parties prenantes**

4.1. **Internes** :

* Direction du campus (sponsor)
* Service IT (chef de projet)
* Alternant IT
* Salariés et apprenants (utilisateurs)

4.2. **Externes** :

* Prestataires réseau / sécurité
* Fournisseurs matériels (serveurs, firewall, AP Wi-Fi)
* Éventuels intégrateurs
#

### :soon: **5. Livrables principaux**

* Dossier d’architecture réseau et sécurité
* Schémas réseau (logique / physique)
* Configuration firewall, VLAN, Wi-Fi
* Serveurs et NAS opérationnels
* Procédures d’exploitation
* Guide de prise en main pour l’alternant
#

### :bangbang: **6. Contraintes**

6.1. **Qualité** :

* Haute disponibilité
* Sécurité des accès
* Simplicité d’administration

6.2. **Coût** :

* Budget maîtrisé
* Priorité aux solutions pérennes

6.3. **Délais** :

* Déploiement progressif
* Interruption minimale de service
#

### :white_check_mark: **7. Méthode recommandée**

* Méthode hybride
* Cycle en V : cette méthode séquentielle structure le projet en phases distinctes (analyse des besoins, conception, réalisation, tests, validation). Elle permet de garantir que chaque étape est validée avant de passer à la suivante, ce qui est particulièrement adapté aux projets d'infrastructure où la sécurité et la fiabilité sont critiques.
* Méthode Agile : l'utilisation de sprints courts permet d'apporter de la flexibilité au projet. Des ajustements peuvent être réalisés rapidement en fonction des retours terrain ou des contraintes techniques découvertes en cours de route.
* Approche DevOps : cette approche favorise l'automatisation de la configuration des serveurs, du déploiement des services et des tests. Elle permet de gagner du temps, de réduire les erreurs humaines et d'assurer une mise en production plus rapide et fiable.
* Découpage par lots fonctionnels (réseau, sécurité, stockage)
* Validation à chaque étape
* Implication active de l’alternant
#

### :interrobang: **8. QQOQCCP (synthèse)**

|Question | Réponse |
| -------- | :-------: |
|Qui ?  |Service IT, alternant, prestataires |
|Quoi ? | Modernisation complète de l’infrastructure IT |
|Où ? | Campus O'clock |
| Quand ? | Déploiement progressif |
|Comment? | Lots techniques + itérations |
| Combien ? | Budget contraint, à optimiser |
|Pourquoi ? | Sécurité, performance, évolutivité |
#
à suivre avec le challenge C102...
