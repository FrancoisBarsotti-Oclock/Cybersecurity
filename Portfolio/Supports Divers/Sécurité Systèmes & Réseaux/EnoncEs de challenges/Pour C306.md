# Pour Challenge C306 :🛡️ Sécurisation d’un serveur Debian exposé sur Internet

## 🎯 Contexte

Vous venez d’intégrer l’équipe infrastructure d’une mairie de votre région.

Un nouveau serveur sous **Debian** doit être déployé en urgence pour héberger un futur service interne.
Avant sa mise en production, l’équipe sécurité exige un **durcissement minimal du système** et une **restriction stricte des accès SSH**.

Le responsable sécurité vous transmet les exigences suivantes :

Votre mission consiste à préparer le serveur conformément aux exigences de sécurité de base.

---

## 🖥️ Environnement technique

* 1 machine virtuelle vierge sous Debian (installation minimale)
* Accès console root
* Pour vos tests, la machine ne sera a ccessible que depuis votre propre IP

---

## 📌 Objectifs techniques

### 1️⃣ Installation et configuration de SSH

* Installer le service **OpenSSH**
* Vérifier le bon fonctionnement du service
* S’assurer que le service démarre automatiquement au boot

Commandes proposées :

```bash
# Installation du serveur SSH
apt update
apt install -y openssh-server

# Vérification du statut et du port d'écoute
systemctl status ssh
ss -tlnp | grep ssh

# Activation au démarrage
systemctl enable ssh
```

---

### 2️⃣ Mise en place d’une règle de filtrage avec iptables

Configurer le pare-feu avec **iptables** afin de :

* ✅ Autoriser le trafic SSH (port 22) **uniquement depuis votre IP**
* ❌ Refuser toute autre tentative de connexion SSH
* ✅ Maintenir l’accès local (loopback)
* ❌ Bloquer par défaut les connexions entrantes non autorisées

Les règles doivent :

* Être persistantes après redémarrage
* Être correctement ordonnées
* Être vérifiables

Commandes proposées (remplacer `X.X.X.X` par votre IP publique) :

```bash
# Pré-requis : installation de la persistance des règles
apt install -y iptables-persistent

# Nettoyage des règles existantes
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

# Politique par défaut : tout bloquer en entrée
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# Autoriser le loopback
iptables -A INPUT -i lo -j ACCEPT

# Autoriser les connexions déjà établies (recommandé)
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Autoriser SSH uniquement depuis votre IP
iptables -A INPUT -p tcp -s X.X.X.X --dport 22 -j ACCEPT

# Vérification des règles
iptables -L -n -v

# Sauvegarde pour persistance
netfilter-persistent save
```

---

### 3️⃣ SSH Hardening (durcissement)

Mettre en place au minimum les mesures suivantes :

* Désactiver la connexion root directe
* Interdire l’authentification par mot de passe (authentification par clé uniquement)
* Limiter éventuellement les utilisateurs autorisés
* Modifier les paramètres inutiles ou dangereux par défaut
* Tester la configuration avant fermeture de session

Commandes proposées :

```bash
# Générer une paire de clés sur votre poste (si besoin)
ssh-keygen -t ed25519 -C "admin@mairie"

# Copier la clé publique sur le serveur
ssh-copy-id utilisateur@IP_DU_SERVEUR

# Éditer la configuration SSH côté serveur
nano /etc/ssh/sshd_config
```

Paramètres recommandés à appliquer dans `sshd_config` :

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers utilisateur
```

Puis recharger et tester :

```bash
# Vérifier la syntaxe
sshd -t

# Recharger la configuration
systemctl reload ssh

# Tester une connexion en conservant une session ouverte
ssh utilisateur@IP_DU_SERVEUR
```


---

## 🎓 Bonus (facultatif si temps restant)

* Changer le port SSH
* Mettre en place une politique iptables plus complète (ESTABLISHED, RELATED)
* Mise en place d’un utilisateur admin avec sudo

#