# 🖥️ Session A105. Découverte virtualisation
## Guide d'utilisation de VirtualBox
### Installation et création de machines virtuelles Windows 10, Windows 11 et Ubuntu

### François BARSOTTI

## 📌 Introduction
La virtualisation permet d’exécuter un système d’exploitation complet dans un environnement isolé.
Elle est utile pour :

* 🌐 Naviguer sans exposer son système principal (isolation des cookies, sandboxing)
* 🛡️ Tester des logiciels sans risque
* 🧪 Apprendre l’administration système
* 🐧 Découvrir Linux ou pratiquer la cybersécurité

Ce guide explique comment créer et configurer des VM Windows 10, Windows 11 et Ubuntu sous **VirtualBox**.


## 🧰 Pré-requis
* VirtualBox installé : [Voir directement sur le site web](https://www.virtualbox.org)

* ISO du système souhaité (Windows 10, Windows 11, Ubuntu 24.04)

* Option Windows : **désactiver “Masquer les extensions des fichiers dont le type est connu”**

    * 📁 Explorateur → Affichage → Options → Affichage → décocher l’option 
    💡​ Cela émule le contenu d’un disque dur.

    ![01-Options dossiers]()


## 🧱 1. Création d’une VM Windows 10

![02-Win10 on VB]()

### ⚙️ Configuration recommandée

| **Ressource** | **Valeur** |
| --- | --- |
| RAM | **4 Go minimum** |
| CPU | **2 cœurs** |
| EFI | ❌ **Désactivé** |
| Disque | 25–40 Go (VDI, dynamique) |

![03-RAM Win10 on VB]()

## 🧩 Étapes d’installation
### 1️⃣ Créer la VM
* Nouveau → Nom : Windows 10
* Type : Microsoft Windows
* Version : Windows 10 (64-bit)
* Définir RAM + CPU
* Créer un disque VDI dynamique

### 2️⃣ Monter l’ISO
* Paramètres → Stockage
* Lecteur optique → Choisir un fichier disque → sélectionner l’ISO Win10

### 3️⃣ Installer Windows
* Démarrer la VM
* Suivre l’assistant d’installation
* Si l’écran reste noir → 🔄 **redémarrer la VM**

### 4️⃣ Retirer l’ISO après installation
Paramètres → Stockage → Supprimer le disque du lecteur optique

###5️⃣ Plein écran & intégration
* Périphériques → **Insérer l’image CD des Additions invité**
* Exécuter l’installeur
* Redémarrer

![04-Ma première Win10 VM]()

>### 📝 Astuce clavier VirtualBox  
>Si la touche Host (Ctrl droit) ne fonctionne pas, utiliser la touche `²`.

## 🧱 2. Création d’une VM Windows 11
### ⚙️ Configuration recommandée

| **Ressource** | **Valeur** |
| --- | --- |
| RAM | **4–8 Go** |
| CPU | **2–4 cœurs** |
| EFI | ✅ **Activé** |
| TPM | Automatique (VirtualBox le simule) |
| Disque | 40 Go |

## 🧩 Étapes d’installation
### 1️⃣ Créer la VM
* Nouveau → Windows 11
* Activer **EFI** dans :
Paramètres → Système → Carte mère → *Activer EFI*

### 2️⃣ Monter l’ISO
Comme pour Windows 10.

## 🚫 Installer Windows 11 sans compte Microsoft
### 🔥 Méthode “bourrine” (100 % efficace)
1. À l’écran demandant un compte Microsoft → **déconnecter Internet**
2. Appuyer sur **Shift + F10** pour ouvrir CMD et Taper : `OOBE\BYPASSNRO` 
    * ou bien appuyer sur **Ctrl + Shift + J** et taper `WinJS.Application.restart("ms-cxh://LOCALONLY")`, selon version.
3. La VM redémarre
4. Choisir **Je n’ai pas Internet**
5. Puis **Continuer avec une configuration limitée**
6. Créer un utilisateur local

### 💡 Méthode “mignonne n°1”
* Faire `OOBE\BYPASSNRO` sans débrancher Internet
* Windows redémarre et propose la création locale

### 💡 Méthode “mignonne n°2”
* Dans CMD (Shift + F10) taper : `start ms-cxh:locally`
    * Windows ouvre directement la création d’un compte local

### 🖥️ Plein écran & Additions invité
Même procédure que Windows 10.

![05-Ma première Win11 VM]()

## 🐧 3. Création d’une VM Ubuntu 24.04
### ⚙️ Configuration recommandée

| **Ressource** | **Valeur** |
| --- | --- |
| RAM | **4 Go minimum** |
| CPU | 2 cœurs |
| Disque | **25 Go VDI dynamique** |

## 🧩 Étapes d’installation
### 1️⃣ Créer la VM
* Nouveau → Linux → Ubuntu (64-bit)
* Définir RAM + CPU + disque

### 2️⃣ Monter l’ISO
* Paramètres → Stockage → Lecteur optique → Choisir ISO Ubuntu

### 3️⃣ Installer Ubuntu
* Démarrer
* Choisir **Try or Install Ubuntu**
* Puis **Install Ubuntu**
* Suivre l’assistant (utilisateur, fuseau horaire, disque virtuel)

### 4️⃣ Retirer l’ISO
* Paramètres → Stockage → Supprimer le disque du lecteur optique

## ➕ Installer les Guest Additions sous Ubuntu
### 1️⃣ Mettre à jour le système
* Ouvrir un terminal :
```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
```

### 2️⃣ Insérer l’image CD
* Périphériques → **Insérer l’image CD des Additions invité**
* Un disque apparaît dans la barre latérale

### 3️⃣ Lancer l’installation
Méthode graphique :
* Cliquer sur le CD → *Run Software*

Méthode terminal :
```bash
cd /media/$USER/VBox_GAs_*/ 
sudo ./VBoxLinuxAdditions.run
```

### 4️⃣ Redémarrer la VM
* Machine → Redémarrer

![06-Ma première Ubuntu VM]()

## 👥 Créer un deuxième utilisateur Ubuntu
1. Paramètres système → **Users**
2. Cliquer sur **Unlock** → entrer le mot de passe
3. Bouton `+` pour ajouter un utilisateur
4. Choisir :
    * Standard
    * ou **Administrator**
5. Définir un mot de passe
6. Activer le compte si nécessaire

## 📎 Commandes utiles Ubuntu
| **Commande** | **Description** |
| --- | --- |
| ``ls`` | Liste les fichiers |
| ``ls ``/`` | Liste la racine |
| ``man ``ls`` | Manuel de la commande |
| ``man ``apt`` | Manuel du gestionnaire de paquets |

## 🔄 Copier / Coller entre hôte et VM
Après installation des Additions invité :
* Paramètres de la VM → Général → Avancé
* **Presse-papiers partagé : Bidirectionnel**
* **Glisser-déposer : Bidirectionnel**

---
