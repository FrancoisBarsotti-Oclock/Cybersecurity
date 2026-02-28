# 🔝🪟 Session A405. Gestion du Stockage : Filtres, Quota & Audit.

Ce cours aborde la gestion avancée des serveurs de fichiers, principalement via le rôle **FSRM** (File Server Resource Manager / Gestionnaire de ressources du serveur de fichiers). Il permet de contrôler l'utilisation du stockage et de sécuriser les données grâce à la mise en place de l'audit.

- **Le Rôle FSRM (Gestionnaire de ressources du serveur de fichiers)** :
     - C'est une fonctionnalité de Windows Server qu'il faut installer pour gérer finement les quotas et les filtres de fichiers.
    - Contrairement aux quotas NTFS standards (qui s'appliquent à un volume entier), FSRM permet de gérer des quotas par **dossier**.

- **Gestion des Quotas (Limitation d'espace)** :
     - **Objectif** : Éviter la saturation des disques par les utilisateurs et répartir équitablement les ressources de stockage. Limite l’espace disque utilisable par un utilisateur ou un dossier. Exemple: Accorder 5 Go maximum à chaque utilisateur pour son répertoire personnel.
    - **Types de Quotas** :
        - **Quota strict (Hard quota / Inconditionnels)** : Bloque l'écriture une fois la limite atteinte (l'utilisateur reçoit un message "Espace disque insuffisant").
        - **Quota souple (Soft quota / Conditionnels)** : Ne bloque pas l'utilisateur, mais sert à la surveillance. Il génère des alertes (logs, emails à l'admin) lorsque des seuils sont dépassés.
    - **Modèles** : On utilise des modèles de quotas pour appliquer automatiquement des règles (ex: "Limite de 5 Go") à tous les nouveaux sous-dossiers créés.

- **Filtrage de Fichiers (File Screening)** :
     - **Objectif** : Contrôler le type de contenu stocké sur un volume (ex: interdire les fichiers personnels comme les MP3 ou les vidéos AVI sur un serveur professionnel ou sur un dossier partagé). Exemple: Filtrer automatiquement les fichiers vidéo dans un dossier "Documents de travail". Tout cela pour garder un espace organisé et éviter l'usage non professionnel du stockage.
    - **Fonctionnement** : Se base sur des **groupes de fichiers** (listes d'extensions, ex: `*.mp3`, `*.mkv`).
    - **Types de filtrage** :
        - **Filtrage actif** : Empêche l'utilisateur d'enregistrer le fichier interdit (message "Accès refusé").
        - **Filtrage passif** : Autorise l'enregistrement mais génère une alerte pour l'administrateur (utile pour surveiller sans bloquer le travail).

- **Audit des Accès (Traçabilité)** :
     - **Objectif** : Renforcer la sécurité en gardant une trace ("Qui a fait quoi et quand ?") sur les fichiers sensibles. Idéal pour savoir qui a supprimé ou modifié un fichier critique. Il suit les actions réalisées sur fichiers et dossiers (lecture, modification, suppression, etc.). Exemple: Vérifier qui a supprimé un fichier partagé.
    - **Mise en place (2 étapes)** : 1. **Activer la stratégie d'audit** : Via une GPO (Configuration Ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies locales > Stratégie d'audit > **Auditer l'accès aux objets**). 2. **Configurer la SACL** : Sur le dossier cible (Clic droit > Propriétés > Sécurité > Avancé > Onglet **Audit**), on définit _qui_ on surveille et pour _quelles actions_ (Réussite/Échec de suppression, écriture, etc.).
    - **Consultation** : Les traces se trouvent dans l'**Observateur d'événements**, journal **Sécurité**.
## Nouvelle GPO (GP_Lecteurs)

Comme le partage de lecteurs dépend des utilisateurs, c’est une stratégie utilisateur
![[Pasted image 20251208213703.png]]
Avec un clic droit on fait Nouveau > Lecteur mappé
![[Pasted image 20251208213726.png]]
Mettre un emplacement **UNC** \\
![[Pasted image 20251208213758.png]]
On se concentre sur les actions !
![[Pasted image 20251208213823.png]]
- **Mettre à jour** : Applique la GPO uniquement s’il y a déjà un lecteur
- **Créer** : crée le lecteur uniquement s’il n’existe pas
- **Mettre à jour** : Peu importe l’état du lecteur (il le crée s’il n’existe pas encore et puis le met à jour). C’est pour ça, celle-ci est la valeur par défaut
- **Supprimer** : S’il existe, le lecteur est supprimé

On choisit la lettre du lecteur, on coche « Reconnecter » et on la libelle
![[Pasted image 20251208213848.png]]
Elle sera rajoutée dans le mappage
![[Pasted image 20251208213909.png]]
On peut faire un `gpupdate /force` dans l’invite de commandes d’un utilisateur.

Tel qu’elle est, la règle est appliquée à tous les utilisateurs, alors il faudra cibler dans les propriétés
![[Pasted image 20251208213945.png]]
![[Pasted image 20251208214003.png]]

![[Pasted image 20251208214019.png]]

![[Pasted image 20251208214036.png]]

![[Pasted image 20251208214051.png]]
Là il y a que les Andromèdes qui sont affectés.

Alors, je peux répéter le même processus pour les Aldebaran (utiliser le même disque « Z : » ne fera pas de conflit si l’on fait bien ciblage).

Puis, faire un autre partagé (Commun)
![[Pasted image 20251208214118.png]]
Mais je vais cibler « GS_Promotions »
![[Pasted image 20251208214142.png]]
# Quota
Gestion de l’espace d’un groupe ou plutôt d’un disque (comme si c’était une partition).

Il faut activer la fonctionnalité
![[Pasted image 20251208214213.png]]
Une fois activée (elle nous emmène à la partie « rôles » et on accepte l’installation), on pourra modifier le Quota de chaque groupe.

Pour créer **son propre Quota**, il faudra aller dans Outils > Gestionnaire des ressources du serveur des fichiers
![[Pasted image 20251208214236.png]]
Cela ouvre la fenêtre de Gestion de ressources…
![[Pasted image 20251208214258.png]]
Il n’y aura rien dans Quotas
![[Pasted image 20251208214322.png]]
Mais que dans les modèles
![[Pasted image 20251208214343.png]]
J’aurai toujours d’options dedans :
![[Pasted image 20251208214404.png]]
Le conditionnel enverra une notification à l’administrateur une fois la limite dépassée.

On va créer un modèle et l’appliquer aux partages
