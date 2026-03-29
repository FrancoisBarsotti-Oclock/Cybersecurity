# 🏆 Challenge B301 (CP7): Supervision réseau.
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous venez d'être recruté(e) en tant qu'administrateur système junior chez TechSecure, une PME spécialisée dans l'hébergement d'applications web pour ses clients.
>
>Votre responsable, inquiet suite à plusieurs incidents non détectés la semaine dernière (serveur web inaccessible pendant 2h, saturation disque sur un serveur de base de données), vous confie une mission critique : **mettre en place les premières briques d'une infrastructure de supervision**.
>
>L'objectif est double :
>
>* Pouvoir interroger les équipements réseau via SNMP pour récupérer des informations vitales
>* Déployer un outil de supervision centralisé capable de surveiller l'ensemble du parc informatique
>

👉 [Énoncé complet du challenge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Supervision/EnoncEs%20de%20challenges%20B3/Pour%20B301_Supervision%20r%C3%A9seau.md)

Voir 👉 [Cours B301](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B3%20(Supervision)/B301_Supervision%20%26%20Monitoring.md)

## Étape 1: Architecture

Dans un environnement virtuel Proxmox avec :

| **Élement** | **OS** | **IP** | **Rôle** |
| :--: | :--: | :--: | :--: |
| **Zabbix** | Debian 13.1.2 | `10.0.0.100/16` | Serveur Zabbix |
| **Web-server-01** | Debian | `10.0.0.2416` | Serveur client |

## Étape 2: sudo & préparation pour Zabbix
 
 Important pour ne pas avoir à administrer la VM en tant que `root`

 ```nginx
adduser francois
apt update
apt install sudo -y
usermod -aG sudo <nom_utilisateur>
su - <nom_utilisateur>
```

Mise à jour de la VM pour préparer l'environnement Zabbix

 ```nginx
# maj de la Debian
sudo apt update
sudo apt upgrade -y

# Installation des prérequis de base
sudo apt install wget curl gnupg2 -y

# Vérification de la RAM
free -h
 ```

## Étape 3: Installation de Zabbix Server

Pour l'installation et configuration du serveur Zabbix avec sa base de données, on se réfère à la [documentation officielle](https://www.zabbix.com/fr/integrations) et chercher la configuration souhaitée selon plateforme: `Get Zabbix > Packages Zabbix` → J’ai favorisé Zabbix 7.4 car la version 8.0 est toujours en beta.

![01-Choix plateforme Zabbix](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_01-Choix%20plateforme%20Zabbix.png)

Lors de l'installation, ne pas oublier que les commandes MySQL se finissent avec un `;` .

Pour s'assurer que le base de données du serveur est up et active, il faut installer MariaDB

```nginx
# Installation de MariaDB
sudo apt install mariadb-server -y

# Pour configurer MariaDB
sudo mariadb-secure-installation
```
Une fois tout installé sur la partie MySQL, on peut vérifier ce qu'il y a dans la base de donnée créé par un `show databases` (toujours dans MariaDB)

![02-Vérification database sur MariaDB](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_02-V%C3%A9rification%20database%20sur%20MariaDB.png)

Ne pas oublier de modifier la configuration Zabbix avec un `sudo nano /etc/zabbix/zabbix_server.conf` pour reécrire la ligne `DBPassword=MotDePasseSecurise` en le décommentant et en ajoutant le mot de passe que l'on avait mis pendant la configuration.

Une fois le serveur Zabbix rebooté, on pourra s'y connecter par l'URL par défaut: http://ip_host/zabbix

## Étape 4: HTTP → HTTPS

À ce stade la connexion à l'interface web de Zabbix se fait uniquement en HTTP, alors il faudra lui faire un HTTPS autosigné et rediriger les requêtes HTTP vers le HTTPS

```nginx
# 🔐 1. Activer SSL sur Apache
sudo a2enmod ssl
sudo systemctl restart apache2

# 🔑 2. Générer un certificat autosigné
sudo openssl req -x509 -nodes -days 365 \
-newkey rsa:2048 \
-keyout /etc/ssl/private/zabbix-selfsigned.key \
-out /etc/ssl/certs/zabbix-selfsigned.crt

# On va remplacer Common Name (CN) par l'ip du serveur
# ⚙️ 3. Configurer le VirtualHost HTTPS
sudo nano /etc/apache2/sites-available/zabbix-ssl.conf

# Le fichier devra avoir ce contenu:
<VirtualHost *:443>
    ServerName 10.0.0.100

    DocumentRoot /usr/share/zabbix

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/zabbix-selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/zabbix-selfsigned.key

    <Directory /usr/share/zabbix>
        Options FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/zabbix_ssl_error.log
    CustomLog ${APACHE_LOG_DIR}/zabbix_ssl_access.log combined
</VirtualHost>

# Activation
sudo a2ensite zabbix-ssl.conf
sudo systemctl reload apache2

# 🔁 4. Redirection HTTP → HTTPS dans default.con ou bien zabbix.conf
sudo nano /etc/apache2/sites-available/000-default.conf

# Ajouter ce contenu:
<VirtualHost *:80>
    ServerName 10.0.0.100
    Redirect permanent / https://10.0.0.100/
</VirtualHost>

# Réinicialisation d'Apache
sudo systemctl reload apache2
```

![03-HTTPS autosigné pour Zabbix](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_03-HTTPS%20autosign%C3%A9%20pour%20Zabbix.png)

## Étape 5: Configuration du frontend Zabbix

On se connecte sur l'interface web de Zabbix (maintenant `https://ip_host/zabbix`) et on suit l'assistant de configuration. Bien retenir que le port par défaut MySQL est `3306` (si l'on ne le change pas lors de la conf).

Pour ressoudre le problème de langue pré-requise (en_US), il faudra dépackager et rebbot Apache (bien rester en anglais car les traductions ne sont pas au point)

```nginx
sudo dpkg-reconfigure locales
```

![04-dpkg locales](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_04-dpkg%20locales.png)

```nginx
sudo systemctl restart apache2
```

![05-Conf Zabbix terminée](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_05-Conf%20Zabbix%20termin%C3%A9e.png)

### Première connexion

* Utilisateur: `Admin`
* Mot de passe:  `zabbix`

### Modification du mot de passe par défaut

 *User settings → Profile*

 ![06-Modif mdp](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B301%20(CP7)/B301_CP7_06-Modif%20mdp.png)


### 🚧 En construction 🚧

---

### 📚 Ressources & liens utiles:

* [documentation officielle Zabbix](https://www.zabbix.com/fr/integrations) 
