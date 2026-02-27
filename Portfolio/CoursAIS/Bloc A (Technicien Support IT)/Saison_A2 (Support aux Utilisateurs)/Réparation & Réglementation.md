# 🛠️ Réparation/remplacement de matériel
Après avoir testé les composants et identifié les pièces défectueuses, il est important de choisir la meilleure approche entre la réparation ou le remplacement.
## Réparation des composants
- **RAM** défectueuse : En fonction de l’erreur, il peut être possible de simplement nettoyer les contacts des barrettes ou de les positionner correctement dans les slots.
- **Disque dur / SSD** défectueux : Réparation via la réallocation des secteurs défectueux (dans la mesure du possible) ou formatage bas niveau. Si le disque est critique pour les données, envisager un clonage avant d’effectuer des réparations. S’il est mort, il faut le détruire à la perceuse avant de jeter (pour éviter du vol de données).
- **Réparation du BIOS/UEFI** : Si la carte mère ou le BIOS est corrompu, un reset du BIOS ou une mise à jour du firmware peut parfois résoudre le problème. Il faut éviter des coupures à tout prix pour éviter l’endommagement de la carte mère.
## Remplacement des composants
- Mémoire RAM : Sila barrette est défectueuse, remplacer par un modèle compatible.
- Alimentation (PSU) : Si le test des tensions ou des câbles révèle des problèmes, remplacer l’alimentation par une nouvelle, en veillant à choisir une puissance adaptée aux composants du système.
- Carte graphique ou carte mère : Si le problème est identifié comme étant lié à l’un de ces composants, remplacement obligatoire si aucune réparation n’est possible. Veiller à la compatibilité avec le reste du système.
## Autres cas complexes
Certains problèmes sont plus difficiles à diagnostiquer car ils ne se produisent pas de manière constante ou surviennent après une modification du système (ex : mise à jour d’un pilote, changement de matériel).
- Pannes intermittentes
- Surchauffe
- Conflits matériels
# Règlementations
## Présentation de la règlementation DEEE
Les DEEE sont des équipements électriques et électroniques en fin de vie, qui nécessitent un traitement spécifique pour réduire leur impact environnemental.
- **Origine de la réglementation** : La directive européenne 2012/19/UE sur les DEEE
- **Qu’est-ce que les DEEE** : Tout équipement fonctionnant à l’électricité ou utilisant des piles est considéré comme un DEEE une fois en fin de vie.
### Les enjeux
Les équipements électroniques contiennent des matériaux polluants (plomb, mercure, cadmium) qui peuvent être dangereux pour la santé et l’environnement s’ils ne sont pas traités correctement.
### Les impacts
- Pollution des sols et des eaux en cas de mise en décharge non contrôlée
- Consommation de ressources rares comme les métaux précieux (or, cuivre)
- Augmentation des déchets électroniques à l’échelle mondiale, avec un volume estimé à 50 millions de tonnes par an
### Les obligations légales
En tant que technicien, il est essentiel de comprendre comment la réglementation DEEE s’applique dans le cadre professionnel, notamment lors du remplacement ou de la mise au rebut d’équipements informatiques.
### Les rôles et responsabilités
- Pour les producteurs et distributeurs
     - Obligation de prendre en charge la collecte et le recyclage des DEEE vendus aux clients
     - Obligation de financer le traitement des équipements mis sur le marché après 2005.
- Pour les entreprises et les professionnels
     - Obligation de trier les DEEE et de les confier à une filière agréée pour le recyclage
     - Suivi des DEEE
     - Destruction des données (sensibles) notamment sur les supports de stockage
### Les sanctions
- Amendes et sanctions administratives si les entreprises ne respectent pas leurs obligations de gestion des DEEE
- Environnement : conséquences graves pour l’image de l’entreprise en cas de pollution causée par une mauvaise gestion des déchets électroniques
### Processus de collecte et de traitement des DEEE
- Tri et collecte des équipements
- Stockage temporaire sécurisé
- Transport vers des filières agréées
- Traitement et recyclage
- Destruction des données
### Les bonnes pratiques
- Réduire la quantité de DEEE
    - Privilégier le **reconditionnement** et la **réutilisation** du matériel
