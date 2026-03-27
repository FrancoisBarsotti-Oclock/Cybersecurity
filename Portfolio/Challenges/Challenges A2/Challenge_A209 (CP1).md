# 🏆 Challenge A209 (CP1): Mise en place d’un processus ITIL avec GLPI pour gérer les incidents et respecter les SLA.
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

## Étape 1: VM Debian

| **Élement** | **OS** | **IP** | **Rôle** |
| :--: | :--: | :--: | :--: |
| **LAMP GLPI** | Debian 12.0.0 | `10.0.0.21` | Serveur GLPI |
| Windows 10 | Windows 10 | `10.0.0.22` | machine à surveiller |

## Étape 2: sudo

Important pour ne pas avoir à administrer la VM en tant que `root`
```nginx
su -
apt update
apt install sudo
usermod -aG sudo <nom_utilisateur>
```

## Étape 3: Apache

```nginx
sudo apt install apache2
systemctl status apache2

# S'il n'est pas actif
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl restart apache2

# Pour vérifier que le site web est opérationnel
sudo apt-get install curl
```

Importnat d'activer HTTPS autosigné sur Apache, pour pouvoir le tester depuis une autre machine 

```nginx
# Activation SSL + site par défaut
sudo a2enmod ssl
sudo a2ensite default-ssl
sudo systemctl restart apache2

# Générer un certificat autosigné
sudo openssl req -x509 -nodes -days 365 \
-newkey rsa:2048 \
-keyout /etc/ssl/private/apache-selfsigned.key \
-out /etc/ssl/certs/apache-selfsigned.crt

# Vérification de config Apache
sudo nano /etc/apache2/sites-available/default-ssl.conf

#réinilisation et connexion
sudo systemctl restart apache2
https://ip_du_serveur
```

![01-Apache HTTPS autosigné](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_01-01-Apache%20HTTPS%20autosign%C3%A9.png)

## Étape 4: MariaDB & MySQL

```nginx
# Installation de MariaDB
sudo apt install mariadb-server

# Pour configurer MariaDB
sudo mysql_secure_installation

# Connexion au serveur de base de données (MariaDB)
mysql -u root -p

# Création d'un utilisateur (autre que root)
CREATE USER 'dbuser'@'localhost' IDENTIFIED BY '<un_motdepasse_fort>';
GRANT ALL PRIVILEGES ON *.* TO 'dbuser'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
exit
```

Vérifier que le service MariaDB sera bien lancé automatiquement au démarrage avec la commande `systemctl status mariadb` (on doit voir enabled).

## Étape 5: PHP

La plupart des applications web sont développées avec le langage PHP : c'est le cas de GLPI, il faut donc qu'on installe l'interpréteur PHP ! Pour cela, lancez les commandes suivantes :

```nginx
sudo apt install php libapache2-mod-php

# Ensuite plusieurs modules de PHP
sudo apt install php-{curl,gd,intl,memcache,xml,zip,mbstring,json,mysql,bz2,ldap}

# Redémarrer apache 
sudo systemctl restart apache2
```

Pour vérifier que PHP est opérationnel, on va créer un fichier très basique en PHP. Lancez la commande suivante :

```nginx
echo "<?php phpinfo(); ?>" | sudo tee -a /var/www/html/info.php
```

![02-PHP fonctionnel](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_02-PHP%20fonctionnel.png)

## Étape 6: GLPI

Téléchargement de GLPI

```nginx
cd ~
wget https://github.com/glpi-project/glpi/releases/download/10.0.17/glpi-10.0.17.tgz

# Vérification de sa présence 
ls

# Décompression du .tgz dans le dossier /var/www/html
sudo tar -xvf glpi-10.0.17.tgz -C /var/www/html

# Vérification de la décompression
ls /var/www/html
```

![03-GLPI HTTPS autosigné fonctionnel](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_03-GLPI%20HTTPS%20autosign%C3%A9%20fonctionnel.png)

Pour mieux protéger la VM Debian, j'ai lancé des règles iptables

```nginx
# Installation de la persistance
# Installation du paquet pour sauvegarder les règles iptables au redémarrage
sudo apt update
sudo apt install iptables-persistent

# Nettoyage des règles existantes
# Vider toutes les chaînes de la table filter (INPUT, OUTPUT, FORWARD)
sudo iptables -F

# Supprimer les chaînes personnalisées
sudo iptables -X

# Vider les chaînes de la table nat
sudo iptables -t nat -F
sudo iptables -t nat -X

# Politique par défaut
# Bloquer tout le trafic entrant par défaut
sudo iptables -P INPUT DROP

# Bloquer le transit de paquets entre interfaces
sudo iptables -P FORWARD DROP

# Autoriser tout le trafic sortant
sudo iptables -P OUTPUT ACCEPT

# Autorisation du loopback
# Autoriser les communications internes (127.0.0.1)
sudo iptables -A INPUT -i lo -j ACCEPT

# Autorisation des connexions déjà établies
# Permettre les réponses aux connexions déjà sudo initiées (ex: réponses HTTP, DNS...)
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Autorisation des connexions SSH & HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 22 -s 10.42.0.0/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -s 10.42.0.0/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -s 10.42.0.0/24 -j ACCEPT

# Drop des paquets invalides
# Rejeter les paquets dans un état de connexion invalide (protection supplémentaire)
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP

#Vérification et sauvegarde
# Afficher les règles actives avec statistiques
sudo iptables -L -n -v

# Sauvegarder les règles pour qu'elles persistent après redémarrage
sudo netfilter-persistent save

```

