# ✳️🔏🪟 Session A410. RDS (Remote Desktop Services)

Ce cours introduit le rôle RDS (Services Bureau à Distance) pour la centralisation des environnements utilisateurs. Il couvre le déploiement rapide, la sécurisation via certificats et l'automatisation de la connexion via les GPO.

- **RDS (Remote Desktop Services)** :

- Les Services Bureau à Distance permettent d'héberger des sessions utilisateurs sur un serveur centralisé (virtualisation de session).
- **Fonctionnement** :

- Le **Serveur Hôte** exécute les applications et le bureau.
- Le **Client** (léger ou PC) se connecte via le protocole **RDP** (port 3389).
- **Multi-session** : Plusieurs utilisateurs travaillent simultanément sur le même serveur, chacun dans sa bulle isolée.

- **Modes d'utilisation** :

- **Bureau complet** : L'utilisateur accède à un bureau Windows distant classique.
- **RemoteApp** : Seule la fenêtre de l'application est envoyée au client. L'application s'exécute sur le serveur mais semble tourner en local (intégration transparente).

- **Installation (Démarrage Rapide)** :

- Via le Gestionnaire de serveur > "Installation des services Bureau à distance".
- Choisir **"Démarrage rapide"** pour une installation sur un seul serveur (installe le Broker, l'Accès Web et l'Hôte de session en une seule opération).
- Choisir "Déploiement de bureaux basés sur une session".

- **Accès Web (RDWeb)** :

- Permet aux utilisateurs d'accéder à leurs applications/bureaux via un navigateur (URL type : https://serveur/RDWeb).
- **Sécurité (HTTPS)** : Nécessite impérativement un certificat SSL. En environnement de lab, on utilise souvent un certificat auto-signé qu'il faut lier au port 443 dans IIS.

- **Gestion des Certificats (MMC)** :

- Pour que les clients acceptent la connexion sans erreur de sécurité, ils doivent faire confiance au certificat du serveur RDS.
- **Export du certificat** : Sur le serveur, ouvrir la console mmc.exe > Ajouter un composant > Certificats > **Compte d'ordinateur** > Ordinateur Local. Exporter le certificat (sans la clé privée) pour le déployer ensuite.

- **Déploiement Automatisé via GPO** :

- Une GPO permet de distribuer le certificat et de configurer automatiquement la connexion RemoteApp sur les postes clients.
- **1. Distribution du Certificat (Confiance)** :

- _Chemin_ : Configuration Ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies de clé publique > **Autorités de certification racines de confiance**.
- _Action_ : Importer le certificat exporté précédemment.

- **2. Configuration du flux RemoteApp** :

- _Chemin_ : Configuration Utilisateur > Stratégies > Modèles d'administration > Composants Windows > Services Bureau à distance > Connexions aux programmes RemoteApp et aux services Bureau à distance.
- _Paramètre_ : **Spécifier l'URL de connexion par défaut**.
- _Valeur_ : https://ws2025.oclock.lan/rdweb/Feed/webfeed.aspx (URL du flux RSS/Webfeed).

- **Avantages et Inconvénients** :

- **Avantages** : Centralisation des données, maintenance simplifiée (1 seule installation d'app pour 50 utilisateurs), accès à distance sécurisé.
- **Inconvénients** : **SPOF** (Single Point of Failure) - si le serveur RDS plante, tous les utilisateurs sont bloqués. Nécessite une infrastructure serveur robuste (RAM/CPU).

```
(incomplet)
```

