# 🧱🐧 Session C306. Pare-feu Linux.

Notions du jour:
* pare-feu netfilter (iptables, nftables et ufw)
* SSH hardening (key based auth, désactiver root & password login)

Comprendre le moteur avant les commandes
Notions du jour

* iptables
* nftables
* ufw

### Pourquoi ça compte
Sur un serveur Linux :
* Le pare-feu est le dernier garde-fou️ 🛡️
* Il protège les services exposés
* Il limite l'impact d'une erreur de config

Même avec un bon réseau, un serveur mal protégé reste vulnérable

### Le moteur : netfilter ⚙️

![01-Netfilter](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C3%20(S%C3%A9curit%C3%A9%20syst%C3%A8mes%20%26%20r%C3%A9seaux)/images%20C3/images%20C306/C306_01-Netfilter.png)

[Netfilter](https://fr.wikipedia.org/wiki/Netfilter) c'est le pare-feu intégré au **noyau Linux** 

### Que fait netfilter ?
Il intercepte les paquets réseau et applique des règles :

* ✅ **ACCEPT** - laisser passer
* ❌ **DROP** - jeter silencieusement
* 🚫 **REJECT** - refuser avec réponse
* 📝 **LOG** – journaliser

### Comment un paquet traverse la machine 📦
Pensez à un **aéroport**✈️

**Les chaînes = les zones de contrôle**

* **INPUT** = les arrivées 🛬

    Trafic qui arrive sur la machine
* **OUTPUT** = les départs 🛫

    Trafic qui part de la machine 
* **FORWARD** = le transit 🔄

    Trafic qui traverse (routeur/passerelle)

**Si l'on retient ça, on sati déjà 80% !**

#### Analogie : le videur de discothèque 🕺

* Le videur (**netfilter**) est à la porte 🚪
* Il a une **liste** (les règles) 📋
* Chaque personne (**paquet**) est vérifiée 🔎
* Soit tu entres ✅, soit tu dégages ❌ 

Et si tu n'es pas sur la liste ? Ça dépend de la **politique par défaut** !

## Les politiques par défaut 🎯
La question fondamentale :

**Policy par défaut = ACCEPT ou DROP ?**

### Règle d'or

>"Deny by default" - tout est interdit sauf ce qui est explicitement autorisé


* On met la policy à **DROP**
* On ouvre **uniquement** ce qui est nécessaire
* Principe du **moindre privilège**

Si on met DROP par défaut sans ajouter aucune règle, que se passe-t-il ?
Plus rien ne passe ! Même pas SSH… ⚠️

**Résultat** : vous venez de vous couper l’accès à votre propre serveur

C'est un classique. On en reparle dans les bonnes pratiques.

# Les 3 outils à connaître 🧰
## iptables - le vétéran ⚔️
*Syntaxe verbeuse mais **incontournable**
* Très présent en production
* Encore demandé en entretien d'embauche

Le couteau suisse historique du pare-feu Linux

### Exemples iptables

Autoriser SSH depuis une IP d'admin :
```ruby
iptables -A INPUT -s 192.168.1.100 -p tcp -- dport 22 -j ACCEPT
```

Autoriser HTTP et HTTPS pour tous :
```ruby
iptables -A INPUT -p tcp -- dport 80 -j ACCEPT
iptables -A INPUT -p tcp -- dport 443 -j ACCEPT
```

Politique par défaut - tout bloquer :
```ruby
iptables -P INPUT DROP
```

## nftables - le futur 🚀
* Unifie IPv4 et IPv6
* Syntaxe **plus lisible**
* Meilleures performances

Le **successeur officiel d'iptables**, mais pas encore partout.

#### Exemple nftables
 ```powershell
nft add table inet filter
nft add chain inet filter input { type filter hook input priority 0 \; policy drop\; }
nft add rule inet filter input tcp dport 22 accept
nft add rule inet filter input tcp dport { 80, 443 } accept
```

Plus propre! 👌

## ufw - le simplifié 🎯
* Interface simple au-dessus d'iptables
* Idéal pour un VPS ou un poste simple
* Pas adapté aux configs complexes

#### Exemple ufw
```bash
ufw default deny incoming
ufw allow from 192.168.1.100 to any port 22
ufw allow 80/tcp
ufw allow 443/tcp
ufw enable
```
5 lignes et c'est réglé ! Parfait pour débuter

## Cas d'usage typique 🎯
Objectif : sécuriser un serveur web

* ✅ Autoriser SSH depuis l'IP d'admin uniquement
* ✅ Autoriser HTTP/HTTPS pour tout le monde
* ❌ Tout le reste : **bloqué**

On va le faire ensemble en démo !️ 💻

## Bonnes pratiques terrain 📋
### Les réflexes à avoir

* ⚠️ Toujours tester en console locale avant d'appliquer en remote.
* 🔑 Ne jamais oublier SSH dans les règles avant de passer en DROP.
* 📝 Documenter chaque règle (pourquoi elle existe).
* 💾 Sauvegarder la config avant de modifier.

#### L'anecdote classique 🙆‍♂️
"Se couper l'accès SSH en remote sur un serveur en prod ... "

Ça arrive à **tout le monde** ... au moins une fois

**La parade** : toujours garder une session console ouverte pendant qu'on modifie les règles !

Ou mieux : utiliser un **cron job** qui restore les règles au bout de 5 minutes 🧠

👉 [Cron et crontab]( https://www.kinamo.fr/base-de-connaissanse/les-t%C3%A2ches-planifi%C3%A9es-sur-linux-en-utilisant-crontab) : les tâches planifiées sur Linux en utilisant crontab

### En résumé
* ⚙️ **netfilter** = le moteur (dans le noyau)
* ⚔️ **iptables** = la base à connaître
* 🚀 **nftables** = le moderne
* 🎯 **ufw** = le simplifie

Prochaine étape : [mise en pratique](https://github.com/O-clock-Aldebaran/SC3-E06-demo-firewall) !

# Récapitulatif des configurations firewall

Ce document présente des configurations de firewall simples et sécurisées pour un serveur exposé uniquement en SSH, avec trois outils différents : iptables, nftables et UFW.


## SSH sur machine locale

```sh
# Sur le serveur
sudo sed -i 's/^#\?PermitRootLogin .*/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart ssh
ip a

# en Local (se connecter en utilisant le mot de passe root)
ssh -o PubkeyAuthentication=no root@10.0.0.99
```

---

## 1️⃣ iptables

### Installation de la persistance

```bash
# Installer le paquet pour sauvegarder les règles iptables au redémarrage
apt install -y iptables-persistent
```

### Nettoyage des règles existantes

```bash
# Vider toutes les chaînes de la table filter (INPUT, OUTPUT, FORWARD)
iptables -F

# Supprimer les chaînes personnalisées
iptables -X

# Vider les chaînes de la table nat
iptables -t nat -F
iptables -t nat -X
```

### Politique par défaut

```bash
# Bloquer tout le trafic entrant par défaut
iptables -P INPUT DROP

# Bloquer le transit de paquets entre interfaces
iptables -P FORWARD DROP

# Autoriser tout le trafic sortant
iptables -P OUTPUT ACCEPT

# Vérifier les politiques par défaut
iptables -L -n --line-numbers -v
```

### Autoriser le loopback

```bash
# Autoriser les communications internes (127.0.0.1)
iptables -A INPUT -i lo -j ACCEPT
# -A (Append) c’est tout à la fin

iptables -I INPUT -i lo -j ACCEPT
# -I (Insert) c’est tout au début

# à chaque fois il faut faire une sauve garde des règles
netfilter-persistent sabe

# Même si pas nécessaire, un test de redémarrage sera positif
reboot
```

### Autoriser les connexions déjà établies

```bash
# Permettre les réponses aux connexions déjà initiées (ex: réponses HTTP, DNS...)
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

### Autoriser SSH uniquement depuis une IP spécifique

```bash
# Remplacer 10.42.0.2 par l'adresse IP autorisée (ex: poste administrateur)
iptables -A INPUT -p tcp -s 10.42.0.2 --dport 22 -m conntrack --ctstate NEW -j ACCEPT
```

### Autoriser les connexions HTTP/HTTPS (optionnel, si besoin d'exposer un service web)

```bash
# Autoriser HTTP (port 80) et HTTPS (port 443) depuis n'importe quelle IP
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

### Optionnel : drop des paquets invalides

```bash
# Rejeter les paquets dans un état de connexion invalide (protection supplémentaire)
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
```

### Vérification et sauvegarde

```bash
# Afficher les règles actives avec statistiques
iptables -L -n -v

#Regarder les règles numérotées
iptables -v -n -L –Line-numbers

#Disons que l’on veut enlèver la 3ème règle (qui est en input), ce sera :
Iptabes -D input 3


# Sauvegarder les règles pour qu'elles persistent après redémarrage
netfilter-persistent save
```

### Côté hôte
```
# Pour se connecter en SSH
ssh ...
```

---

## 2️⃣ nftables

### Installation et activation

```bash
# Installer nftables
apt install -y nftables

# Activer et démarrer le service nftables
systemctl enable nftables
systemctl start nftables

# Vérification de son activation
systemctl status nftables
```

### Exemple de configuration (`/etc/nftables.conf`)

(avec un nano)
```bash
#!/usr/sbin/nft -f

# Supprimer toutes les règles existantes avant d'appliquer la nouvelle configuration
flush ruleset

table inet filter {

    chain input {
        # Politique par défaut : bloquer tout le trafic entrant
        type filter hook input priority 0; policy drop;

        # Autoriser le trafic loopback (communications internes)
        iif lo accept

        # Autoriser les connexions déjà établies ou liées
        ct state established,related accept

        # Autoriser SSH uniquement depuis l'IP 10.42.0.2 (remplacer par l'IP réelle)
        tcp dport 22 ip saddr 10.42.0.2 ct state new accept

        # Autoriser les connexions HTTP/HTTPS (optionnel)
        tcp dport { 80, 443 } accept

        # Rejeter les paquets avec un état de connexion invalide
        ct state invalid drop
    }

    chain forward {
        # Politique par défaut : bloquer le transit de paquets
        type filter hook forward priority 0; policy drop;
    }

    chain output {
        # Politique par défaut : autoriser tout le trafic sortant
        type filter hook output priority 0; policy accept;
    }
}
```

### Vérification et persistance

```bash
# Afficher le ruleset actif
nft list ruleset

# Charger la configuration depuis le fichier
nft -f /etc/nftables.conf

# Sauvegarder le ruleset actuel dans le fichier de configuration
nft list ruleset > /etc/nftables.conf
```

---

## 3️⃣ UFW

### Installation et activation

```bash
# Installer UFW (Uncomplicated Firewall)
apt install -y ufw

# Activer UFW au démarrage et appliquer les règles
ufw enable
```

### Réinitialiser les règles existantes

```bash
# Supprimer toutes les règles existantes et désactiver UFW temporairement
ufw reset
```

### Politique par défaut

```bash
# Bloquer tout le trafic entrant par défaut
ufw default deny incoming

# Bloquer le routage/transit de paquets
ufw default deny routed

# Autoriser tout le trafic sortant
ufw default allow outgoing
```

### Autoriser le loopback (optionnel, géré par défaut)

```bash
# Autoriser explicitement le trafic sur l'interface loopback
ufw allow in on lo
```

### Autoriser SSH uniquement depuis une IP spécifique

```bash
# Remplacer 10.42.0.2 par l'adresse IP autorisée (ex: poste administrateur)
ufw allow from 10.42.0.2 to any port 22 proto tcp
```

### Autoriser les connexions HTTP/HTTPS (optionnel, si besoin d'exposer un service web)

```bash
# Autoriser HTTP (port 80) et HTTPS (port 443) depuis n'importe quelle IP
ufw allow 80/tcp
ufw allow 443/tcp
```

### Vérification

```bash
# Afficher le statut d'UFW et toutes les règles actives
ufw status verbose
```

### Côté hôte

```bash
# Pour se connecter en SSH
ssh root@<ipcible>

# ou bien, avec un débug (mode verbose)
ssh -vvv root@<ipcible>
```

---

## Notes et bonnes pratiques

- Testez toujours votre connexion SSH avant d'appliquer des règles DROP, sinon vous risquez de vous bloquer.
- Les connexions établies sont gérées automatiquement par nftables et UFW.
- Pour plus de sécurité, vous pouvez ajouter un blocage des paquets invalides ou limiter les tentatives SSH (anti-bruteforce).
- nftables est plus moderne et lisible, recommandé pour les nouvelles installations.

# Authentification forte & SSH hardening 🔐
Moins d'exposition, plus de contrôle

### Pourquoi SSH est une cible 🎯

### SSH, c'est la porte d'entrée
- 🖥️ Administration à distance des serveurs
- ⚙️ Automatisation (scripts, déploiements, CI/CD)
- 📂 Transfert de fichiers (SCP, SFTP)
- 🔗 Tunnels sécurisés

**SSH est partout** → c'est la cible n°1 des attaquants

### Ce que voit un attaquant
Un serveur avec le port 22 ouvert, c'est comme une porte avec une pancarte "Essayez d'entrer !" 🚪

- 🤖 Bots qui scannent en permanence (Shodan, Masscan ... )
- 💥 Attaques brute-force automatisées (24h/24)
- 📋 Dictionnaires de mots de passe courants

En moyenne, un serveur exposé sur Internet reçoitses premièrestentatives SSH en moins de **10 minutes**!😱

### Question 🤔 
Quels sont les noms d'utilisateurs les plus testés par les bots ?

**root**, admin, test, user, ubuntu, postgres, oracle ...

Et oui, root est de loin le plus ciblé ! D'où l'intérêt de le désactiver ...

## Authentification par mot de passe 🔑
Le maillon faible

### Comment ça marche (par defaut)
1. Le client se connecte au serveur SSH
2. Le serveur demande un mot de passe
3. L'utilisateur tape son mot de passe
4. Le serveur vérifie - accès accordé ou refusé

Simple, intuitif ... mais plein de failles⚠️ 

### Les problèmes du mot de passe
* 🔁 **Réutilisé** sur plusieurs services (un leak = tout compromis)
* 🧠 **Devinable** (prénom + année, nom du chat ... )
* 💥 **Brute-forçable** (les bots testent des milliers de combinaisons)
* 📧 **Phishable** (on peut le voler par ingénierie sociale)

Même un "bon" mot de passe reste vulnérable si c'est la seule protection.

### Analogie 🏠

Un mot de passe, c'est comme une serrure à code sur votre porte d'entrée

Quelqu'un qui regarde par-dessus votre épaule peut le voir👀

Quelqu'un de patient peut essayer toutes les combinaisons🔢

**Il nous faut quelque chose de plus robuste !**

# Les clés SSH : le vrai standard ️🗝️

La pièce maîtresse du hardening SSH

## Le principe : cryptographie asymétrique
* 🔑 On génère un **couple de clés** : une **privée** + une **publique**
* 🔒 La clé **publique** va sur le serveur
* 🏠 La clé **privée** reste chez vous (et ne sort **jamais**)

**Analogie** : la clé publique, c'est un cadenas ouvert que vous donnez au serveur. Seule votre clé privée peut l'ouvrir 🔐

### Comment ça fonctionne
1. Le client se connecte au serveur SSH
2. Le serveur envoie un **défi chiffré** avec la clé publique
3. Le client prouve qu'il possède la clé privée (sans la transmettre !)
4. Le serveur vérifie - accès accordé ✅

Le mot de passe n'est jamais transmis sur le réseau - impossible à intercepter !

### Générer une paire de clés
```powershell
ssh-keygen -t ed25519 -C "admin@monserveur"
```
* **-t ed25519** : algorithme moderne et sûr
* **-C** : commentaire pour identifier la clé

On peut aussi utiliser rsa (4096 bits minimum), mais **ed25519** est préféré aujourd'hui

### Ce que ça produit
```ruby
~/.ssh/id ed25519  ← clé privée 🔐 (NE JAMAIS PARTAGER)
~/.ssh/id_ed25519.pub   ← clé publique 🔓 (à copier sur le serveur)
```
Lors de la génération, on vous demande une **passphrase**

La passphrase protège la clé privée en cas de vol du fichier. C'est du **2FA du pauvre** : quelque chose qu'on **a** (la clé) + quelque chose qu'on **sait** (la passphrase)

### Copier la clé publique sur le serveur
**La méthode simple** :
```ruby
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@serveur
# ~ c’est un alias du dossier home ou root
```

**La méthode manuelle** :
```bash
cat ~/.ssh/id_ed25519.pub >>~/.ssh/authorized_keys
```

La clé publique est ajoutée dans ~/. ssh/authorized_keys sur le serveur

### Les permissions, c'est critique ! ⚠️
```ruby
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id ed25519
```

Si les permissions sont trop ouvertes, SSH **refuse** la connexion !

C'est une protection : si quelqu'un d'autre peut lire votre clé, elle n'est plus fiable

[Rappel de la gestion des droits sur Linux](https://www.it-connect.fr/les-droits-sous-linux/)

👉 [Image recapitulative des droits sur Linux](https://www.reddit.com/media?url=https%3A%2F%2Fpreview.redd.it%2Fvkxuqbatopk21.png%3Fauto%3Dwebp%26s%3D81f97dac1e1ceb5054ee43cbe96ec6fa55215695) 👈

### Tester la connexion par clé
```ruby
ssh -i ~/.ssh/id ed25519 user@serveur
```

Si tout est bien configuré : connexion **sans mot de passe** !

Si ça demande encore un mot de passe, il faudra vérifier les permissions et le fichier `authorized_keys`

### Ce que ça change au quotidien 💡
* 🛡️ Élimine **90% des attaques automatiques** (brute-force inutile)
* 🔑 Pas de mot de passe à retenir (ni à réutiliser)
* 🚫 On peut **révoquer** une clé sans toucher aux autres
* 📋 On sait **qui** a accès (une clé = une personne)

Un collaborateur quitte l'équipe ? On supprime sa clé publique du serveur. 
C'est tout.

## Hardening du serveur SSH 🔧

On verrouille `sshd_çconfig`

> Le gestionnaire de clés SSH est appelé [SSH Agent](https://smallstep.com/blog/ssh-agent-explained/)
>
> Et voici une bonne procédure pour l'instaler, si nécessaire 👉 [Generating a new SSH key and adding it to the ssh-agent](https://smallstep.com/blog/ssh-agent-explained/)

### Le fichier magique
Toute la configuration du serveur SSH est dans un seul fichier :
```yaml
/etc/ssh/sshd_config
```

⚠️Ne pas confondre avec ssh_config (config du **client**) !

_sshd_config = serveur (le d = daemon) / ssh_config = client_

### Étape 1 : Désactiver le login root 🚫
```yaml
PermitRootLogin no
```

#### Pourquoi ?

* root est le compte **le plus ciblé** par les bots
* root a **tous les droits** → impact maximal si compromis
* On perd la **traçabilité** (qui s'est connecté ?)

_Alternative : on se connecte avec un compte nominatif, puis sudo pour les tâches admin_

### Les options de PermitRootLogin

* no : aucune connexion root en SSH ✅ (recommandé)
* prohibit-password : root uniquement par clé (pas de mot de passe)
* yes : root peut se connecter comme il veut ❌  (à éviter !)

_prohibit-password est un compromis acceptable dans certains cas (automatisation, Ansible ... )_

### Étape 2 : Désactiver le mot de passe 🔒
```yaml
PasswordAuthentication no
```

#### Pourquoi ?

* Les clés sont en place - le mot de passe ne sert plus à rien
* On coupe définitivement le brute-force
* Même un mot de passe volé ne permet plus de se connecter

 ⚠️ **Rappel de Tester la clé AVANT de couper le mot de passe !**

### Étape 3 : Activer l'authentification par clé
```yaml
PubkeyAuthentication yes
```

_C'est généralement activé par défaut, mais on le met explicitement pour être sûr_

Et… On restart le serveur ssh

### Étape 4 : Durcissement supplémentaire 🛡️

D'autres paramètres utiles :

```yaml
# Limiter les utilisateurs autorisés
AllowUsers admin deployer

# Changer le port (sécurité par obscurité)
Port 2222  

# → On peut même l’activer pour la connexion d’une seule IP
ufw allow from 10.42.0.3 to any port 2222 proto tcp

# Timeout de session inactive (déconnexion automatique)
ClientAliveInterval 300
ClientAliveCountMax 2

# Désactiver les méthodes inutiles
ChallengeResponseAuthentication no
UsePAM no
```

### Récapitulatif : le sshd_config durci 📋

```yaml
# === Authentification ==
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no

# === Accès ===
AllowUsers admin deployer
MaxAuthTries 3
LoginGraceTime 30

# === Sécurité réseau ===
Port 2222
X11Forwarding no  	# ancient serveur graphique
AllowTcpForwarding no
```

Chaque ligne est **justifiable** : c'est ça la gouvernance en action !💡

### Appliquer les changements
```yaml
# Vérifier la syntaxe AVANT de redémarrer !
sudo sJhd -t

# Si pas d'erreur - redémarrer le service
sudo systemctl restart sshd
```

⚠️ **Garder toujours une session ouverte** pendant que l’on teste !

_Si la config est cassée et que vous fermez la session ... vous êtes enfermé dehors !_ 🔒😱

## La procédure complète, pas à pas 📝

_Le workflow sécurisé pour ne rien casser_

### Étape 1 : Préparer
* ✅ Ouvrir deux sessions SSH (une de secours)
* ✅ S'assurer d'avoir un acces console (Proxmox, IPMI ... )
* ✅ Avoir les droits sudo sur un compte non-root

### Étape 2 : Générer et copier la clé
```ruby
# Sur le poste client
ssh-keygen -t ed25519 -C "admin@monserveur"
ssh-copy-id -i ~/.ssh/id_ed25519.pub admin@serveur
```

### Étape 3 : Tester la clé

```yaml
# Toujours tester AVANT de modifier la config !
ssh -i ~/.ssh/id_ed25519 admin@serveur
```
Si ça marche → on continue. Sinon → on corrige **avant** de toucher à sshd_config

### Étape 4 : Modifier sshd_config
```yaml
sudo nano /etc/ssh/sshd_config
```

Paramètres à modifier :
```yaml
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

### Étape 5 : Vérifier et appliquer
```yaml
# Vérifier la syntaxe
sudo sshd -t

# Redémarrer le service
sudo systemctl restart sshd
```

### Étape 6 : Tester (sur la 2e session !)
```yaml
# Depuis un NOUVEAU terminal (sans fermer l'ancien !)
ssh admin@serveur
```

✅ Connexion par clé - parfait !

Vérifier que le mot de passe est bien refusé :
```yaml
ssh -o PubkeyAuthentication=no admin@serveur
# - Permission denied (publickey)
```
✅Accès refusé → mission accomplie !🎉

## Les pièges classiques 😬

_Ce qui peut mal tourner (et comment l'éviter)_

### Piège n°1 : Se verrouiller dehors 🔒
Vous coupez le mot de passe avant d'avoir testé la clé...
**Résultat** : plus aucun moyen de se connecter en SSH😱

**La parade** : toujours tester la clé dans une **nouvelle session** avant de modifier quoi que ce soit !

### Piège n°2 : Les permissions 🔐
SSH esttrès strict sur les permissions des fichiers
* ~/.ssh/ doit être en **700**
* authorized_keys doit être en **600**
* Le dossier home ne doit pas être en écriture pour le groupe

_Si les permissions sont mauvaises, SSH refuse silencieusement la clé et demande un mot de passe_

### Piège n°3 : SELinux / AppArmor ️
Sur certaines distributions, SELinux peut bloquer l'accès au fichier `authorized_keys`

```yaml
# Restaurer le contexte SELinux
restorecon -Rv ~/.ssh/
```
_Si tout semble bon mais que ça ne marche pas → vérifiez SELinux !_

### Piège n°4 : Oublier de redémarrer sshd 🔄

Vous modifiez sshd_config mais oubliez de redémarrer le service
**Résultat** : l'ancienne config est toujours active... jusqu'au prochain reboot surprise💥
```
sudo systemctl restart sshd
# ou
sudo systemctl reload sshd # moins brutal
```

## Gestion des clés en équipe 👥
_Quand on n'est pas tout seul_

### Un utilisateur = un compte = une clé
* 👤 Chaque admin a son propre compte Unix
* 🔑 Chaque admin a sa propre clé SSH
* 📋 Chaque clé est dans le authorized_keys du bon utilisateur

**Jamais** de clé partagée entre plusieurs personnes !🚫

Si on partage une clé, on perd la traçabilité : qui s'est connecté ?

### Révoquer un accès
Un collaborateur quitte l'équipe ?

1. Supprimer sa clé publique de authorized_keys
2. Supprimer ou désactiver son compte Unix
3. Vérifier les logs pour s'assurer qu'il ne se connecte plus

Avec des mots de passe partagés, il faudrait changer le mot de passe partout ... Avec des clés, on supprime une ligne. C'est tout ! ✅

### Automatisation & clés de service 🤖

* Les scripts et outils (Ansible, CI/CD ... ) utilisent aussi des clés SSH
* Ces clés de service doivent être séparées des clés personnelles
* On peut restreindre une clé à une commande précise :

```powershell
# Dans authorized_keys, préfixer la clé :
command="/usr/bin/rsync -- server ... " ssh-ed25519 AAAA ...
```

_Cette clé ne peut exécuter que rsync, même si l'attaquant la vole !_

## Récap’ : avant/ après 📊

### Avant le hardening ❌
* 🔓 root peut se connecter en SSH
* 🔑 Mot de passe = seule authentification
* 🤖 Bots qui brute-forcent 24h/24
* 😰 Aucune traçabilité

### Après le hardening
* 🚫 root interdit en SSH
* ️🗝️ Authentification par cle uniquement
* 🛡️ Brute-force inutile (pas de mot de passe à deviner)
* 📋 Chaque accès est identifié et révocable

**Même config, même serveur, sécurité radicalement différente !**

### Bonnes pratiques terrain 📋

* ⚠️ **Toujours garder un acces console de secours** (Proxmox, IPMI, KVM ... )
* 🧪 **Tester la cle AVANT de couper le mot de passe**
* 🔐 **Protéger la cle privee** avec une passphrase
* 📝 **Documenter** qui a accès à quoi
* 🔄 **Auditer regulierement** les fichiers authorized_keys

_Un serveur sécurisé, c'est un serveur dont on sait exactement qui y a accès_

# Passons à la pratique: Sécurité SSH

[Sécurité SSH](https://github.com/O-clock-Aldebaran/SC3-E06-demo-ssh-hardening)

# Sécurité SSH

## Prérequis

Prendre la machine sur laquelle était déjà installé UFW (voir démo précédente) et s'assurer que le port SSH est ouvert :

```yaml
ufw allow 22/tcp
```

## Générer une paire de clés SSH

```powershell
ssh-keygen -t ed25519 -C "admin@monserveur"
```

## Copier la clé publique sur le serveur

```ruby
# Remplacer par l'IP ou le nom de votre serveur
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@10.0.0.100
```

## Gérer les permissions du dossier .ssh

```yaml
# Sur le serveur, vérifier les permissions du dossier .ssh
ls -ld ~/.ssh
# Si nécessaire, corriger les permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519
```

## Tester la connexion SSH

```powershell
ssh -o PubkeyAuthentication=yes root@10.0.0.100
```

## Création d'un nouvel utilisateur avec accès SSH

```bash
# Sur le serveur, créer un nouvel utilisateur
sudo adduser admin
sudo usermod -aG sudo admin
# Copier la clé publique pour le nouvel utilisateur
sudo mkdir -p /home/admin/.ssh
sudo touch /home/admin/.ssh/authorized_keys
sudo cp ~/.ssh/authorized_keys /home/admin/.ssh/authorized_keys
sudo chown -R admin:admin /home/admin/.ssh
sudo chmod 700 /home/admin/.ssh
sudo chmod 600 /home/admin/.ssh/authorized_keys
```

## Modifier la configuration SSH

```yaml
# Sur le serveur, éditer le fichier de configuration SSH
sudo nano /etc/ssh/sshd_config

# Exemple de modifications recommandées :
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers admin

# Redémarrer le service SSH pour appliquer les changements
sudo systemctl restart ssh
```

## Vérifier que le mot de passe est désactivé

```powershell
ssh -o PubkeyAuthentication=no admin@10.0.0.100
# Permission denied (publickey).
```

## Durcissement supplémentaire (optionnel)

```yaml
# Limiter les tentatives de connexion pour prévenir les attaques par force brute
MaxAuthTries 3
LoginGraceTime 30
# Changer le port
Port 2222
# Timeout pour les connexions inactives
ClientAliveInterval 300
ClientAliveCountMax 0
# Désactiver les méthodes inutiles
ChallengeResponseAuthentication no
UsePAM no
X11Forwarding no
AllowTcpForwarding no

## Ne pas oublier de mettre à jour les règles de pare-feu si vous changez le port SSH
ufw allow 2222/tcp

## Redémarrer le poste après les modifications (changement du port SSH, etc.)
reboot
```

## Vérifier la configuration

```yaml
# Vérifier la syntaxe du fichier de configuration SSH
sudo sshd -t

# Redémarrer le service SSH après les modifications
sudo systemctl restart ssh
```

Challenge du jour 👉 [Challenge C306](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/Challenge_C306.md) : Sécurisation d’un serveur Debian exposé sur Internet

#