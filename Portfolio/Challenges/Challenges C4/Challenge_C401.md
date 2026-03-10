# 🏆 Challenge C401: Docker, les bases
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Ce premier exercice va vous guider pour installer Docker sur votre machine et vous permettre de vous familiariser avec les commandes usuelles et fonctionnalités de base de Docker.

👉 [Énoncé complet du challenge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/EnoncEs%20de%20challenges/Pour%20C401.md) 👈

Voir 👉 [Cours C401](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C4%20(Conteneurs%20%26%20orchestration)/C401_Conteneur%20%26%20Docker.md) 👈

---

## 🛠️ Environnement technique

| **Élément** | Docker |
| :---: | :---: | 
| **OS** | Debian 13.3.0 |
| **IP** | :---: | 
| **System** | Qemu Agent |
| **Disque** | 32 GiB | 
| **CPU** | 2 Sockets + 2 Cores (type host, si en local) | 
| **RAM** | 2048 MiB | 

Toujours commencer par :
```apache
su -

apt update && apt upgrade -y && apt install sudo

usermod -aG sudo <user>

reboot
```

Pour l'installation de docker sur Debian, suivre la [documentation officielle](https://docs.docker.com/engine/install/debian/)


# ⚠️ Debug 

Si pendant l'installation des prérequis, on trouve ce message d'erreur 
```rust
Erreur : Impossible de recuperer http://deb.debian. org/debian/pool/main/c/cyrus-sas12/libsas12-modules-db_2.1.28%2bdfsg1 -9_amd64.deb Erreur temporaire de resolution de « deb.debian.org >
```
Cela veut dire qu'il faut reconfigurer le DNS de la VM pour qu'elle puisse résoudre `deb.debian.org` (normalement à ce stade elle ne ping même pas le site)

### Solution

1. **Vérification du fichier /interfaces**
```ps
cat /etc/network/interfaces
```

Lequel doit contenir:

```ps
auto ens18
iface ens18 inet static
    address 10.0.0.8
    netmask 255.255.0.0
    gateway 10.0.0.1
    dns-nameservers 8.8.8.8 1.1.1.1
```

2. **Vérifier le résolveur DNS**

```ps
cat /etc/resolv.conf
```
où il faudra y voir:

```
nameserver 8.8.8.8
nameserver 1.1.1.1
```

S'il n'existe pas, il faudra le créer et tester:

```ps
# Pour le créer
sudo nano /etc/resolv.conf

# il faudra y mettre les DNS
nameserver 8.8.8.8
nameserver 1.1.1.1

# Tester et restart
ping deb.debian.org
sudo apt update
sudo systemctl restart networking
```

## Pré-requis jusqu'installation de Docker

![01-Hello from Docker]()

## Étapes 1 & 2: Premier conteneur

Récupération du projet de Baptiste Delphin:

```apache
# En direct
sudo docker run -p 8888:80 bdelphin/hello-docker

# En tâche de fond
sudo docker run -d -p 8888:80 bdelphin/hello-docker

# Vérification sur le navigateur
http://localhost:8888

# Arrêt du conteneur
sudo docker stop <nom_conteneur> 
# ou 
sudo docker stop <id_conteneur>

# Consulter l'état du conteneur
sudo docker ps
```
![02-Etapes 1&2]()



