# Session C3E07. PAM & fail2ban

Notions du jour:
* PAM
* commandes last & lastb
* fail2ban
* port-knocking

# PAM sur Linux 🧩
_Comprendre quis'authentifie, et comment_

### Le problème sans PAM
Sur Linux, beaucoup de services doivent authentifier un utilisateur : SSH, sudo, login local, su, cron ...

Sans PAM, chaque service aurait sa propre logique, ses propres règles, ses propres bugs

Imaginez devoir configurer les règles de mot de passe séparément dans SSH, sudo, login, su ... 🙆‍♂️

[Lien démo PAM](https://github.com/O-clock-Aldebaran/SC3-E07-demo-pam)

### La solution : un système unique
PAM (**Pluggable Authentication Modules**) permet de brancher des **modules d'authentification** de maniere coherente sur tout le système

_**Analogie** : PAM, c'est un standard de prise électrique🔌— tous les  appareils (services) utilisent le même format_

### Ce que PAM contrôle vraiment 🎯

PAM ne fait pas que "valider un mot de passe". Il intervient dans **4 phases** distinctes :

### Les 4 types de modules
* **auth** - vérifier l'identité (mot de passe, clé, MFA ... )
* **account** - vérifier si le compte a le droit d'entrer (expiration, groupe, horaires)
* **password** - imposer des règles lors du changement de mot de passe
* **session** - appliquer des règles au démarrage/fin de session (logs, variables, quotas)

_Chaque type répond à une question différente sur l'utilisateur_

#### Exemple terrain : connexion SSH
Un utilisateur se connecte en SSH :

1. **auth** → PAM vérifie son identité (mot de passe ? clé ?)
2. **account** → le compte est-il autorisé ? expiré ?
3. **session** → on applique les limites et on journalise

Même logique pour SSH, sudo ou un login console : c'est la force de PAM 💪 

### La logique des modules 🧠
### Un module = une tâche précise
Un module PAM fait une seule chose :

* pam_unix → vérifier un mot de passe local
* pam_ldap → interroger un annuaire LDAP
* pam_faillock → bloquer après X échecs
* pam_pwquality → imposer une complexité de mot de passe
* pam_limits → quotas et limites de ressources

On **chaîne** ces modules pour construire une politique de sécurité

### Les modules les plus fréquents

| **Module** | **Rôle** |
| --- | --- |
| pam_unix | Auth locale classique (passwd/shadow) |
| pam_ldap / pam_sss | Auth via annuaire (LDAP, AD) |
| pam_faillock | Verrouillage après échecs |
| pam_pwquality | Règles de complexité de mot de passe |
| pam_limits | Quotas / limites de ressources |
| pam_wheel | Restreindre su à un groupe |
| pam time | Restreindre l'accès à certaines heures |

### Où se trouve la config
PAM est configuré dans /etc/pam. d/. Chaque service a son propre fichier :

```swift
/etc/pam.d/
├── sshd ← config PAM pour SSH
├── login ← config PAM pour le login console
├── sudo ← config PAM pour sudo
├── su ← config PAM pour su
└── common-* ← règles partagées (Debian/Ubuntu)
```

Le contenu du fichier décide **qui passe et dans quelles conditions**

### Anatomie d'une ligne PAM 🧾

### Le format
Chaque ligne suit ce format :
```ps
Type  control  module  [arguments]
```
Exemple :
```swift
Auth  required   pam_unix.so
```

→ Pour l'authentification, le module pam_unix est **obligatoire**

### Les control flags ⚙️

Les plus courants :

| **Flag** | **Comportement** |
| --- | --- |
| **required** | Obligatoire, mais continue la chaîne même en cas d'échec |
| **requisite** | Obligatoire, **stop immédiat** si échec |
| **sufficient** | Suffit si succès (court-circuite la suite) |
| **optional** | Pris en compte uniquement si aucun autre n'a tranché |

### Exemples concrets 📌

### Fichier PAM — SSH 🔐
Extrait simplifié de /etc/pam.d/sshd :

```go
auth required pam_faillock.so preauth silent deny=5 unlock_time=300
auth required pam_unix.so
auth [default=die] pam_faillock.so authfail deny=5 unlock_time=300
account required pam_unix.so
session required pam_limits.so
```
**Logique** : on bloque après 5 échecs, on vérifie le mot de passe, on applique les limites de session

### Fichier PAM — sudo 🧑‍💻
Extrait simplifié de /etc/pam.d/sudo :
```apache
auth required pam_unix.so
account required pam_unix.so
session required pam_limits.so
```

Plus simple que SSH : sudo s'appuie sur l'auth locale

### Fichier PAM — su (restreint au groupe wheel) 🔒

```apache
auth required pam_wheel.so
auth required pam_unix.so
account required pam_unix.so
```
pam_wheel → seuls les membres du groupe wheel peuvent utiliser su

Un classique du hardening Linux !

## Cas pratique : politique de mot de passe 🔑

## Forcer la complexité avec pam_pwquality

Dans /etc/pam.d/common-password (Debian/Ubuntu) ou /etc/pam.d/system-auth (RHEL) :
```swift
password required pam_pwquality.so minlen=12 ucredit=-1 lcredit=-1 dcredit=-
password required pam_unix.so use_authtok sha512
```
Traduction :

* **minlen=12** : 12 caractères minimum
* **ucredit=-1** : au moins 1 majuscule
* **lcredit=-1** : au moins 1 minuscule
* **dcredit=-1** : au moins 1 chiffre
* **ocredit=-1** : au moins 1 caractère spécial

### Verrouillage avec pam_faillock

```swift
auth required pam_faillock.so preauth silent deny=5 unlock_time=300
auth required pam_unix.so
auth [default=die] pam_faillock.so authfail deny=5 unlock_time=300
```
* **deny=5** : verrouillage après 5 échecs
* **unlock_time=300** : déverrouillage automatique après 5 minutes

Vérifier l'état d'un compte :
```ps
faillock --user admin
```
Déverrouiller manuellement :

```ps
faillock --user admin --reset
```

### Ce que PAM permet (vraiment) ✅
* Forcer des mots de passe solides partout
* Bloquer les brute-force sur tous les services
* Restreindre des comptes à certaines heures (pam_time)
* Intégrer un annuaire (LDAP/AD) de manière transparente
* Limiter les ressources par utilisateur (pam_limits)
* Appliquer des règles **identiques** à SSH, sudo, login, su ...

_Un seul endroit pour gouverner toute l'authentification du système_🎯

### Erreurs classiques à éviter ! ⚠️

* ❌ Modifier /etc/pam. d/sshd **sans garder une session ouverte**
* ❌ Empiler des modules sans comprendre l'**ordre d'exécution**
* ❌ Oublier l'accès **console** en cas de blocage
* ❌ Confondre required et requisite (comportement très différent !)
* ❌ Tester en prod sans avoir valide en local

_Toujours garder un accès de secours quand on touche à PAM !_🔒

### Ce qu'il faut retenir 🧠
PAM est une brique **invisible mais essentielle**

* 4 types : **auth, account, password, session**
* Config dans /etc/pam.d/ (un fichier par service)
* Modules chaînés avec des control flags (**required, requisite, sufficient, optional**)
* Permet de centraliser et homogénéiser la sécurité d'authentification

_Si on comprend PAM, on peut sécuriser l'ensemble du système avec des règles claires et maintenables._

# Logs, fail2ban & port-knocking 🔒
Voir, bloquer, réduire l'exposition

## Pourquoi ces trois sujets vont ensemble 🔍
### Sur un serveur exposé, il faut :

1. **Voir** ce qui se passe (logs)
2. **Réagir** aux attaques répétées (fail2ban)
3. **Réduire** les points d'entrée visibles (port-knocking)

_Trois couches complémentaires de durcissement_

## Partie 1 : les logs de connexion 🧾
_Avant de défendre, il faut comprendre_

### last : qui s'est connecté ?
La commande last lit /var/log/wtmp - la mémoire des connexions réussies
```yaml
last
```
```apache
admin pts/0 192.168.1.42 Fri Feb 27 09:12 still logged in
root pts/1 10.0.0.5 Thu Feb 26 22:03 - 22:45 (00:42)
reboot system boot 6.1.0-18 Thu Feb 26 21:58
```

On y voit : **qui, d'où, quand, et combien de temps**

### Options utiles de last
```ps
# Les 10 dernières connexions
last -n 10

# Connexions d'un utilisateur précis
Tast admin

# Avec les IPs complètes (pas de troncature)
last -a

# Depuis un fichier spécifique (rotation)
last -f /var/log/wtmp.1
```

_Réflexe d'audit : "qui est passé sur ce serveur récemment ?"_

### lastb : qui a échoué ? !
La commande lastb lit /var/log/btmp - la mémoire des connexions échouées
```yaml
sudo lastb
```
```apache
root ssh:notty 185.234.72.19 Fri Feb 27 03:12 - 03:12 (00:00)
root ssh:notty 185.234.72.19 Fri Feb 27 03:12 - 03:12 (00:00)
admin ssh:notty 45.133.1.88 Fri Feb 27 03:11 - 03:11 (00:00)
root ssh:notty 185.234.72.19 Fri Feb 27 03:11 - 03:11 (00:00)
```
Des dizaines d'échecs sur root en quelques minutes ? C'est un **brute- force** 🤖

### Lire un log, c'est déjà se défendre 👈
Un serveur attaqué laisse des traces :

* Rafales d'échecs sur root ou admin
* Mêmes IPs qui reviennent en boucle
* Tentatives à 3h du matin depuis des pays lointains

_Comprendre ces signaux évite de configurer fail2ban "dans le vide"_

### Autres logs utiles

| **Fichier** | **Contenu** |
| --- | --- |
| /var/log/auth.log | Détail des authentifications (Debian/Ubuntu) |
| /var/log/secure | Idem sur RHEL/CentOS |
| /var/log/wtmp | Connexions réussies (binaire, lu par last) |
| /var/log/btmp | Connexions échouées (binaire, lu par lastb) |

```bash
# Voir les échecs SSH en temps réel
tail -f /var/log/auth.log | grep "Failed password"
```

## Partie 2 : fail2ban 🚧
_Le garde-barrière automatique_

### C'est quoi fail2ban ?
fail2ban surveille les logs. Quand un comportement suspect dépasse un seuil, il ajoute automatiquement une **règle firewall** pour bannir l'**IP**.

![01-fail2ban](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C3%20(S%C3%A9curit%C3%A9%20syst%C3%A8mes%20%26%20r%C3%A9seaux)/images%20C3/images%20C307/C307_01-fail2ban.png)

_Simple, efficace, et ça stoppe la majorité des attaques automatisées_

### Comment ça fonctionne

```py
Logs (auth.log, sshd...)

│
┌───▼───────────┐
│ fail2ban      │
│ (surveillance)│
│               │
│ filter → jail │
└───┬───────────┘
│ seuil dépassé ?
┌───▼───────────┐
│ Action :      │
│ iptables/     │
│ nftables ban  │
└───────────────┘
```
### Les concepts clés
* **Filter** : une regex qui matche les lignes suspectes dans les logs
* ** Jail** : une prison qui combine un filter + des paramètres (seuil, durée, action)
* ** Action** : ce qui se passe quand le seuil est atteint (ban IP, mail, script ... )

_Un jail = "si ce pattern apparaît X fois en Y secondes, on bannit pendant Z secondes"_

### Installation
```yaml
# Debian / Ubuntu
sudo apt install fail2ban

# RHEL / Centos
sudo dnf install fail2ban

# Activer et démarrer le service
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```
### Les fichiers de configuration 📁

```apache
/etc/fail2ban/
├── fail2ban.conf ← config globale (ne pas modifier)
├── jail.conf ← jails par défaut (ne pas modifier)
├── jail.local ← VOS surcharges (à créer)
├── jail.d/ ← jails additionnelles
├── filter.d/ ← filtres (regex)
└── action.d/ ← actions (ban, mail...)
```

⚠️ Ne jamais modifier jail. conf directement - il sera écrasé à la mise à jour

_Toujours travailler dans jail. local ou jail.d/_

### Configuration de base : jail . local
```ruby
[DEFAULT]
# Durée du ban (10 minutes)
bantime = 10m

# Fenêtre d'observation
findtime = 10m

# Nombre d'échecs avant ban
maxretry = 5

# Action par défaut
banaction = iptables-multiport

# Ignorer certaines IPs (ne jamais se bannir soi-même !)
ignoreip = 127.0.0.1/8 :: 1 192.168.1.0/24
```

### Activer le jail SSH

```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 1h
findtime = 10m
```

_Traduction_ : **3 echecs SSH en 10 minutes → ban pendant 1 heure**

### Les filtres : comment fail2ban lit les logs
Le filtre SSH (/etc/fail2ban/filter.d/sshd.conf) contient des regex comme :

```ruby
^Failed password for .* from <HOST>
^Connection closed by authenticating user .* <HOST>
^Failed publickey for .* from <HOST>
```

`<HOST>` est remplacé par l'IP détectée — c'est elle qui sera bannie

### Ban progressif (récidivistes) 🔨
Pour les IPs qui reviennent après un ban :

```ini
[recidive]
enabled = true
logpath = /var/log/fail2ban.log
filter = recidive
bantime = 1w
findtime = 1d
maxretry = 3
```
_3 bans en 24h → banni pour 1 semaine !_

### Commandes de gestion
```ps
# Statut général
sudo fail2ban-client status

# Statut d'un jail spécifique
sudo fail2ban-client status sshd

# Bannir manuellement une IP
sudo fail2ban-client set sshd banip 185.234.72.19

# Débannir une IP
sudo fail2ban-client set sshd unbanip 185.234.72.19

# Voir les IPs bannies
sudo fail2ban-client get sshd banned
```

### Tester fail2ban ✅
Tester, c'est **prouver** que la protection fonctionne

#### 1. Vérifier que le jail est actif :
```yaml
sudo fail2ban-client status sshd
```

#### 2. Depuis une autre machine, provoquer des échecs :
```ruby
# Taper volontairement un mauvais mot de passe X fois
ssh fakeuser@serveur
```

#### 3. Vérifier le ban :
```ps
sudo fail2ban-client status sshd
# - Banned IP list: 10.0.0.99
```
#### 4. Confirmer la règle firewall :
```ps
sudo iptables -L -n | grep "10.0.0.99"
```

### Ce que fail2ban ne remplace pas ❌
* Ce n'est **pas un pare-feu complet**
* Il ne corrige pas une config SSH fragile
*·* Il ne protège pas d'une attaque **distribuée** (1000 IPs différentes)
* Il ne sert à rien si les logs ne sont pas correctement configurés

_fail2ban est une couche de défense, pas LA défense_

## Partie 3 : port-knocking 🚪
_Rendre un service invisible_

### Le principe
Le port-knocking consiste à **ouvrir un port uniquement après une séquence précise** de connexions sur d'autres ports

```rust
Attaquant           Admin
    │                 │
scan port 22       knock 7000
→ fermé ❌         knock 8000
scan port 22       knock 9000
→ fermé ❌        → port 22 s'ouvre ✅
                   ssh admin@serveur
```

Tant que la séquence n'est pas faite, le port **n'existe pas** pour le monde extérieur

### Analogie 🏰
C'est comme un **mot de passe secret pour entrer dans un speakeasy**

Vous devez frapper a la porte avec le bon rythme : toc ... toc-toc ... toc

_Si vous ne connaissez pas la séquence, vous ne savez même pas qu'il y a un bar derrière la porte_

### Installation de knockd
```apache
# Debian / Ubuntu
sudo apt install knockd

# RHEL / CentOS
sudo dnf install knock-server
```

### Configuration : /etc/knockd.conf

```ini
[options]
UseSyslog
[openSSH]
sequence = 7000,8000,9000
seq_timeout = 5
command = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
tcpflags = syn
[closeSSH]
sequence = 9000,8000,7000
seq_timeout = 5
command = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
tcpflags = syn
```
* **openSSH** : la séquence 7000 - 8000 - 9000 ouvre SSH pour l'IP du client
* **closeSSH** : la séquence inverse referme l'accès

### Les paramètres importants

| **Paramètre** | **Rôle** |
| --- | --- |
| sequence | Ports à frapper dans l'ordre |
| seq_timeout | Délai max entre les knocks (secondes) |
| command | Commande exécutée quand la séquence est validée |
| tcpflags | Type de paquet attendu (syn = connexion) |
| %IP% | Remplacé par l'IP du client qui frappe |

### Activer knockd

Sur Debian/Ubuntu, éditer /etc/default/knockd :
```bash
# Activer le démarrage
START KNOCKD=1

# Interface réseau à écouter
KNOCKD_OPTS="-i eth0"
```
```bash
sudo systemctl enable knockd
sudo systemctl start knockd
```

### Préparer le firewall
Avant d'activer knockd, il faut que SSH soit **ferme par défaut** dans iptables :
```ps
# S'assurer que SSH est bloqué par défaut
sudo iptables -A INPUT -p tcp -- dport 22 -j DROP
```

⚠️ **Gardez une session ouverte** ou un accès console avant de faire ça !

_knockd ajoutera/retirera la règle ACCEPT dynamiquement_

### Côté client : comment frapper
```bash
# Avec le client knock
knock Ierveur 7000 8000 9000

# Puis se connecter normalement
ssh admin@serveur

# Refermer après usage
knock serveur 9000 8000 7000
```
#### Sans le client knock, on peut utiliser nmap :

```bash
for port in 7000 8000 9000; do
nmap -Pn -- host-timeout 100 -p $port serveur
done
```
### Tester le port-knocking ✅

#### 1. Vérifier que SSH est fermé :

```ruby
nmap -p 22 serveur
# → 22/tcp filtered
```
#### 2. Frapper la séquence :

```ruby
knock serveur 7000 8000 9000
```

#### 3. Vérifier que SSH est ouvert :

```ruby
nmap -p 22 serveur
# → 22/tcp open
ssh admin@serveur
```

#### 4. Refermer :
```ruby
knock serveur 9000 8000 7000
```

### Avantages du port-knocking
* Élimine le **bruit des scans** automatiques
* Le port SSH n'apparaît pas dans les scans → **invisible**
* Simple à mettre en place
* Aucun logiciel côté serveur à exposer
* Compatible avec fail2ban et les clés SSH

### Inconvénients du port-knocking ⚠️
* La séquence transite en **clair** sur le réseau (interceptable)
* Si on oublie la séquence → **enfermé dehors**
* Le client knock n'est pas toujours disponible (réseau d'hôtel, mobile ... )
* Pas de protection si un attaquant **rejoue** la séquence capturée
* Ajoute de la **complexité opérationnelle**

