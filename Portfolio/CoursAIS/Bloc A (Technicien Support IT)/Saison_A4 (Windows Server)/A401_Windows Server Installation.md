# 🪟 Session A401. Installation Windows Serveur 2019

## Pourquoi WS 2019 ?

### Sécurité renforcée

Protection avancée avec Windows Defender et intégration transparente avec Azure pour une sécurité cloud optimale

### Gestion simplifiée

Windows Admin Center offre une interface moderne et intuitive pour administrer vos serveurs efficacement

### Virtualisation améliorée

Support optimisé pour Hyper-V et services réseau avancés, idéal pour les infrastructures modernes

## Préparation Essentielle

Vérifier les prérequis matériels

### Processeur

64 bits, minimum 4 cœurs à 2 GHz pour des performances optimales

### Stockage

Disque dur avec 32 Go d'espace libre minimum, davantage selon les rôles

### Mémoire RAM

Au moins 512 Mo requis, mais 8 Go fortement recommandés en production

### Affichage

Carte graphique supportant une résolution minimale de 1024x768 pixels

## Préparation du support d’installation

### Téléchargement

Récupérez l'image ISO officielle de Windows Server 2019 depuis le site Microsoft ou votre portail de licences

### Création de média

Utilisez l'outil de création de média Windows ou Rufus pour créer une clé USB bootable à partir de l'ISO

### Configuration BIOS

Accédez au BIOS/UEFI de votre serveur et configurez l'ordre de démarrage pour prioriser la clé USB

## Installation de WS 2019

Étapes clés de l’installation

### Démarrage

Démarrez le serveur sur le support d'installation USB créé précédemment

### Paramètres régionaux

Sélectionnez la langue d'installation, le format horaire et la disposition du clavier français

### Type d'installation

Choisissez "Installation personnalisée" pour effectuer une installation propre et complète

### Partitionnement

Créez et formatez les partitions selon vos besoins spécifiques en stockage

### Installation

Lancez l'installation et patientez pendant la copie des fichiers système (15-30 minutes)

## Configuration initiale post-installation

### Mot de passe administrateur

Créez un mot de passe fort et sécurisé respectant les critères de complexité Windows

### Paramètres régionaux

Vérifiez et ajustez les fuseaux horaires, formats de date et paramètres réseau de base

### Mises à jour

Connectez-vous à Windows Update et installez immédiatement tous les correctifs de sécurité disponibles

## Configuration Réseau et Rôles

### Configurer une adresse IP statique

#### Accès aux paramètres

Ouvrez le Centre Réseau et Partage, puis cliquez sur "Modifier les paramètres de la carte"

#### Configuration IP

Définissez manuellement l'adresse IP fixe, le masque de sous-réseau, la passerelle par défaut et les serveurs DNS

#### Validation

Une IP statique garantit que votre serveur reste accessible de manière cohérente sur le réseau

**Astuce** : Documentez toujours votre plan d'adressage IP pour éviter les conflits réseau futurs

### Renommer le serveur

Un nom de serveur explicite facilite l'identification et la gestion dans votre infrastructure réseau.

#### Navigation

Accédez à Paramètres > Système > Informations système

#### Modification

Cliquez sur "Renommer ce PC" et saisissez un nom descriptif comme AD-01 ou SRV-DC01

#### Redémarrage

Redémarrez le serveur pour appliquer le nouveau nom de manière permanente.
![[Pasted image 20251207182058.png]]
### Ajouter le rôle Active Directory (AD DS)

#### Étape 1

Lancez le Gestionnaire de serveur depuis la barre des tâches ou le menu Démarrer

#### Étape 2

Cliquez sur "Gérer" puis sélectionnez "Ajouter des rôles et fonctionnalités"

#### Étape 3

Cochez "Services de domaine Active Directory" et ajoutez les fonctionnalités requises

#### Étape 4

Poursuivez l'installation sans redémarrer immédiatement le serveur
![[Pasted image 20251207182149.png]]
## Promouvoir le serveur en contrôleur de domaine
![[Pasted image 20251207182226.png]]
### Étapes de promotion AD

#### Notification post-installation

Cliquez sur le drapeau de notification dans le Gestionnaire de serveur affichant "Configuration requise"

#### Lancement de la promotion

Sélectionnez "Promouvoir ce serveur en contrôleur de domaine" pour démarrer l'assistant

#### Création de forêt

Choisissez "Ajouter une nouvelle forêt" et définissez le nom de domaine racine (ex : entreprise.local ou entreprise.lan)

#### Niveau fonctionnel

Sélectionnez Windows Server 2016 ou supérieur pour bénéficier des fonctionnalités les plus récentes

#### Mot de passe DSRM

Définissez un mot de passe robuste pour le mode restauration des services d'annuaire - conservez-le précieusement

#### Finalisation

Validez la configuration, lancez l'installation. Le serveur redémarrera automatiquement une fois terminé

### Vérification finale

#### Tests de validation

- Reconnectez-vous au serveur avec les identifiants du domaine
- Ouvrez la console "Utilisateurs et ordinateurs Active Directory"
- Vérifiez la présence du domaine et des unités organisationnelles par défaut
- Créez un utilisateur de test pour valider le bon fonctionnement d'AD
- Testez également la création d'un groupe de sécurité

### Bonnes pratiques post-installation

#### Sécurité prioritaire

Installez immédiatement toutes les mises à jour de sécurité critiques via Windows Update et configurez les mises à jour automatiques.

#### Stratégies de groupe

Déployez des GPO pour renforcer la sécurité : politique de mots de passe, verrouillage de compte, et restrictions d'accès

#### Sauvegardes régulières

Mettez en place un plan de sauvegarde automatique de l'état du système et de la base Active Directory

#### Documentation complète

Conservez une documentation détaillée de la configuration réseau, des mots de passe DSRM et de l'architecture

## Conclusion : Votre serveur Windows Server 2019 est prêt !

- Installation complète

Serveur configuré de manière sécurisée et optimisée selon les meilleures pratiques Microsoft

- Base solide

Infrastructure prête à accueillir vos services : partages de fichiers, messagerie, applications métier

- Prochaines étapes

Gestion avancée des utilisateurs, déploiement de GPO, intégration de services spécifiques à vos besoins
