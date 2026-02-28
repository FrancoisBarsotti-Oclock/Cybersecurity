# 👨‍👨‍👦‍👦🪟👩‍👩‍👧‍👧 Session A403. GPO (Stratégie de Groupe)

Ce cours aborde les Stratégies de Groupe (**GPO - Group Policy Objects**), un outil puissant d'Active Directory pour gérer centralement la configuration des utilisateurs et des ordinateurs dans un environnement Windows.

- **Principe des GPO** :
        - Une GPO est un ensemble de paramètres de configuration appliqués à des utilisateurs ou des ordinateurs.
    - Elles permettent d'automatiser la gestion, d'appliquer des règles de sécurité, d'installer des logiciels ou de configurer l'environnement de travail (fond d'écran, mappage de lecteurs, etc.) à grande échelle.
    
- **Structure et Application** :
    * ***Conteneurs** : Les GPO peuvent être liées à des **Sites**, des **Domaines** ou des **Unités Organisationnelles (OU)**.
    - **Héritage** : Par défaut, les GPO appliquées à un conteneur parent sont héritées par les conteneurs enfants. Cet héritage peut être bloqué ou forcé.
    - **Ordre d'application (LSDOU)** : Local > Site > Domaine > OU. La dernière GPO appliquée l'emporte en cas de conflit (l'OU a donc la priorité sur le Domaine).
    
- **Configuration Utilisateur vs. Ordinateur** :
    * **Configuration Ordinateur** : S'applique au démarrage de la machine, quel que soit l'utilisateur qui se connecte (ex: paramètres de pare-feu, installation de logiciels système).
    - **Configuration Utilisateur** : S'applique à l'ouverture de session de l'utilisateur (ex: scripts de connexion, restrictions du panneau de configuration, raccourcis bureau).
    
- **Gestion des GPO** :
    * L'outil principal est la **Console de gestion des stratégies de groupe (GPMC)**.
    - On y crée les objets GPO, on les modifie via l'éditeur, et on les lie aux conteneurs AD souhaités.
    - **Filtrage de sécurité** : Permet de restreindre l'application d'une GPO à certains utilisateurs, groupes ou ordinateurs spécifiques, même si elle est liée à leur OU.
    - **WMI Filters** : Permettent d'appliquer une GPO selon des critères techniques (ex: version de l'OS, espace disque disponible).
    
- **Commande utile** :
    - `gpupdate /force` : Force la mise à jour immédiate des stratégies de groupe sur le client, sans attendre le cycle de rafraîchissement automatique (environ 90 minutes).
    - `gpresult /r` : Affiche un rapport sur les GPO appliquées à l'utilisateur et à l'ordinateur, utile pour le diagnostic.
## Restauration de Active Directory
Il faut désactiver le « démarrage sécurisé »
![[Pasted image 20251208202617.png]]
## Créer une nouvelle UO (dossier)

Déjà vu, cette fois pour créer « Promotions »
![[Pasted image 20251208202644.png]]
Assigner le nom du dossier
![[Pasted image 20251208202746.png]]
Si jamais on veut effacer : Affichage > Fonctionnalités avancées, puis sélectionner le dossier qu’on veut effacer et rentrer dans ces **propriétés**
![[Pasted image 20251208202811.png]]
**Décocher** la case de « protection des suppressions » au niveau de l’**objet**. Et à partir de là, le dossier pourra être effacé.
![[Pasted image 20251208202857.png]]
## Création de groupes de sécurité

Clic droit sur la page blanche, pour **Nouveau > Groupe**
![[Pasted image 20251208202943.png]]
### La différence des types d’étendues :
![[Pasted image 20251208203012.png]]
## Rajout d’un utilisateur dans un groupe de sécurité

Pour ajouter qqn dans un groupe il y a deux façons

**Méthode 1** : Double clic sur le nom du groupe > `membres > ajouter > écrire le nom > vérifier les noms`
![[Pasted image 20251208203037.png]]
**Méthode 2** : Double clic sur le nom de la pax > `Membre de > Ajouter >`
![[Pasted image 20251208203121.png]]
il va apparâitre
![[Pasted image 20251208203759.png]]
Je peux ensuite créer un **groupe de sécurité** pour les deux groupes : GS_Promotions, sous le UO « Promotions »
![[Pasted image 20251208203830.png]]
Et les choisir au même temps :
![[Pasted image 20251208203856.png]]
Comme ça on peut gérer les droits et règles génériques de mes promotions, même si l’on peut toujours gérer des droits individuels.

# Group Policies Object (GPO)

Au niveau des groupe nous pouvons créer de GPO, avec des stratégies de groupe : il s’agit des politiques que l’on va appliquer aux utilisateurs. Pour cela il faudra aller dans « `Gestion des stratégies de groupe` »
![[Pasted image 20251208203958.png]]
Et c’est là-dedans où nous trouvons le concept de `Forêt` :
![[Pasted image 20251208204043.png]]
Avec toutes les informations du domaine racine et du domaine dedans :
![[Pasted image 20251208204111.png]]
Si je déroule mon domaine, je retrouve la même arborescence qu’on a du côté du AD (le premier outil pour gérer les groupes et les utilisateurs):
![[Pasted image 20251208204152.png]]
Alors, **attention** à ne pas confondre l’outil AD (comptes utilisateurs et ordinateurs) avec cet outil de GSG/GPO (permissions et actions des utilisateurs).

Ce que l’on voit dans le panneau de droite du domaine c’est la politique par défaut de mon serveur et de tout mon parc (**Default Domaine Policy**, qui se place juste en dessous du domaine et qui représente un lien pour les GPO). C’est lui qui nous oblige, par exemple, à faire un mdp complexe lors de l’enregistrement.

Vu qu’il se trouve toute en haut (au niveau de mon domaine), il va affecter tout ce qui se trouve en bas, comme les comptes utilisateurs.

Pour **modifier la GPO**, il faut juste la sélectionner et modifier pour rentrer dans les configurations (ordinateur et utilisateur)
![[Pasted image 20251208204231.png]]
Et des nouvelles informations apparaissent, pour me proposer de paramétrer sous plusieurs niveaux :
![[Pasted image 20251208204254.png]]
Si je prends, par exemple, système, je vais trouver un tas de stratégies dedans, comme « _Désactiver l’accès à l’invite de commandes_ » :
![[Pasted image 20251208204344.png]]
## Copier une GPO
La GPO se trouve dans « objets de stratégie de groupe »
![[Pasted image 20251208204420.png]]
On va les prendre et les **glisser dans le dossier** où on veut les appliquer (ils vont apparaître en forme de raccourcie, comme ici dans les sous-dossier de « Promotions »)
![[Pasted image 20251208204447.png]]
Ces raccourcis peuvent être effacés, contrairement à l’original (**à éviter**).
# Créer une GPO
On sélectionne le dossier où on va appliquer la GPO
![[Pasted image 20251208204609.png]]
Une fois créée, l’originale va apparaître automatiquement dans « objets de stratégie de groupe », même si l’on l’a créée à partir du dossier « Andromède » (où elle apparaît comme raccourci)
![[Pasted image 20251208204636.png]]
Comme elle est vide, il faut la configurer (dans ce cas, on le fait pour les utilisateurs) :
![[Pasted image 20251208204703.png]]
Et on va dans système pour chercher la politique de « désactiver l’accès à l’invite de commandes » (aux utilisateurs Andromède) :
![[Pasted image 20251208204731.png]]
On « _active la désactivation_ ». Le côté commentaire c’est réservé pour des possibles tickets liés (genre JIRA).
![[Pasted image 20251208204805.png]]
