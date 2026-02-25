# 🏆 Challenge C302: Radius
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous intervenez pour une PME qui veut centraliser l’authentification réseau. L’objectif est de ne plus gérer des comptes locaux sur chaque équipement et de s’appuyer sur l’Active Directory. Votre mission : mettre en place un serveur RADIUS et l’intégrer proprement à l’AD.
>
>Objectifs
>* Monter un serveur RADIUS opérationnel sur Proxmox.
>* Lier l’authentification à l’Active Directory.
>* Tester un scénario d’accès (succès / échec) avec un compte AD.

👉 [Énoncé complet du challenge](https://github.com/O-clock-Aldebaran/E02-SC3E02-radius-FrancoisBarsotti-Oclock) 👈

Voir 👉 Cours C302 👈

---

## 1. Préparation de l'environnement

| **Élement** | **OS** | **IP** | **Rôle** |
| :---: | :---: | :---: | :---: |
| **FreeRadius** | Ubuntu 24.04.4 | `10.0.0.68/16` | Serveur |
| **ActiveDirectory** | Win Srv 2025 | `10.0.0.69/16` | Contrôl de Domaine |
| **LDAP** | Ubuntu 24.04.4 | `10.0.0.70/16` | Serveur |
| **Réseau** | Vmb2 | `10.0.0.0/16` | Segment réseau dans proxmox |

* ### Côté Windows Server 2025


![01-WinSrvAD](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_01-WinSrvAD.png)

* ### Côté Ubuntu 🐧

Après installation d'Ubuntu, on teste la communication entre VMs: 

* `WinSrv-AD → Ubuntu-Radius`:

![02-PingWin2Ubuntu](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_02-PingWin2Ubuntu.png)

* `UbuntuLDAP → WinSrv&Radius-AD`

![03-PingLDAPToWin&Radius](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_03-ingLDAPToWin%26Radius.png)

Pas besoin de le faire depuis le serveur de Radius, puisqu'il a déjà donné réponse aux autres deux serveurs.

Alors, on commence à faire l'installation du serveur ldap, en suivant les commandes:

### Installation du serveur LDAP

`sudo apt update`

`sudo apt install slapd ldap-utils -y`

Il faut refaire la configuration du serveur LDAP

`sudo dpkg-reconfigure slapd`

    NON pour configurer le serveur maintenant (sinon on ne peut pas poursuivre).
    Nom de domaine DNS : ldap.lan
    Nom de l’organisation : FranBarso Inc.
    Mot de passe admin : 12345_je_rigole 🤭
    Base de données : généralement MDB (default).
    Supprimer la base existante : NON si vous voulez garder d’anciennes données.

![04-ConfigSlapd](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_04-ConfigSlapd.png)

Vérification que le serveur LDAP est bien en route (rappel, slapd c'est un service qui tourne en arrier plan sur le système):
`sudo systemctl status slapd`

`ldapsearch -x -LLL -H ldap:// -b dc=example,dc=com` → → `ldapsearch -x -LLL -H ldap:// -b dc=challenge,dc=C302`

![05-slapdStauts](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_05-slapdStauts.png)

Première commande pour vérifier que mon LDAP répond: 
`ldapsearch -x -LLL -H ldap:// -b dc=ldap,dc=lan`

Et on voit que le DN est bien trouvé sur mon serveur LDAP en locale 

![05.1-DNtrouvé]()

### Création d'une "OU" (nano base_users.ldif)

`nano base_users.ldif`
    
    dn: ou=users,dc=ldap,dc=lan
    objectClass: organizationalUnit
    ou: users

Commande qui permettra de rajouter l'ou qui vient d'être créé
`sudo ldapadd -x -D cn=admin,dc=example,dc=com -W -f base_users.ldif` → → `sudo ldapadd -x -D cn=admin,dc=ldap,dc=lan -W -f base_users.ldif`

![06-OUcréé]()

### Création d'un "USER" (nano newuser.ldif)

`nano newuser.ldif`

    dn: uid=gbarsotti,ou=users,dc=ldap,dc=lan
    objectClass: inetOrgPerson
    objectClass: posixAccount
    objectClass: shadowAccount
    uid: gbarsotti
    sn: Barsotti
    givenName: Gérard
    cn: Gérard Barsotti
    displayName: Gérard Barsotti
    uidNumber: 1001
    gidNumber: 1001
    userPassword: motdepasse
    loginShell: /bin/bash
    homeDirectory: /home/gbarsotti

De suite, il faut chiffrer le mdp qui apparait en clair avec la commande `slappasswd`

![07-newmdpHashé]()

puis le remplacer (dans le même fichier) par le mot de passe hashé 

![07.1-mdpHashéRenseigné]()

Commande qui permettra de rajouter l'uid (utilisateur local) qui vient d'être créé
`sudo ldapadd -x -D cn=admin,dc=example,dc=com -W -f newuser.ldif` → → `sudo ldapadd -x -D cn=admin,dc=ldap,dc=lan -W -f newuser.ldif`

![07.2-uidRajouté]()

### TEST LOCAL
`ldapsearch -x -LLL -b dc=example,dc=com uid=newuser` → → `ldapsearch -x -LLL -b dc=ldap,dc=lan uid=gbarsotti`

![07.3-Vérifuid]()

à partir de là, on peut continuer à créer autant d'utilisateurs nécessaires, car notre serveur LDAP est bien fonctionnel

## Installation de freeRADIUS

👉 [FreeRadius for Debian and Ubuntu](https://www.freeradius.org/documentation/freeradius-server/4.0.0/howto/installation/debian.html) 👈

👉 [Install FreeRADIUS on Ubuntu](https://www.youtube.com/watch?v=3bvdL3uWHkE&t=141s) 👈

Installer le serveur FreeRadius en mode simple

`sudo apt update && sudo apt upgrade -y`

`sudo apt install freeradius freeradius-ldap ldap-utils net-tools`

`sudo systemctl enable --now freeradius.service`

`sudo systemctl status freeradius`

Vérification des ports en écoute :
`ss -aun`

![08-PortsEnécoute](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_08-PortsEn%C3%A9coute.png)

### Ajout de la configuration


sudo nano /etc/freeradius/3.0/clients.conf
    client localhost {
        secret = monsecret123
    }
    client pfsense {
        ipaddr = 10.0.0.1
        secret = monsecret123
        require_message_authenticator = no
    }

`ifconfig` (connaître l'adresse IP)

### Ajout de l'utilisateur local

sudo nano /etc/freeradius/3.0/mods-config/files/authorize
    testuser Cleartext-Password := "password123"

sudo systemctl restart freeradius
radtest testuser password123 127.0.0.1 0 monsecret123 (TEST LOCAL)

### Configuration sur PfSense

System / User Manager => AUTHENTICATION Serveurs

FreeRadius
RADIUS
MS-CHAPv2
10.0.0.69 (à remplacer)
Shared secret : monsecret123
Authentication and Accounting
1812
1813
timeout 5
WAN ou LAN (peu importe) : 192.168.42.254

### TEst Authentifaction sur PfSense (visuel)



TEST DEPUIS LA MACHINE RADIUS (penser à remplacer l'IP)
ldapsearch -x -H ldap://10.0.0.80 -b dc=example,dc=com
ldapsearch -x -H ldap://10.0.0.80 -D "cn=admin,dc=example,dc=com" -W -b dc=example,dc=com

### 📚 Ressources:

* [FreeRadius for Debian and Ubuntu](https://www.freeradius.org/documentation/freeradius-server/4.0.0/howto/installation/debian.html)

* [Install FreeRADIUS on Ubuntu](https://www.youtube.com/watch?v=3bvdL3uWHkE&t=141s)

* [FreeRADIUS with AD](https://deployingradius.com/documents/configuration/active_directory.html)