# 🏆 Challenge C307: Sécurisation d’un serveur Linux contre les attaques par force brute
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous êtes administrateur système dans une PME.
>
>Un serveur Linux (Debian/Ubuntu) est exposé sur Internet et héberge un accès SSH. Les logs d'authentification révèlent des dizaines de tentatives de connexion échouées chaque nuit depuis des adresses IP inconnues.
>
>Le responsable sécurité vous demande de mettre en place une protection automatisée contre ces attaques et, en bonus, de masquer complètement le port SSH aux scanners extérieurs.

---

👉 [Énoncé complet du challenge](https://github.com/O-clock-Aldebaran/SC03E07-challenge-fail2ban)

Voir 👉 [Cours C307](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C3%20(S%C3%A9curit%C3%A9%20syst%C3%A8mes%20%26%20r%C3%A9seaux)/C307_PAM%20%26%20fail2ban.md)

### 📚 Ressources d'intérêt
* [Démonstration de last, fail2ban et knockd ](https://github.com/O-clock-Aldebaran/SC3-E07-demo-last-lastb-fail2ban-crowdsec)

---

## 🖥️ Environnement technique

* 1 machine virtuelle sous Debian ou Ubuntu (installation minimale)
* Accès console ou SSH root
* SSH déjà installé et fonctionnel (SSH de base suffit, il doit autorisé les connexions par mot de passe pour simuler les attaques)
* Une machine cliente pour effectuer les tests

| **Machine** | **Accès console** | **IP address** |
| --- | --- | --- |
| CT Serveur Debian 13.1 | SSH root | `10.0.0.71` |
| CT Cliente Ubuntu | root | `10.0.0.80` |


---

## 📌 Objectifs techniques

### 1️⃣ Audit préalable : lire les logs de connexion

### 🚧 En construction 🚧 

Avant de configurer une protection, comprendre ce qui se passe sur le serveur.

* Afficher les **dernières connexions réussies** sur le serveur
* Afficher les **dernières tentatives échouées**
* Surveiller les logs d'authentification en temps réel

Pour simuler une attaque, vous pouvez effectuer plusieurs tentatives de connexion SSH avec un mot de passe incorrect depuis la machine cliente.

```bash
# Dernières connexions réussies
last -n 10

# Tentatives de connexion échouées (nécessite root)
sudo lastb -n 10

# Surveiller auth.log en temps réel
sudo tail -f /var/log/auth.log | grep "Failed password"
```

> **Question de réflexion :** Y a-t-il des tentatives répétées depuis une même IP ? Sur quel compte ?

---

### 2️⃣ Installation et configuration de fail2ban

#### Installation

```bash
sudo apt update
sudo apt install -y fail2ban

# Activer et démarrer le service
sudo systemctl enable fail2ban
sudo systemctl start fail2ban

# Vérifier que le service est actif
sudo systemctl status fail2ban
```

#### Configuration

Créer le fichier de configuration local (ne jamais modifier `jail.conf` directement) :

```bash
sudo nano /etc/fail2ban/jail.local
```

Contenu minimal à mettre en place :

```ini
[DEFAULT]
# Durée du ban
bantime  = 1h

# Fenêtre d'observation
findtime = 10m

# Nombre d'échecs avant ban
maxretry = 5

# Ne jamais se bannir soi-même
ignoreip = 127.0.0.1/8 ::1

[sshd]
enabled  = true
port     = ssh
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 3
bantime  = 1h
findtime = 10m
```

Puis redémarrer fail2ban pour appliquer la configuration :

```bash
sudo systemctl restart fail2ban
```

---

### 3️⃣ Vérification et test de fail2ban

#### Vérifier l'état du jail SSH

```bash
# Statut général
sudo fail2ban-client status

# Statut du jail SSH spécifiquement
sudo fail2ban-client status sshd
```

#### Tester le bannissement

Depuis la **machine cliente**, effectuer plusieurs tentatives de connexion avec un mauvais mot de passe :

```bash
# Répéter cette commande jusqu'à atteindre maxretry
ssh utilisateur_inexistant@IP_SERVEUR
```

Puis, **côté serveur**, vérifier que l'IP est bien bannie :

```bash
# Vérifier les IP bannies
sudo fail2ban-client status sshd

# Confirmer la règle firewall générée
sudo iptables -L -n -v | grep "fail2ban"
```

#### Débannir une IP si nécessaire

```bash
sudo fail2ban-client set sshd unbanip IP_A_DEBANNIR
```

---

### 4️⃣ Ban progressif pour les récidivistes (optionnel mais recommandé)

Ajouter dans `jail.local` un jail qui détecte les IPs déjà bannies et prolonge leur exclusion :

```ini
[recidive]
enabled  = true
logpath  = /var/log/fail2ban.log
filter   = recidive
bantime  = 1w
findtime = 1d
maxretry = 3
```

> **Traduction :** 3 bans en 24h → banni pour 1 semaine.

---

## 🎓 Bonus : mise en place du port-knocking avec knockd

Le port-knocking masque complètement SSH aux scanners : tant que la bonne séquence de ports n'est pas envoyée, le port 22 n'est tout simplement **pas visible**.

### Installation

```bash
sudo apt install -y knockd
```

### Fermer SSH par défaut dans iptables

⚠️ **Gardez une session ouverte ou un accès console avant d'exécuter ces commandes !**

```bash
# Réinitialiser les règles iptables
sudo iptables -F
sudo iptables -X
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Autoriser le loopback et les connexions établies
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# SSH fermé par défaut — knockd ouvrira dynamiquement pour l'IP autorisée
sudo iptables -A INPUT -p tcp --dport 22 -j DROP
```

### Configuration de knockd

```bash
sudo nano /etc/knockd.conf
```

```ini
[options]
    UseSyslog

[openSSH]
    sequence    = 7000,8000,9000
    seq_timeout = 5
    command     = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
    tcpflags    = syn

[closeSSH]
    sequence    = 9000,8000,7000
    seq_timeout = 5
    command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
    tcpflags    = syn
```

### Activer knockd

```bash
# Modifier la configuration de démarrage
sudo nano /etc/default/knockd
```

S'assurer que les lignes suivantes sont présentes :

```text
START_KNOCKD=1
KNOCKD_OPTS="-i eth0"
```

> Remplacer `eth0` par l'interface réseau réelle du serveur (`ip a` pour la trouver).

```bash
sudo systemctl enable knockd
sudo systemctl start knockd
```

### Test du port-knocking

**Côté client — vérifier que SSH est invisible :**

```bash
nmap -p 22 IP_SERVEUR
# Résultat attendu : 22/tcp  filtered
```

**Côté client — envoyer la séquence d'ouverture :**

```bash
# Avec le client knock
knock IP_SERVEUR 7000 8000 9000

# Alternative avec nmap si knock n'est pas disponible
for port in 7000 8000 9000; do
    nmap -Pn --max-retries=0 --host-timeout=1000ms -p $port IP_SERVEUR
done
```

**Côté client — vérifier que SSH est maintenant accessible :**

```bash
nmap -p 22 IP_SERVEUR
# Résultat attendu : 22/tcp  open

ssh utilisateur@IP_SERVEUR
```

**Côté serveur — vérifier les logs de knockd :**

```bash
sudo tail -f /var/log/syslog | grep knockd
```

**Côté client — refermer SSH après usage :**

```bash
knock IP_SERVEUR 9000 8000 7000
```

---

## ✅ Critères de validation

| Étape | Critère |
|-------|---------|
| Audit logs | Savoir lire `last`, `lastb` et `auth.log` |
| fail2ban installé | Service actif et activé au démarrage |
| Jail SSH configuré | `jail.local` avec les bons paramètres |
| Test de bannissement | Une IP cliente est bannie après dépassement de `maxretry` |
| Vérification iptables | La règle de ban est visible dans `iptables -L` |
| *Bonus* — port-knocking | Port 22 filtered avant knock, open après la séquence |
| *Bonus* — fermeture | Port 22 redevient filtered après la séquence inverse |

---

## 🔎 Points de vigilance

* Ne jamais se bannir soi-même : toujours renseigner `ignoreip` avec votre IP ou votre réseau
* Toujours garder une **session console** ouverte avant de bloquer SSH au niveau firewall
* Tester la configuration dans une session séparée **avant** de fermer la session active
* `knockd` nécessite un accès **console de secours** en cas d'oubli de la séquence