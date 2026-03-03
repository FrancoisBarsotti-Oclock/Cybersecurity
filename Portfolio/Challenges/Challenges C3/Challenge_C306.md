# 🏆 Challenge C306: Sécurisation d’un serveur Debian exposé sur Internet
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous venez d’intégrer l’équipe infrastructure d’une mairie de votre région.
>
>Un nouveau serveur sous Debian doit être déployé en urgence pour héberger un futur service interne. Avant sa mise en production, l’équipe sécurité exige un durcissement minimal du système et une restriction stricte des accès SSH.
>
>Le responsable sécurité vous transmet les exigences suivantes :
>
>Votre mission consiste à préparer le serveur conformément aux exigences de sécurité de base.
>

---

👉 [Énoncé complet du challenge](https://github.com/O-clock-Aldebaran/SC3E06-ssh-hardening) 👈

Voir 👉 [Cours C306](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C3%20(S%C3%A9curit%C3%A9%20syst%C3%A8mes%20%26%20r%C3%A9seaux)/C306_Pare-feu%20Linux.md) 👈

### 📚 Ressources d'intérêt
* [Configuration firewall Linux](https://github.com/O-clock-Aldebaran/SC3-E06-demo-firewall)

* [Sécurité SSH](https://github.com/O-clock-Aldebaran/SC3-E06-demo-ssh-hardening)

---

## 🖥️ Environnement technique

| **Machine** | **Accès console** | **IP address** |
| --- | --- | --- |
| CT Debian 13.1 | root | `10.0.0.61` |


---

## 📌 Objectifs techniques

### 1️⃣ Installation et configuration de SSH ⚙️

* Installer le service **OpenSSH**
* Vérifier le bon fonctionnement du service
* S’assurer que le service démarre automatiquement au boot

Commandes proposées :

```bash
# Installation du serveur SSH
apt update -y && apt upgrade 
apt install -y openssh-server

# Vérification du statut et du port d'écoute
systemctl status ssh
ss -tlnp | grep ssh

# Activation au démarrage
systemctl enable ssh
```

![01-OpenSSHstatus](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_01-OpenSSHstatus.png)

---

### 2️⃣ Mise en place d’une règle de filtrage avec iptables ⚔️

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
iptables -A INPUT -p tcp -s 10.0.0.51 --dport 22 -j ACCEPT

# Vérification des règles
iptables -L -n -v

# Sauvegarde pour persistance
netfilter-persistent save
```

![02-VérificationRègles](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_02-V%C3%A9rificationR%C3%A8gles.png)

---

### 3️⃣ SSH Hardening (durcissement) 🔐

Mettre en place au minimum les mesures suivantes :

* Désactiver la connexion root directe
* Interdire l’authentification par mot de passe (authentification par clé uniquement)
* Limiter éventuellement les utilisateurs autorisés
* Modifier les paramètres inutiles ou dangereux par défaut
* Tester la configuration avant fermeture de session

## Côté machine cliente
### Sur CT Ubuntu (`10.0.0.51`) :

```bash
# Générer une paire de clés sur votre poste (si besoin)
ssh-keygen -t ed25519 -C "ubuntu@10.0.0.51"

# Garder sur /home/franbar/.ssh/id_ed25519 et créer le passphrase
```

![03-Création de clé](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_03-03-Cr%C3%A9ation%20de%20cl%C3%A9.png)


### Copie de clé publique sur le serveur (vers Debian `10.0.0.61`)
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub franbar@10.0.0.61
```

![04-Copie de clé](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_04-Copie%20de%20cl%C3%A9.png)

### Une vérification de connexion s'impose

![05-TestConnexionSSH](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_05-TestConnexionSSH.png)

→ Le fait qu'il ne demande que "Enter passphrase for key" veut dire que:

* ✅ La clé privée est utilisée
* ✅ la clé publique est acceptée côté Debian
* ✅ l’authentification par mot de passe n’est plus utilisée

On est donc bien en authentification par clé. Par sécurité, on laisse la passphrase en place.

## Côté Serveur OpenSSH

### Vérification de droits

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```



### Éditer la configuration SSH sur Debian (`10.0.0.61`)

```bash
# Pour désactiver l'authentification par mot de passe
nano /etc/ssh/sshd_config 
```

Paramètres recommandés à appliquer dans `sshd_config` :

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers franbar@10.0.0.51
```

Puis recharger (avec un `sudo systemctl restart ssh`) et tester :

```bash
# Vérifier la syntaxe (voir qu'il n'y ait aucune sortie)
sudo sshd -t

# Ou bien afficher la configuration effective complète
sudo sshd -T

# Recharger la configuration
sudo systemctl reload ssh

# Tester une connexion en conservant une session ouverte
ssh franbar@10.0.0.61
```

![06-TestRe-ConnexionSSH](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_06-TestRe-ConnexionSSH.png)

---

## 🎓 Bonus (facultatif si temps restant)

### Changer le port SSH

```bash
# Modification du port SSH
sudo nano /etc/ssh/sshd_config

# Décommenter et changer Port 22 par 2222
Port 2222

# Tester la config AVANT restart
sudo sshd -t  # attendu: aucune sortie

# Suppression de l'ancienne règle
sudo iptables -D INPUT -p tcp -s 10.0.0.51 --dport 22 -j ACCEPT

# Ajout de nouvelle règle
sudo iptables -A INPUT -p tcp -s 10.0.0.51 --dport 2222 -j ACCEPT

# Sauvegarde des règles persistantes
sudo netfilter-persistent save

# Vérification du statut et du port d'écoute
systemctl status ssh
ss -tlnp | grep ':2222'
sudo grep ^Port /etc/ssh/sshd_config
sudo grep -nE '^(Port|ListenAddress|AllowUsers|Match)' /etc/ssh/sshd_config

# Rédemmarage SSH
sudo systemctl restart ssh
sudo systemctl status ssh --no-pager
```
![07-Port2222](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_07-Port2222.png)

### Mise en place d'une politique iptables plus complète (ESTABLISHED, RELATED)

⚒️ En construction... 

### Mise en place d’un utilisateur admin avec sudo

Cela a été fait tout au début, avec la création de l'utilisateur franbar avec sudo.

```bash
adduser franbar
apt update
apt install sudo
usermod -aG sudo franbar
```

Voici la preuve 😏👇

![08-SudoUser](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C306/Challenge%20C306_08-SudoUser.png)

#