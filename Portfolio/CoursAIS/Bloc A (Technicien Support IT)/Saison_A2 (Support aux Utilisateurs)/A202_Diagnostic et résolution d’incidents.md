# 🩺 Diagnostic et résolution d’incidents

**Pourquoi c’est crucial en entreprise ?** tout simplement pour limiter l’impact de certaines pannes. L’objectif c’est de résister aux pannes

- **Impact des pannes** : interruption de service, baisse de productivité, perte de données.
- **Prévention des incidents majeurs** : Surveillance **proactive** pour éviter les crashs critiques (mise en place d’outils de supervision, collecte de data sur le serveur pour prévoir d’incidences). Pour tout ce qui est système et data de système, le plus simple est de le faire avec des outils de monitoring et de supervision, mais ça dépend de ce que l’on veut surveiller et le type de panne que l’on veut préserver (comme prévoir une communication d’éducation à l’utilisateur).
- **Sécurité** : Les incidents peuvent révéler des failles exploitables par des attaques. Analyser le type de panne et pouvoir trouver un correctif et, à partir de là, mettre un autre plan en place pour vérifier qu’elle ne soit pas effective sur d’autres types de système.

  [ ] Une des premières choses à mettre en place : PRA (Plan de Reprise d’Activité, pour engager des actions une fois qu’on a la panne) et PCA (Plan de Continuité d’Activité, qui est basé avant d’avoir la panne pour les éviter), (on en parle après, mais c’est en effet une des premières choses à mettre en place pour gérer toute incidence)

 [ ] On a la priorité au service (faire en sorte de remettre l’activité en place, même si dégradée, ce sera mieux que pas d’activité du tout)

# Différents outils

## Le Gestionnaire de Tâches (Ctrol + Alt + Suppr)

C’est le premier point d’un système Windows et qui va nous permettre de commencer à comprendre ce qui se passe dans l’ordinateur. C’est juste un commencement car ce n’est pas le plus complet ni le plus efficace, mais c’est le coup d’œil rapide.

C’est l’outil essentiel pour surveiller et gérer les processus en cours d’exécution (applications lancées par nous, par d’autres personnes ou par notre système) sur un système Windows. Il permet de visualiser les performances en temps réel, d’arrêter les processus qui ne répondent pas et de contrôler l’impact des applications sur les ressources système comme le CPU et la mémoire. Il est également utile pour générer les programmes qui se lancent au démarrage du système. Le plus d’application qu’on a, le plus de consommation de ressources.

Avec un click droit en n’importe quelle cellule supérieure, on peut rajouter le **PID** (Process identifier) ou numéro unique qui identifie différents processus. Aussi le **Nom du processus** (nom réel du fichier), puis la **ligne de commande** (c’est les catégories les plus intéressantes qu’il faudrait activer en tant qu’admin)

Aussi avec un clic droit, on peut changer la valeur des ressources en pourcentage.

La partie **historique des applications** n’est pas intéressant car ce n’est pas toutes les applications que l’on peut y voir.

Avec la console **msconfig** (Win+S) où on pouvait voir tout ce qui se lançait au démarrage, mais cela a été bougé sur la gestion de tâches.

Il y a l’option de Collecte du vidage de la mémoire, ce qui libérera de la RAM

Comment défaire ?

**La pagination** (ou _fichier d’échange / swap file_) est un espace sur ton disque dur que Windows utilise comme **mémoire virtuelle** quand la **RAM** est pleine. Autrement dit, si ton PC manque de RAM, Windows déplace temporairement certaines données vers ce fichier pour éviter les plantages.

**Pour voir la pagination sur Windows 11 :**

1. Clique droit sur **Ce PC** → **Propriétés**.
2. Dans la colonne de droite, clique sur **Paramètres système avancés**.
3. Sous l’onglet **Avancé**, dans la section **Performances**, clique sur **Paramètres**.
4. Va dans l’onglet **Avancé** → section **Mémoire virtuelle** → clique sur **Modifier…**.

## Historique de fiabilité (Win + S + fiabilité)

Le _Reliability Monitor_ est un outil intégré à Windows 10 et 11 qui affiche la **stabilité du système** dans le temps. Cela permet de voir ce qui a crashé.

## L’Observateur d’événements

C’est un outil de diagnostic permettant d’accéder aux journaux des événements systèmes.

Il aide à identifier des erreurs, des avertissements ou des événements critiques qui peuvent perturber le bon fonctionnement du système. Cet outil est souvent utilisé pour diagnostiquer des crashs d’applications, des pannes système ou de problèmes de sécurité.

Son but est de savoir ce qui s’est passé. On va l’utiliser après un événement passé.

Il faut taper « **événement** » sur Windows search pour arriver à « observateur d’événements »

Créer des événements d’administration

