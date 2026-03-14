# Challenge B201 : Installation et configuration de TrueNAS Scale

**Objectifs** :
- Installer TrueNAS Scale sur une VM Proxmox
- Configurer un stockage ZFS avec RAIDZ1
- Créer un dataset et un partage SMB
- Tester la création et la restauration de snapshots ZFS

**Prérequis** :
- Accès à un serveur Proxmox
- Connaissances basiques en virtualisation et réseaux
- Une VM Windows ou Linux pour tester le partage SMB

---

## 1. Introduction

TrueNAS Scale est une solution NAS open source permettant de gérer du stockage, des applications et de la virtualisation.
**Différence avec TrueNAS Core et Scale** :
- **Scale** : Gère les applications et l’hypervision (conteneurs, VMs).
- **Core** : Optimisé pour le stockage pur (plus performant si besoin uniquement de NAS).

---

## 2. Installation de TrueNAS Scale

### 2.1. Téléchargement de l’ISO
1. Récupérer la version **Community stable** depuis [le site officiel](https://www.truenas.com/download-truenas-community-edition/).
2. Dans Proxmox :
   - Aller dans `local` → `ISO images` → `Download from URL`.
   - Coller le lien de téléchargement de l’ISO TrueNAS Scale.

### 2.2. Création de la VM
- **Disques** :
  - 1 disque SCSI (50 Go) pour l’installation.
  - 3 disques SATA (100 Go chacun) pour le stockage (RAID).
- **CPU** : 2 cœurs.
- **Mémoire** : 4 Go.
- **Réseau** :
  - Bridge : LAN.
  - Désactiver le firewall.

### 2.3. Installation
1. Démarrer la VM sur l’ISO.
2. Suivre l’assistant :
   - Installer sur le disque SCSI (50 Go).
   - Méthode d’authentification : **Administrative user** (login : `truenas_admin`).
   - Désactiver le **legacy boot**.
3. À la fin de l’installation, noter l’**IP** affichée (ex: `10.0.0.x`).
4. Redémarrer la VM.

**Test** : Se connecter à l’interface web via l’IP notée.

---

## 3. Configuration initiale

1. **Localisation** :
   - Aller dans `System` → `General settings`.
   - Régler : France, clavier français, fuseau horaire Paris.
   - *Astuce* : Rafraîchir la page si les changements ne s’affichent pas.

---

## 4. Gestion du stockage avec ZFS

### 4.1. Concepts clés
- **Vdev** : Regroupement de disques physiques (RAID ou non).
- **Pool** : Datastore créé sur un ou plusieurs Vdevs.
- **Dataset** : Zone de stockage dans un pool (équivalent à un dossier).
- **Partage** : Permet d’accéder à un dataset depuis le réseau.

### 4.2. Création d’un volume de stockage
1. Aller dans `Stockage` → `Créer un volume`.
2. Remplir :
   - **Nom** : `PoolNAS`.
   - **Chiffrement** : Aucun.
   - **Données** :
     - Layout : **RAIDZ1** (équivalent RAID5).
     - Sélection automatique : 3 disques de 100 Go.
3. Valider et créer le volume.

---

## 5. Création d’un dataset et partage SMB

### 5.1. Création du dataset
1. Aller dans `Datasets` → `Ajouter un dataset`.
2. Remplir :
   - **Nom** : `Dataset`.
   - **Préréglage** : SMB (pour Windows).
   - *Ne pas cocher* "Créer un partage SMB".
3. Laisser les options avancées par défaut.

### 5.2. Configuration du partage SMB
1. **Créer un utilisateur SMB** :
   - Aller dans `Identifiants` → `Utilisateurs` → `Ajouter`.
   - Remplir : Prénom/Nom, mot de passe, groupe principal `root`, cocher `Utilisateur SMB`.
2. **Créer le partage** :
   - Aller dans `Partages` → `Ajouter SMB`.
   - Sélectionner le dataset `Dataset`.
   - Valider.

### 5.3. Test du partage
1. Depuis une autre VM (Windows/Linux) :
   - Ouvrir l’explorateur réseau.
   - Se connecter à l’IP de TrueNAS avec l’utilisateur SMB créé.
   - Créer un fichier `texte.txt` avec le contenu "Hello".

---

## 6. Snapshots ZFS

### 6.1. Création d’un snapshot
1. Aller dans `Datasets` → Sélectionner `Dataset` → `Créer un instantané`.
2. Laisser les valeurs par défaut et valider.

### 6.2. Restauration
1. Supprimer le fichier `texte.txt` depuis la VM cliente.
2. Retourner dans TrueNAS, sélectionner l’instantané → `Cloner vers un nouveau dataset`.
3. Partager le nouveau dataset.
4. Depuis la VM cliente, vérifier que le fichier `texte.txt` est de nouveau présent.

---

## 7. FAQ / Aide

**Problème de connexion SMB ?**
- Vérifier que le service SMB est activé dans `Services`.
- Vérifier que l’utilisateur a bien le droit SMB.

**Erreur de création de volume ?**
- Vérifier que les disques ne sont pas déjà utilisés.

**Besoin d’aide ?**
- Consulter la [documentation officielle](https://www.truenas.com/docs/).

---
