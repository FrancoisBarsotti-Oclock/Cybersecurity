# 🔏🪟 Session A408. Pools IIS, Authentication & Backup.

Ce cours approfondit la configuration du serveur Web IIS avec la gestion des pools d'applications et de l'authentification, et aborde un aspect critique de l'administration système : la sauvegarde et la restauration (notamment d'Active Directory) avec Windows Server Backup.

- **IIS - Pools d'Application** :
     - **Définition** : Un pool d'application est un mécanisme d'isolation qui permet d'exécuter des sites web ou des applications dans des processus séparés. il les isole afin qu'ils ne se perturbent pas entre eux. Chaque pool fonctionne avec son propre processus système (`w3wp.exe`).
    - **Idée clé** : **Un pool = Une isolation**. Si un site plante ou consomme toutes les ressources, les autres sites (dans d'autres pools) continuent de fonctionner normalement.
    - **Avantages** :
        - **Isolation** : Si une application plante, elle ne fait pas tomber les autres pools.
        - **Sécurité & Stabilité** : Permet de définir une identité spécifique (compte de service) et des réglages de recyclage pour chaque application.
        - **Flexibilité** : Possibilité de configurer des versions de .NET différentes par pool.
    - **Configuration** : Dans le Gestionnaire IIS > Pools d'applications > Ajouter un pool d'applications. Chaque pool peut avoir sa propre version de .NET et son propre compte de service (identité).

- **IIS - Authentification et Contrôle d'Accès** : Méthode simple pour sécuriser un site en demandant un nom d'utilisateur et un mot de passe. *Exemple*: accès restreint à un site internet via des comptes AD.
     - **Objectif** : Par défaut, un site est en accès anonyme. L'authentification permet d'exiger une identification pour garantir la traçabilité et restreindre l'accès aux seules personnes autorisées.
    - **Installation** : C'est une fonctionnalité à ajouter via le Gestionnaire de serveur > Rôle Serveur Web (IIS) > Serveur Web > Sécurité >
    - **Authentification de base** : Méthode simple où le navigateur demande un identifiant et un mot de passe.
        - _Attention_ : Les identifiants sont encodés en Base64 (facilement déchiffrables), il est donc crucial d'utiliser le **SSL/TLS (HTTPS)** pour chiffrer la connexion.
        - **Règle d'or** : Si on active l'Authentification de base, il faut impérativement **désactiver l'Authentification anonyme** pour forcer la connexion.
    - **Authentification Digest** : Plus sécurisée que la "Basic" (hachage des identifiants), mais moins robuste que Windows.
    - **Authentification Windows** : La plus robuste pour un Intranet, utilise **NTLM** ou **Kerberos** (AD).
    - **Certificat Client** : Authentification forte basée sur des certificats X.509.
    - **Autres restrictions** : Il est aussi possible de filtrer par **Adresse IP/Domaine** ou d'utiliser le **Filtrage des demandes** (URL Authorization) pour bloquer certaines requêtes spécifiques.

- **Windows Server Backup & Stratégie de Sauvegarde (PRA)** : Outil permettant de sauvegarder et restaurer la configuration système, les fichiers et les rôles installés. *Exemple*: restaurer un service ou une configuration supprimée par erreur. *Idée clé* : la restauration peut cibler tout le système ou une partie précise.
     - **Contexte** : La sauvegarde est le pilier du **PRA (Plan de Reprise d'Activité)**.
        
    - **Windows Server Backup** : Solution native idéale pour les TPE/PME. Elle permet des sauvegardes complètes, incrémentielles ou de l'état du système ("System State"). A installer via "Ajout de rôles et fonctionnalités".
        
    - **Limites** : Pour les grandes structures, on privilégie des solutions tierces (Veeam, etc.) offrant la centralisation, la déduplication, et la réplication cloud.
        
    - **Technologies** :
        
        - **Sauvegarde classique** : Garantie de continuité "à froid".
        - **VSS (Volume Shadow Copy)** : Mécanisme de "Snapshot" (instantané) permettant de sauvegarder des fichiers en cours d'utilisation sans interrompre le service.
    - **Stratégie** : Les snapshots permettent des sauvegardes fréquentes sans interruption de service. Une sauvegarde complète "à froid" (services arrêtés) reste une bonne pratique ponctuelle pour une cohérence absolue.
        
    - **Format** : Les sauvegardes sont stockées sous forme d'images disques **.vhdx**, qui peuvent être montées manuellement pour récupérer des fichiers unitaires.
        
    - **Types de récupération** : Fichiers et dossiers, Volumes entiers, Applications, ou **État du système** (System State).
        
- **Sauvegarde et Restauration Active Directory** : Un utilisateur supprimé peut être récupéré en restaurant l’état du système. La - restauration réinjecte les objets AD tels qu’ils étaient au moment de la sauvegarde.
    - **Composants critiques** : La sauvegarde de l'AD repose sur deux dossiers clés :
        - **NTDS** : Contient la base de données de l'annuaire (`ntds.dit` avec utilisateurs, groupes, ordinateurs).
        - **SYSVOL** : Contient les fichiers publics répliqués (Stratégies de groupe/GPO, scripts de connexion).
        - **Restauration de l'État du Système (System State)** : C'est l'option à choisir pour récupérer un AD. Elle restaure :
        - **AD** (L'annuaire lui-même).
        - **FRS/DFSR** (Le dossier SYSVOL).
        - **Registry** (La configuration système locale).
    - **Règle impérative** : Active Directory étant un service critique en cours d'exécution, il **ne peut pas être restauré en mode normal**. Il faut obligatoirement redémarrer le serveur en **Mode de restauration des services d'annuaire (DSRM)**.
Organisation via pool, de façon à avoir tous le sites web dans un seul endroit
![[Pasted image 20251208222524.png]]

![[Pasted image 20251208222538.png]]

![[Pasted image 20251208222554.png]]
# WSB (Windows Server Backup)

C’est un outil pour gérer la sauvegarde du réseau.

On va rajouter le rôle (WSB)  juste pour la démo et après on le supprime, pour éviter que la journalisation remplisse la ROM trop vite (il va faire des sauvegardes toutes les 24h par défaut, même si cela peut se paramétrer).

Pour pouvoir faire la démo d’utilisation du rôle, nous rajoutons un disque dur de 50 Go sur la VM

-------------------------------------------

**Astuce Test TSSR** : On peut avoir une question pour rajouter un disque dur dans une VM. Alors, il faudra juste regarder le type de disque dur qu’il y a déjà d’installé pour rester sur le même type de disque dur (même si ce n’est pas le meilleur type). Il faudra toujours justifier son choix.

------------------------------------------

Début d’après-midi : La sauvegarde ne doit pas être négligée par les AIS

Pour les AIS

![[Pasted image 20251208222655.png]]
On va devoir choisir entre deux options de sauvegarde
![[Pasted image 20251208222714.png]]
Pour un Plan de reprise d'activité c'est mieux une sauvegarde qu'un VSS

On choisit l’heure de sauvegarde
![[Pasted image 20251208222738.png]]
Choisir le type de destination
![[Pasted image 20251208222759.png]]
On doit sélectionner le dossier correspondant
![[Pasted image 20251208222821.png]]
Il fera apparaître le volume du dossier choisis
![[Pasted image 20251208222845.png]]
Il faudra cocher le disque pour pouvoir continuer avec l’avertissement :
![[Pasted image 20251208222904.png]]
Deuxième avertissement à accepter,
![[Pasted image 20251208222925.png]]
Ce qui va faire disparaître le disque que j’avais rajouté
![[Pasted image 20251208222955.png]]
Jusqu’à là on a juste créé la planification
![[Pasted image 20251208223019.png]]
Il faudra l’exécuter à travers « sauvegarde unique »
![[Pasted image 20251208223035.png]]

![[Pasted image 20251208223048.png]]
A savoir que la sauvegarde se trouve dans le disque que j’avais rajouté

Une fois la sauvegarde faite, nous pourrons faire la restauration sur récupérer »
![[Pasted image 20251208223107.png]]

![[Pasted image 20251208223124.png]]

![[Pasted image 20251208223139.png]]
On choisit ce qu’on restaure
![[Pasted image 20251208223201.png]]
Encore une fois à choisir
![[Pasted image 20251208223224.png]]
On peut choisir l’emplacement
![[Pasted image 20251208223244.png]]
Il me fait un dossier
![[Pasted image 20251208223303.png]]
Or, il le garde toujours dans le dossier que nous avons choisi juste avant
![[Pasted image 20251208223324.png]]
« `msconfig.msc` » permettra de faire un redémarrage sans échec.