_C'est une couche supplémentaire, pas un remplaçant d'une authentification forte !_

### Alternative : Single Packet Authorization (SPA) 🔐
fwknop est une alternative plus sécurisée au port-knocking classique :

* Un seul paquet **chiffré** au lieu d'une séquence en clair
* Protection contre le rejeu (nonce + timestamp)
* Même principe : le port s'ouvre uniquement pour l'IP autorisée

_Pour aller plus loin quand le port-knocking classique ne suffit pas_

### Le trio gagnant en production
Le scénario le plus sain pour un serveur exposé :

1.	**Clés SSH** + mot de passe désactivé (épisode précédent)
2.	**fail2ban** actif sur le jail SSH 
3.	**Port-knocking** en option pour masquer SSH
```ruby
Clés SSH → empêche le brute-force
fail2ban → bannit les IP abusives
port-knocking → masque le service
logs → permet de vérifier que tout tient
```

_Les trois se complètent, aucun ne se remplace_

### Ce qu'il faut retenir 🧠
* last / lastb - comprendre **qui** se connecte et **qui** échoue
* fail2ban - bloquer les abus automatiquement (filter + jail + action)
* port-knocking - masquer un service pour réduire la surface d'attaque
* Les trois ensemble - des **réflexes de durcissement** simples et efficaces

## Pour plus d'information 📚  
Voir 👉 [Démonstration de last, fail2ban et knockd ](https://github.com/O-clock-Aldebaran/SC3-E07-demo-last-lastb-fail2ban-crowdsec)

Voir aussi 👉 [Protéger un serveur Linux avec CrowdSec : le guide pour bien débuter](https://www.it-connect.fr/comment-proteger-son-serveur-linux-des-attaques-avec-crowdsec/)

👉 le [Challenge C307](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/Challenge_C307.md)