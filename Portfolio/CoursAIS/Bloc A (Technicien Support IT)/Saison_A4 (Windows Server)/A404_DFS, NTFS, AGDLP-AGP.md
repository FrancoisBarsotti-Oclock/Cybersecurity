# ✍️🪟 Session A404. DFS, NTFS, AGDLP/AGP

Ce cours explore le service DFS de Windows Server, essentiel pour centraliser l'accès aux données, simplifier l'arborescence réseau pour les utilisateurs, et garantir la haute disponibilité des fichiers grâce à la réplication.

- **DFS (Distributed File System)** :
    
    - **Définition** : Service Windows Server permettant de regrouper plusieurs partages réseau (situés sur différents serveurs) sous un seul et unique espace de noms logique (ex : `\\domaine.local\partages`).
    - **Rôle principal - DFS Namespace** : Crée un point d'accès logique et unifié. Les utilisateurs n'ont plus besoin de connaître le nom du serveur physique.
    - **Rôle secondaire - DFS Replication (DFSR)** : Synchronise les données entre plusieurs serveurs pour assurer la tolérance de panne et la haute disponibilité. Si un serveur tombe, les utilisateurs accèdent automatiquement à une copie des données sur un autre serveur.
    - **Bénéfices** : Simplification des chemins d'accès pour les utilisateurs et haute disponibilité des données.
- **Gestion des Permissions : Partage vs. NTFS** :
    
    - **Permissions NTFS** : S'appliquent au niveau du **système de fichiers local**. Elles définissent les droits précis (Lecture, Écriture, Modification, Contrôle total) et s'appliquent après l'accès au partage.
    - **Permissions de Partage** : Gérées au niveau du **répertoire partagé** sur le réseau. Elles sont plus générales (Lecture, Modification, Contrôle total).
    - **Règle de cumul** : Les permissions NTFS s'appliquent **après** les droits de partage. L'utilisateur reçoit toujours le **droit le plus restrictif** entre les droits de partage et les droits NTFS.
- **Héritage et Bonnes Pratiques** :
    
    - **Héritage** : Transmet automatiquement les droits définis sur un dossier parent aux sous-dossiers et fichiers. Il est possible de désactiver cet héritage ("casser l'héritage") pour isoler et redéfinir des droits spécifiques.
    - **Bonne Pratique (Simplification)** : Pour la simplicité administrative, il est courant de donner le droit **Contrôle total** au groupe `Tout le monde` (ou `Utilisateurs Authentifiés`) sur la **permission de partage**, et de gérer toutes les **vraies restrictions** et la sécurité via les **permissions NTFS**.
- **Modèle AGDLP / AGP (Modèle pour les droits)** :
    
    - Ce modèle est une bonne pratique pour l'administration évolutive des droits dans les grandes structures (plutôt pour les grosses sociétés) :
        - **A**ccounts (Utilisateurs et ordinateurs)
        - placés dans des **G**roupes **D**omains **G**lobaux
        - ajoutés dans des **L**ocal **P**ermission Groups
        - puis ces groupes reçoivent des **P**ermissions sur la ressource (dossiers NTFS ou partages DFS).
- **Tips** :
    
    - Ajouter le suffixe `$` au nom d'un dossier partagé (ex : `drivers$`) cache le répertoire aux utilisateurs qui parcourent le réseau, tout en permettant l'accès via le chemin UNC complet.
    - Pour les besoins spécifiques, il est parfois plus simple de **casser l'héritage** et de redéfinir manuellement les permissions. Soit en supprimant tout et remettant manuellement, soit en
## UNC (Universal…)

Modifier le Wallpaper (image du York)
Une fois la GPO créée, il faut faire l’update via le Command Prompt : `gpupdate /force`
![[Pasted image 20251208210546.png]]
Avec `gpresult /r` on peut voir les GPO acceptées (appliqués) et refusées
![[Pasted image 20251208210634.png]]
## Serveur de Service des fichiers (DFS)

C’est pour la distribution des fichiers, à partir des fichiers que j’ai en tant qu’administrateur.