- Sensibiliser au recyclage
- Suivi et traçabilité des déchets
- Utiliser des logiciels de destruction des données (DBAN, Eraser par exemple)
## Introduction au RGPD
- Adopté au mai 2018
- **Objectif** : Renforcer la protection des données personnelles dans l’UE
- **Champ d’application** : Toute organisation traitant des données personnelles de citoyens de l’UE
- **Sanctions** : Jusqu’à 20 millions d’euros ou 4% du CA annuel mondial
## La CNIL (Commission Nationale de l’Informatique et des Libertés)
- **Rôle** : Autorité administrative indépendante française chargée de veiller à la protection des données personnelles
- **Création** : Fondée en 1978, renforcée par le RGPD en 2018
- **Missions principales** :
        1. Informer les individus et les professionnels
        2. Veiller à l’application de la législation
        3. Contrôler et sanctionner les entreprises en cas de non-conformité
- **Exemple** : Audits réguliers des pratiques des entreprises en matière de traitement de données.
### Principes clés du RGPD
1.Légalité, loyauté, transparence
2.Limitation des finalités
3.Minimisation des données : Réduire le nombre de données demandées aux utilisateurs
4.Exactitude des données :
5.Limitation de la conservation : détruire ou anonymiser la donnée.
6.Intégrité et confidentialité
7.Responsabilité (Accountability)
### Droits des individus
1.Droit d’accès 
2.Droit de rectification
3.Droit à l’effacement (« droit à l’oubli » ) : cependant sur le Web il y aura toujours des traces (voir **Wayback Machine** sur Google)
4.Droit à la limitation du traitement
5.Droit à la portabilité des données
6.Droit d’opposition
7.Droit de ne pas faire l’objet de décisions automatisées
### Mise en œuvre du RGPD en entreprise
Analyse d’Impact sur la Protection des Données (AIPD)
- Obligatoire si risque élevé pour les droits et libertés des personnes
- **Méthodologie** : Identifier les risques, évaluer l’impact, proposer des mesures
#### Registre des activités de traitement
- **Obligatoire** pour toutes les entreprises traitant des données personnelles
- Doit recenser les traitements effectués : objectifs, nature des données, durée de conservation
#### Consentement des personnes
- Consentement libre, spécifique, éclairé et univoque
- **Exemple** : Consentement pour les cookies sur les sites web (voir **Ad-Blockus** pour bloquer des pubs)
- **Bonnes pratiques** et pièges à éviter
#### Mesures de sécurité techniques et organisationnelles
- **Pseudonymisation et chiffrement** des données
- **Gestion des accès** : mots de passe sécurisés, gestion des utilisateurs
- **Sauvegardes et audits réguliers**
- **Formation des employés** à la protection des données
#### Le Délégué à la Protection des Données (DPO)
Ce n’est jamais le directeur. C’est souvent quelqu’un d’externe ou de la CSSCT
- **Rôle** : Conseiller, superviser la conformité, point de contact avec la CNIL
- **Obligation** : DPO obligatoire pour les organismes publics et grandes entreprises
- **Responsabilités** : Veiller à la conformité RGPD, former les équipes
#### En cas de violation des données
- **Définition** : Perte, vol, divulgation non autorisée des données personnelles
- **Obligations** :
     - Notifier la CNIL dans les 72h
     - Informer les personnes concernées si risque élevé
- **Exemples** : Fuite de données par email, disque dur non chiffré perdu
#### Plan de réponse aux incidents
- **Prévention** : Mise en place d’un plan de réponse pour limiter les dommages
- **Importance** : Limiter les risques en cas de violation de données (Voir
#### Outils et ressources
- **Outils logiciels** pour la gestion de la conformité RGPD : gestion des consentements, chiffrement, sécurité des données
- **Ressources en ligne** : Guides de la CNIL, pour accompagner les entreprises*
# Conclusion de la journée
1.**Diagnostique et résolution de pannes matérielles :**
- Approche méthodique
- Utilisation des outils de diagnostic
- Importance de la maintenance proactive

2.Réglementation **DEEE** :
- Respect des obligations de recyclage
- Gestion des déchets électroniques dans le cadre légal
3.Conformité **RGPD** :
- Protection des données personnelles
- Mise en place de mesures techniques et organisationnelles
- Droits des individus et obligations des entreprises
## Message final
- **Importance de la vigilance** : Respecter les bonnes pratiques de diagnostic et de conformité pour éviter les pannes matérielles et les sanctions légales
- **Rôle de la formation continue** : La technologie et la réglementation évoluent constamment, d’où l’importance de rester à jour

**Cours qui suit:** [[BIOS, UEFI...]]