**L’outil de résolution des problèmes** (bientôt à disparaître)

C’est intégré à Windows automatise le diagnotistic et la correction de certains problèmes courants. Que ce soit pour résoudre des soucis de réseau, des erreurs liées aux périfiphériques ou des problèmes de mises à jour, cet outil propose des solutions immédiates après avoir détecté l’origine du problème.

Question examen AIS : A quoi sert Winget ?

**L’Outil PSR (Problem Steps Recorder Ctrl shift S)** aussi à disparaître bientôt, c’est un outil peu connu mais puissant de Windows qui enregistre les actions effectuées sur le système. I génère un rapport détaillé sous forme de captures d’écran et de descriptions des étapes, facillitant ainsi la communication des incidents techniques à un service d’assistance ou un administrateur système. Remplacé par « **Capture d’écran** »  où on peut faire un enregistrement aussi
## Nestat

est un utilitaire en ligne de commande qui permet de surveiller les connexions réseau actives et d’afficher des statistiques sur les ports, les protocoles et les adresses IP. Il est utile pour diagnostiquer des problèmes de réseau et pour vérifier les connexions ouvertes par les applications en cours d’exécution.

Je cherche à comprendre qui est connecté sur ma machine et à travers quel port (on peut croisé ave Gestionnaire des tâches, en utilisant le PID).
## Les outils SysInternals

Développée par Microsoft, regroupe une série d’outils avancés pour l’analyse et le dépannage du système. Parmi les outils les plus utilisés :

**Process Explorer** : Une version avancée du gestionnaire de tâches pour une analyse approfondie des processus.

**TCPView** : Monitore les connexions réseau en temps réel.

C’est quoi un Sysvol ? (à voir)

# Dépannage du démarrage de Windows : comprendre et réparer le boot de Windows

**BootMGR (1ère étape)**

C’est le gestionnaire de démarrage de Windows. Il remplace l’ancien NTLDR depuis Windows Vista et permet de charger le système d’exploitation à partir de l’initialisation du BIOS ou UEFI. En cas de corruption de BootMGR, le système ne pourra pas démarrer correctement, nécessitant une réparation via des commandes comme bootrec /fixboot.

Il se lance dans un environnement de récupération

**Winload.exe**
C’est le chargeur du système d’exploitation qui prend le relais après Boot MGR pour charger le noyau de Windows ainsi que les pilotes critiques nécessaires au démarrage du système. C’est après cette étape que la RAM es chargée. Si Winload est corrompu, le démarrage échoue et un écran de récupération apparaîtra, suggérant des options de dépannage comme la réparation du démarrage.

**WinResume.ese** gère la reprise du système après une mise en veille prolongée (hibernation). Il permet à Windows de restaurer la session précédente, incluant l’état des applications et des fichiers ouverts. Si ce processus échoue, Windows peut rester bloqué sur l’écran de reprise, nécessitant une réinitialisation ou une réparation du fichier hibernation.

**Hibernation** étape pour , on etteint le poste… en pause. C’est la veille prolongée.

**La veille classique** c’est juste un arrêt de l’écran et du système, mais elle va tout de même continuer à calculer

**BCD (Boot Configuration Data)** c’est une base de données utilisée par Boot MGR pour stocker les paramètres de démarrage du système d’exploitation. C’est l’équivalent moderne du fichier **boot.ini** dans les anciennes versions de Windows. Les erreurs liées au BCD peuvent empêcher Windows de démarrer correctement. Il est possible de corriger ces problèmes via des outils de réparation comme bootrec ou en modifiant directement le BCD avec des commandes avancées.

**Bootrec** c’est une commande puissante pour réparer les problèmes liés au démarrage de Windows. Il peut reconstruire le **Master Boot Record (MBR)**, le secteur de démarrage, et restaurer le BCD. Les commandes essentielles incluent :

- bootrec /fixmbr : Répare le MBR.
- bootrec /fixboot : Répare le secteur de démarrage.
- bootrec / rebuildbcd : Reconstruit le BCD en recherchant les installations Windows sur les disques.

**Réinitialiser ce PC**

La fonction permet de restaurer Windows à son état initial tout en conservant ou supprimant les fichiers personnels. C’est une solution rapide pour résoudre des problèmes logiciels graves qui ne peuvent être réparés par des méthodes classiques. On peut choisir entre :
- Conserver mes fichiers : Réinstalle Windows sans affecter les fichiers personnels.
- Tout supprimer : Réinitialise complètement le PC en effaçant toutes les données.

**Démarrage Avancé**

**WinRE**

**Restauration du Systèyme** permet de ramener Windows à un état antérieur en restaurant les fichiers système et la configuration. Cela peut corriger des erreurs introduites par des modifications récentes (installation d’applications, mises à jours).

