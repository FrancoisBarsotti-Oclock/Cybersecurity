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

## Étape 4: MariaDB & MySQL

```nginx
# Installation de MariaDB
sudo apt install mariadb-server

# Pour configurer MariaDB
sudo mysql_secure_installation

# Connexion au serveur de base de données (MariaDB)
mysql -u root -p

# Création d'un utilisateur (autre que root)
CREATE USER 'dbuser'@'localhost' IDENTIFIED BY 'motdepasse';
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



---




### 📚 Ressources & liens utiles:

* 🎥​ [Tuto: Installer et configurer GLPI sous Debian 12](https://www.youtube.com/watch?v=4p-Zuuyr_Ts)
* 📘 [Atelier S505](https://github.com/O-clock-Aldebaran/SA5-Atelier-LAMP)

