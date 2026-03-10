# 🏆 Challenge C402: Docker Compose
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous êtes administrateur système dans une PME. Votre responsable vous demande de déployer GLPI, l'outil de gestion de parc informatique open-source, de façon reproductible et conteneurisée.
>
>Plutôt que d'installer GLPI directement sur un serveur, vous allez utiliser Docker Compose pour orchestrer plusieurs services : l'application GLPI elle-même, sa base de données MariaDB, et en bonus un outil de gestion de BDD via une interface web.
>
>💡 Pourquoi Docker Compose ?
>
>* Tout l'environnement est décrit dans un seul fichier : `docker-compose.yml`
>* Une seule commande pour tout démarrer, tout arrêter, tout reconstruire
>* L'environnement est identique sur tous les postes de l'équipe

👉 [Énoncé complet du challenge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/EnoncEs%20de%20challenges/Pour%20C402.md) 👈

Voir 👉 [Cours C402](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C4%20(Conteneurs%20%26%20orchestration)/C402_Multiconteneurs.md) 👈

---

## 🛠️ Environnement technique

| **Élément** | Docker |
| :---: | :---: | 
| **OS** | Debian 13.3.0 |
| **IP statique** | 10.0.0.8 | 
| **System** | Qemu Agent |
| **Disque** | 32 GiB | 
| **CPU** | 2 Sockets + 2 Cores (type host, si en local) | 
| **RAM** | 2048 MiB | 

## Étape 1 - Mise en place du projet

```apache
# Création du dossier du projet
mkdir glpi-docker && cd glpi-docker

# Préparation de la structure du projet
mkdir -p glpi mariadb phpmyadmin
touch docker-compose.yml

# Vérification (avec tree)
sudo apt install tree -y
tree
```

![01-VérificationTree](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C4/images%20C4/images%20C402/Challenge%20402_01-V%C3%A9rificationTree.png)

### Services à prévoir dans le `docker-compose.yml`

**Obligatoires**

* **glpi** : l’application web GLPI
* **mariadb** : la base de données de GLPI

**Bonus**

* **phpmyadmin** : interface web pour administrer MariaDB

### Volumes / persistance à prévoir

Pour que ce soit reproductible **sans perdre les données** :

* un volume pour **MariaDB**

* un volume pour les **fichiers GLPI** si besoin

* éventuellement un réseau Docker dédié

### Projet prévu (selon tree)

```
glpi-docker/
├── docker-compose.yml
├── glpi/
├── mariadb/
└── phpmyadmin/
```
## Étape 2 — Service MariaDB

```Apache
# Édition du fichier, dans le dossier glpi-docker
nano docker-compose.yml

# Ajout du service MariaDB
services:
  db:
    image: mariadb:11
    container_name: glpi-mariadb
    restart: always

    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: glpidb
      MYSQL_USER: glpiuser
      MYSQL_PASSWORD: glpipassword

    volumes:
      - db_data:/var/lib/mysql

    ports:
      - "3306:3306"

volumes:
  db_data:
```
```apache
# Démarrage du conteneur dans le dossier glpi-docker
sudo docker compose up -d db

# Vérification que glpi-mariaDB soit UP
sudo docker ps

# Vérification des logs
sudo docker logs glpi-mariadb
```

![03-MariaDBOK]()

## Étape 3 — Service GLPI

```apache
sudo nano docker-compose.yml

# Nouvelle édition du fichier docker-compose pour ajouter le service glpi
services:

  db:
    image: mariadb:11
    container_name: glpi-mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: glpidb
      MYSQL_USER: glpiuser
      MYSQL_PASSWORD: glpipassword
    volumes:
      - db_data:/var/lib/mysql

  glpi:
    image: diouxx/glpi
    container_name: glpi
    restart: always

    depends_on:
      - db

    ports:
      - "8080:80"

    volumes:
      - glpi_data:/var/www/html

volumes:
  db_data:
  glpi_data:
```

```apache
# Démarrage de glpi
sudo docker compose up -d

# Vérification des conteneurs
sudo docker ps
```

### Vérification depuis la VM

![04-GlpiVM]()

### Vérification depuis mon navigateur

![05-GlpiNavigateur]()

## Étape 4 — Réseau & Communication

```apache
# Ajout (3 fois) de glpi-net ent que réseau privé Docker pour rattacher db et glpi

services:
  db:
    image: mariadb:11
    container_name: glpi-mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: glpidb
      MYSQL_USER: glpiuser
      MYSQL_PASSWORD: glpipassword
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - glpi-net

  glpi:
    image: diouxx/glpi
    container_name: glpi
    restart: always
    depends_on:
      - db
    ports:
      - "8080:80"
    volumes:
      - glpi_data:/var/www/html
    networks:
      - glpi-net

volumes:
  db_data:
  glpi_data:

networks:
  glpi-net:
    driver: bridge
```

```apache
# Application de modif
sudo docker compose down
sudo docker compose up -d

# Vérification du réseau
sudo docker network ls
sudo docker network inspect glpi-docker_glpi-net
```

![06-GlpiNet]()

### Création de phpMyAdmin

```apache
# Ajout du service phpmyadmin sur le port 8081

services:

  db:
    image: mariadb:11
    container_name: glpi-mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: glpidb
      MYSQL_USER: glpiuser
      MYSQL_PASSWORD: glpipassword
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - glpi-net

  glpi:
    image: diouxx/glpi
    container_name: glpi
    restart: always
    depends_on:
      - db
    ports:
      - "8080:80"
    volumes:
      - glpi_data:/var/www/html
    networks:
      - glpi-net

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: glpi-phpmyadmin
    restart: always
    depends_on:
      - db
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: rootpassword
    networks:
      - glpi-net

volumes:
  db_data:
  glpi_data:

networks:
  glpi-net:
    driver: bridge
```

```apache
# Rédemarrage des conteneurs
sudo docker compose down
sudo docker compose up -d
```

![07-phpMyAdmin]()

### Vérification côté navigateur

![08-NavigateurphpMyAdmin]()