Il faudra créer un fichier « Wallpaper » dans `C:\` et j’y vais déplacer le dossier d’image
![[Pasted image 20251208210719.png]]
Il faudra alors mettre le chemin **UNC** (genre : `\\Server\Share\Corp.jpg`), c’est un chemin **obligatoire** à faire pour partager des fichiers. Dans ce cas, il faudra que je partage le chemin de Wallpaper sur le réseau, pour que tous les utilisateurs puissent accéder au contenu de ce dossier.

Clic droit sur le blanc du dossier (ou directement sur le dossier) > `Propriétés > Partage > Partager`

![[Pasted image 20251208211019.png]]On définit avec qui on le partage avec « Tout le monde » (tel que recommandé par Microsoft)
![[Pasted image 20251208211104.png]]
Et on leur donne accès en lecture/écriture une fois rajoutés (de toute façon, plus tard, on modifie les droits d’écriture)
![[Pasted image 20251208211129.png]]
Maintenant partagé, j’ai un répertoire son chemin UNC
![[Pasted image 20251208211157.png]]
Accepter pour obtenir le chemin copiable :
![[Pasted image 20251208211225.png]]
Avec ce chemin, nous pourrons trouver le dossier dans les autres utilisateurs

Et je n’ai plus que mettre le chemin de partage dans ma GPO sur le WS

Pour cela, je remplace le chemin local de la GPO par le chemin UNC
![[Pasted image 20251208211249.png]]
Pour que l’écran de l’utilisateur prenne l’image, il faut faire le `gpupdate /force` sur le Command Prompt. Puis, on déconnecte et rouvre la session.
## Dossier de partage commun

Si l’on rafraîchit les partages, on verra le nouveau dossier partagé :
![[Pasted image 20251208211356.png]]
Mais il faudra installer le rôle de « Réplication DFS » à travers un nouveau partage SMB Rapide (Windows pour Windows)
![[Pasted image 20251208211419.png]]
**Note** : De base la bonne pratique serait d'avoir un deuxième disque dur (à part) pour le partage de fichiers
![[Pasted image 20251208211452.png]]
Si on prend la sélection par volume, il va faire un nouveau dossier (« Shares ») que je ne peux pas écraser

En sélectionnant par défaut, cela va créer un fichier C:\Shares si on veut changer on doit sélectionner un chemin personnalisé.

On donne un nom à notre dossier de partage.

- Le dossier "Shares" sera pas partagé (c'est du local).
- Le point d'entrée sera le dossier qu'on va créer
- Donc ce sont les dossiers qui seront dans shares qui sera partagé.
![[Pasted image 20251208211522.png]]
Mais « Shares » n’est pas partagé, mais le dossier qui se trouve à l’intérieur.

On ne peut pas activer l’énumération basée sur l’accès car cela va créer des conflits avec les droits d’accès de chaque utilisateur, et pas la peine de « chiffrer l’accès aux données » :
![[Pasted image 20251208211552.png]]
Windows recommande de laisser les droits de partage open-bar pour tous les utilisateurs :

- **Par défaut, sur un serveur, tous les utilisateurs disposent des droits de lecture et d’exécution sur le partage.**
- À ce stade, ce sont donc les **droits de partage** qui s’appliquent.
- En pratique, ce ne sont pas les autorisations du dossier de partage lui‑même que l’on gère, mais bien celles sur le **contenu** qu’il contient.
- Autrement dit, par défaut, tout le monde peut voir et exécuter les fichiers présents dans le partage.
![[Pasted image 20251208211617.png]]
Les droits d’un compte peuvent être modifiés de plusieurs façons. Si jamais on modifier les droits d’un côté, Windows va favoriser les droits vers le bas (refus). C’est déconseillé d’utiliser le refus d’accès (le refus c’est un ‘forcing’).

L’héritage signifie que le dossier de partage hérite des mêmes droits que son dossier parent, et que ces autorisations s’appliquent automatiquement à l’ensemble des sous‑répertoires et fichiers qu’il contient.

Si l’on rentre dans « Personnaliser », très **attention** car on pourra modifier le « Héritage de » :
![[Pasted image 20251208211647.png]]
Maintenant mon droit de partage fonctionne et ce nouveau partage apparaitra listé :
![[Pasted image 20251208211706.png]]
Une fois le partage créé je peux me mettre dans les propriétés du dossier et modifier tout ce que je veux, niveau autorisations.
![[Pasted image 20251208211737.png]]
Une fois dedans, on peut casser l’héritage
![[Pasted image 20251208211803.png]]
Pour les dossiers qui se trouvent dans le « Shares » on peut bien casser l’héritage depuis « `C :` ». Si je ne casse pas l'héritage de "Commun" et qu'après je donne des nouveaux droits à "C:", ça veut dire que "Commun" aura ces nouveaux droits de "`C:`". Il faut juste le casser correctement et proprement (on le verra après).

Puis, confirmer. Pour voir le résultat.
![[Pasted image 20251208211857.png]]
Si je rentre dans administrateur, je pourrai personnaliser les autorisations de base et spéciales.
![[Pasted image 20251208211922.png]]
Résultat
![[Pasted image 20251208211949.png]]
On pourra aller voir le partage du côté de nos utilisateurs. On active la découverte. Il me faut le mdp administrateur, côté Win10.

Du côté de WS je peux voir aussi les paramètres de sécurité des fichiers.

Puis, on peut ajouter un droit.

Sélectionner un principal, pour choisir le groupe, puis (dé)cocher les droits :
![[Pasted image 20251208212016.png]]
On applique uniquement aux objets du conteneur.

Les autorisations spéciales sont étendues (personnalisées) dans « Afficher les autorisations **avancées**.
![[Pasted image 20251208212042.png]]
Puis, on peut vérifier les accès effectifs
![[Pasted image 20251208212106.png]]
**Attention** : le bouton « Afficher l’accès » Bugue.

Si l’on veut **cacher un dossier**, on peut le nommer avec le `symbole de USDollar` à la fin. Dans Windows, il sera toujours caché même pour l’administrateur (on ne pourra le voir que dans le gestionnaire de serveur > Partages). Cependant, tout le monde pourra y accéder en écrivant la route exacte sur les dossiers du Réseau (sur WS ou sur Windows pareil).

Vidéo 2025-11-24_aprèsmidi (minute 1 : 21 :23)