Important : cette fonctionnalité doit être activée au préalable. Si désactivée, il sera impossible de restaurer le système à une version antérieure.

**DiskPart** C’est un utilitaire en ligne de commande pour gérer les disques et partitions. Il peut être utilisé pour diagnostiquer des problèmes de disque empêchant Windows de démarrer. Des commandes comme list disk, select disk, et clean sont couramment utilisées pour préparer les disques à une nouvelle installation ou réparer des partitions corrompues.

**CHKDSK** C’est un outil de vérification des disques qui permet de détecter et de réparer les erreurs du système de fichiers et les secteurs défectueux. Il est souvent utilisé pour diagnostiquer et résoudre des problèmes de démarrage liés à des disques durs endommagés. Les commandes courantes incluent :
- chkdsk /f : Répare les erreurs trouvées sur le disque.
- chkdsk /r : Recherche les secteurs défectueux et récupère les données.

**Autocompletion**

**Historique des Fichiers** : c’est une fonctionnalité de sauvegarde automatique qui permet de conserver des copies régulières de fichiers personnels (documents, photos, etc.). Il stocke plusieurs versions d’un fichier, permettant de revenir à une version précédente en cas de suppression ou de modification accidentelle. Ce service doit être configuré et activé pour être utilisé.

**Sauvegarde du Système** permet de créer une image complète du système, y compris les fichiers, les programmes installés et les paramètres système. Cette image peut être utilisée pour restaurer le PC en cas de panne matérielle ou logicielle majeure. Cette méthode est plus complète que l’Historique des Fichiers, car elle sauvegarde tout le système, pas uniquement les fichiers personnels.

**Windows Service Manager** est un outil permettant de gérer les services Windows (programmes en arrière-plan). Il est possible de démarrer, arrêter, désactiver ou configurer le démarrage des services, ce qui peut être essentiel pour résoudre des problèmes de démarrage ou de performance. Des services critiques comme **Windows Update** ou **Windows Defender** peuvent y être contrôlés.

**Windows Memory Diagnostic** est utilisé pour vérifier l’intégralité de la mémoire vive (RAM) du système. Il permet de détecter des erreurs matérielles qui pourraient être à l’origine de plantages ou de comportement anormaux de Windows (écrans bleus, redémarrages intempestifs). Il effectue une analyse approfondie au redémarrage du système et fournit un rapport détaillé après le test.

**éditeur de Registre**

c’est un outil puissant qui permet d’accéder et de modifier les paramètres du registre Windows, une base de données contenant les configurations système, les paramètres des logiciels, et des informations critiques pour le bon fonctionnement du système d’exploitation. Manipuler le registre peut résoudre des problèmes spécifiques, mais une mauvaise modification peut entraîner des dysfonctionnements graves.

**HKEY_CLASSE_ROOT**

**HKEY_CURRENT_USER (HKCU)**

Il contient les paramètres et les préférences de l’utilisateur actuellement connecté. Il stocke les configurations spécifiques à cet utilisateur, telles que les préférences

**HKEY_LOCAL_MACHINE (HKLM)**

Il stocke les paramètres et configurations globales du système, qui affectent tous les utilisateurs. Cette clé contient des informations sur les périphériques matériels, les pilotes, les services, et les paramètres critiques de Windows.

**L’éditeur de Registre : Types de valeurs**

Dans le registre Windows, les données sont sotckées sous différents types de valeurs :

- REG_SZ :
- REG_DWORD :
- REG_BINARY :
- REG_MULTI_SZ :

**Le Gestionnaire de Périphériques** c’est un outil Windows permettant de visualiser, gérer et dépanner tous les périphériques matériels connectés au système, comme les cartes réseau, les disques durs, les claviers, et les pilotes associés. Il est utilisé pour installer, mettre à jour, désactiver ou désinstaller des pilotes, ainsi que pour identifier les périphériques défectueux grâce aux icônes d’alerte (par exemple, un point d’exclamation jaune indiquant un problème).

**Dépannage des périphériques**

Lorsque vous rencontrez un problème matériel, le **Gestionnaire de périphériques** fournit des informations détaillées sur les erreurs. On peut effectuer des actions comme :
- Mettre à jour un pilote : Rechercher et installer la dernière version dun pilote pour un matériel spécifique.
- Restaurer un pilote : Revenir à une version précédente d’un pilote en case de mise à jour défaillante.
- Désactiver/activer un périphérique : Résoudre les conflits matériels en désactivant temporairement un périphérique.

**Les Logiciels Tiers (faire attention)**

Les **logiciels tiers** sont des outils externes qui complètent

**Exemples de logiciels Utiles (faire attention)**
- CCleaner : Un logiciel de nettoyage

**Avantages et précautions**

**Cours qui suit:** [[Prise en main à distance & Hardware]]
