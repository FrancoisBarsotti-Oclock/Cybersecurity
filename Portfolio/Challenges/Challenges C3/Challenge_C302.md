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


![01-WinSrv]()

* ### Côté Ubuntu 🐧

Après installation d'Ubuntu, on teste la communication entre VMs: 

* `WinSrv-AD → Ubuntu-Radius`:

![02-PingWin2Ubuntu]()

* `UbuntuLDAP → WinSrv&Radius-AD`

![03-PingLDAPToWin&Radius]()

Pas besoin de le faire depuis le serveur de Radius, puisqu'il a déjà donné réponse aux autres deux serveurs.

On se fait l'installation du serveur ldap, en suivant les commandes:

### Installation du serveur ldap

`sudo apt update`

`sudo apt install slapd ldap-utils -y`

`sudo dpkg-reconfigure slapd`

    NON pour configurer le serveur maintenant (sinon on ne peut pas poursuivre).
    Nom de domaine DNS : challenge.C302
    Nom de l’organisation : FranBarso Inc.
    Mot de passe admin : 12345_je_rigole 🤭
    Base de données : généralement MDB (default).
    Supprimer la base existante : NON si vous voulez garder d’anciennes données.

![04-ConfigSlapd](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_04-ConfigSlapd.png)

`sudo systemctl status slapd`

`ldapsearch -x -LLL -H ldap:// -b dc=example,dc=com` → → `ldapsearch -x -LLL -H ldap:// -b dc=challenge,dc=C302`

![05-slapdStauts](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_05-slapdStauts.png)

### Création d'une "OU" (nano base_users.ldif)

`nano base_users.ldif`
    
    dn: ou=users,dc=example,dc=com
    objectClass: organizationalUnit
    ou: users

`sudo ldapadd -x -D cn=admin,dc=example,dc=com -W -f base_users.ldif` → → `sudo ldapadd -x -D cn=admin,dc=challenge,dc=C302 -W -f base_users.ldif`

→ Ici le mdp n'a pas été reconnu, même si j'ai retourné sur la configuration de slapd

![06-InvalidCredentials](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_06-InvalidCredentials.png)

### Création d'un "USER" (nano newuser.ldif)

`nano newuser.ldif`

    dn: uid=gbarsotti,ou=users,dc=example,dc=com
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

`slappasswd` (puis remplacer le mot de passe hashé dans le fichier)

![07-newmdp](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_07-newmdp.png)

`sudo ldapadd -x -D cn=admin,dc=example,dc=com -W -f newuser.ldif` → → `sudo ldapadd -x -D cn=admin,dc=challenge,dc=C302 -W -f newuser.ldif`


### TEST LOCAL
`ldapsearch -x -LLL -b dc=example,dc=com uid=newuser` → → `ldapsearch -x -LLL -b dc=challenge,dc=C302 uid=gbarsotti`

### Installation de freeRADIUS

👉 [FreeRadius for Debian and Ubuntu](https://www.freeradius.org/documentation/freeradius-server/4.0.0/howto/installation/debian.html) 👈

👉 [Install FreeRADIUS on Ubuntu](https://www.youtube.com/watch?v=3bvdL3uWHkE&t=141s) 👈
`sudo apt update && sudo apt upgrade -y`

`sudo apt install freeradius`

`sudo systemctl enable --now freeradius.service`

`sudo systemctl status freeradius`

`ss -aun`

![08-PortsEnécoute](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/C302/Challenge%20C302_08-PortsEn%C3%A9coute.png)

### En construction...

TEST DEPUIS LA MACHINE RADIUS (penser à remplacer l'IP)
ldapsearch -x -H ldap://10.0.0.80 -b dc=example,dc=com
ldapsearch -x -H ldap://10.0.0.80 -D "cn=admin,dc=example,dc=com" -W -b dc=example,dc=com

### 📚 Ressources:

* [FreeRadius for Debian and Ubuntu](https://www.freeradius.org/documentation/freeradius-server/4.0.0/howto/installation/debian.html)

* [Install FreeRADIUS on Ubuntu](https://www.youtube.com/watch?v=3bvdL3uWHkE&t=141s)