![04-Vérification politiques iptables](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_04-V%C3%A9rification%20politiques%20iptables.png)

Pendant l'installation on arrive à l'érreur classique due à un problème de permissions dans le dossier `/var/www/html`

![05-Problème de permission glpi](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_05-Probl%C3%A8me%20de%20permission%20glpi.png)

Pour la corriger, il faudra "refresh" après avoir fait deux choses:
1. Changer le propriétaire des fichiers → <nom_utilisateur> (dans ce cas c'est `francois`) et le groupe utilisé par Apache → www-data en l'appliquant à tout le dossier récursivement (`-R`), pour que l'utilisateur puisse contrôler les fichiers et le serveur web puisse aussi y accéder via le groupe.
2. Définir les permissions de récursivement (sur tous les dossiers, `-R`) pour que l'utilisateur et `www-data` aient accès complet tandis que les autres n'y aient aucun accès:
    * **7 (user)** = lecture + écriture + exécution
    * **7 (group)** = lecture + écriture + exécution
    * **0 (others)** = aucun accès

```nginx
sudo chown -R <nom_utilisateur>:www-data /var/www/html
sudo chmod 770 -R /var/www/html
```

En continuant l'installation, on rencontre des Erreurs de sécurité à corriger plus loin


![06-Erreurs de sécurité à corriger plus loin](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_06-Erreurs%20de%20s%C3%A9curit%C3%A9%20%C3%A0%20corriger%20plus%20loin.png)

Quand on nous demande la configuration de la connexion à la base de données, on va s'appuyer sur l'utilisateur qu'on a créé en Étape 4 (MariaDB & MySQL); dans ce cas on saisit `localhost` comme adresse du serveur, `dbuser` comme nom d'utilisateur et le mot de passe fort que l'on avait choisi, en suivant les récommendations de l'ANSSI (dans mon cas, je le sauvegarde avec un gestionnaire de mot de passe):

![07-Authentification GLPI](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_07-Authentification%20GLPI.png)

Pour la suite, on peut créer une nouvelle base de données nommée `glpi` :

![08-base de données glpi](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_08-base%20de%20donn%C3%A9es%20glpi.png)

💡 Après avoir cliqué sur le bouton `Continuer`, l'initialisation de la base de données peut prendre plusieurs minutes. 

> [!WARNING]
> Ne pas recharger la page pendant l'initialisation de la base de données, pour éviter dévoir tout recommencer depuis le début de l'installation de GLPI. Il faudra attendre jusqu'à ce que GLPI nous dise que la base de données est bien créée et initialisée (avec un OK)

![09-base de données créée et initialisée](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_09-base%20de%20donn%C3%A9es%20cr%C3%A9%C3%A9e%20et%20initialis%C3%A9e.png)

Attention :  la première connexion à la base de données est par défaut avec `glpi` comme utilisateur et mot de passe:

![10-Première connexion à base interne glpi](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_10-Premi%C3%A8re%20connexion%20%C3%A0%20base%20interne%20glpi.png)

On aura réussi notre première connexion avec les récommandations initiales de sécurité

![11-GLPI opérationnel](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_11-GLPI%20op%C3%A9rationnel.png)

Alors, vite l'action à faire en urgence, suite à la première connexion, sera de remplacer le mot de passe (par défaut), par un mot de passe qui respecte les recommandations de l'ANSSI. 

![12-Modif du mdp du super utilisateur](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_12-Modif%20du%20mdp%20du%20super%20utilisateur.png)

Non seulement pour le super utilisateur mais aussi pour tous les autres utilisateurs qu'y soient déjà créés par défaut avec l'installation (à chercher sur `Administration → Utilisateurs`)

![13-Modif du mdp par défaut des utilisateurs](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_13-Modif%20du%20mdp%20par%20d%C3%A9faut%20des%20utilisateurs.png)

## Étape 7: sécurité

Après avoir modifié tous les mdp par défaut, on doit: 

### 1. supprimer le fichier `install/install.php` 

```nginx
# Rentrer dans le dossier en question 
cd /var/www/html/glpi

# le supprimer 
sudo rm install/install.php

# Vérifier
ls install/
```

![14-Suppression du dossier glpi](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_14-Suppression%20du%20dossier%20glpi.png)

### 2. Faire pointer Apache sur le dossier `public/` uniquement 
Ceci est pour que certains dossiers restent privés. cela corrige l'erreur: 
>"La configuration du dossier racine du serveur web n'est pas sécurisée car elle permet l'accès à des fichiers non publics")

```nginx
# Modifier la config Apache pour http
sudo nano /etc/apache2/sites-available/000-default.conf

# Modifier la config Apache pour http
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

Remplacer `DocumentRoot /var/www/html` par `DocumentRoot /var/www/html/glpi/public` et rajouter trois lignes pour autoriser l'accès au bon dossier, comme il suit:

```nginx
DocumentRoot /var/www/html/glpi/public

<Directory /var/www/html/glpi/public>
    Require all granted
    AllowOverride All

    RewriteEngine On

    RewriteCond %{HTTP:Authorization} ^(.+)$
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php [QSA,L]
</Directory>
```
Si l'on lance une vérification à ce stade avec `sudo apache2ctl configtest` On verra qu'Apache ne connaît pas notre domaine et nous envite à rajouter un nom au serveur global, selon l'erreur: 

>AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
>Syntax OK

il faudra l'appeler `localhost`:

```
sudo nano /etc/apache2/apache2.conf

# On rajoute le "ServerName"
```

Ensuite active le module de réécriture et redémarre Apache :

```nginx
sudo a2enmod rewrite
sudo apache2ctl configtest
sudo systemctl restart apache2
```

Jusqu'à présent nous avons toujours deux vhosts (le HTTP *80 et le HTTPS autosigné *443). Pour éviter un mélange de configs inutile on peut désactiver le HTTP *80:

```nginx
# désactivation dossier du HTTP
sudo a2dissite 000-default.conf

# Restart de service
sudo systemctl reload apache2

# Vérification pour y voir uniquement *:443
apache2ctl -S
```

![15-Désactivation fichier HTTP](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_15-D%C3%A9sactivation%20fichier%20HTTP.png)

💡 à savoir que, en production, on peut aussi juste rediriger HTTP vers HTTPS au lieu de désactiver:

```nginx
<VirtualHost *:80>
    Redirect permanent / https://10.0.0.21/
</VirtualHost>
```

### 3. Définir directive PHP `session.cookie_secure` & `session.cookie_httponly` à "ON"

Il faut l’activer dans la configuration PHP, puisque GLPI recommande `session.cookie_secure=on` quand l’application est accessible uniquement en HTTPS. PHP précise aussi que ce réglage fait que le cookie de session n’est envoyé qu’en connexion sécurisée.
→ Cela nous aide à résoudre l'erreur:
>"La directive PHP "session.cookie_secure" devrait être définie à "on" quand GLPI est accessible via le protocole HTTPS."

Pareil pour la directive "session.cookie_httponly". Tous les deux empêcherot JavaScript d'accéder au cookie de session et protegéront contre XSS (vol de session).

```nginx
# Pour préparer la version PHP
php -v

# Pour éditer le bon phip.ini (version 8.2)
sudo nano /etc/php/8.2/apache2/php.ini
```

On va chercher la ligne "session.cookie_secure =" pour y ajouter:

```INI
session.cookie_secure = On
session.cookie_httponly = On
session.cookie_samesite = Lax
```

Puis, on redémarre Apache et on vérifie les valeurs actives :

```nginx
sudo systemctl restart apache2
grep -E "session.cookie_secure|session.cookie_httponly" /etc/php/8.2/apache2/php.ini
```

il devra retourner: session.cookie_secure => Off => Off

![16-Activation cookie secure sur PHP](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_16-Activation%20cookie%20secure%20sur%20PHP.png)

### 4. Constater que toutes les erreurs soient résolues

Même si l'on constate que toutes les erreurs sont disparues du tableau de bord, rendez-vous sur la page `Configuration > Générale > Tous` pour voir que tout soit correct:

![17-Sécurité GLPI garantie](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_17-S%C3%A9curit%C3%A9%20GLPI%20garantie.png)

## Étape 8: Configuration GLPI

### 1. Ajout d'une autre VM à être supervisée

Pour surveiller la Windows 10 (10.0.0.22), il est nécessaire d'installer GLPI Agent for Windows

Quand il s'agit d'une installation silencieuse, il faut configurer le Server URL / Target, en modifiant le fichier `C:\Program Files\GLPI-Agent\etc\agent.cfg` pour y rajouter: `server = https://10.0.0.21/`, puis redémarrer le service avec un `Restart-Service glpi-agent` sur PowerShell.

Ainsi on pourra voir la machine rajoutée automatiquement dans le `GLPI > Parc > ordinateurs` 

![18-Machine dans Parc](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_18-Machine%20dans%20Parc.png)


Et on pourra la voir aussi répertoriée sur le tableau de bord

![19-Machine dans tableau de bord](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20A2/images%20A2/images%20A209%20CP1/A209_CP1_19-Machine%20dans%20tableau%20de%20bord.png)

---

### 📚 Ressources & liens utiles:

* 🎥​ [Tuto: Installer et configurer GLPI sous Debian 12](https://www.youtube.com/watch?v=4p-Zuuyr_Ts)
* 📘 [Atelier S505](https://github.com/O-clock-Aldebaran/SA5-Atelier-LAMP)
* [Site officiel GLPI](https://www.glpi-project.org//fr/)
* [Installation de GLPI-Agent sur Windows](https://www.it-connect.fr/tuto-installer-configurer-gpo-agent-glpi-windows/)

---

