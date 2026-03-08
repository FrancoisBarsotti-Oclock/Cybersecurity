# 🧱 Session C303. DMZ, pare-feu & VPN.

Notions du jour
* Radius
* DMZ
* pare-feu pfSense
* révisions/rappels NAT & redirections de port
* VPN (site-à-site et rappels client-à-site)

## DMZ & pare-feu 🧱
_Isoler pour mieux protéger_
### Pourquoi la DMZ existe
_On a toujours des services exposés à Internet :_

* 🌐 Site web
* 🔐 VPN
* 📧 Serveur mail
* ⚡API publique

La DMZ sert à **contenir le risque** 🛡️

## L'analogie du château fort 🏰

### Imaginez un château médiéval ...
* 🏰 Le **donjon** (au centre) = votre **LAN**

_Les données critiques, les utilisateurs internes_
* **️La cour extérieure** = la **DMZ**

_Les marchands, les visiteurs ... sous surveillance_
* **🌍 L'extérieur = Internet**
_Tout le monde, y compris les assaillants_

### Le principe clé
On accepte que la DMZ soit **plus exposée ⚠️**

Mais si un attaquant compromet la DMZ ...

**Le donjon (LAN) reste protégé ! 🔒**

![01-Architecture DMZ]()

### Le schéma mental

```nginx
Internet ↔ 🔥 Pare-feu ↔ DMZ ↔ 🔥 Pare-feu ↔ LAN
```
**Règle absolue** : pas de route directe Internet → LAN !🚫

## Les règles de flux 🔀

_Qui parle à qui, et dans quel sens ?_

### Internet → DMZ ✅

* Autorisé sur des **services précis**  
* Ex: HTTP (80), HTTPS (443)
* Tout le reste : **bloqué**

### DMZ → LAN (Non) 🔒

* **Très limité !**
* Ex: acces base de donnees sur un port precis
* Jamais d'acces libre vers le LAN

_**Note** : C’est LA règle la plus importante de toute l'architecture_ ⚠️ 

### LAN → DMZ ✅

* Autorisé selon les besoins
* Ex: SSH pour l'administration
* Ex: monitoring des services

### LAN → Internet 🌐

* Généralement autorisé (navigation, mises à jour ... )
* Mais peut être filtré (proxy, inspection SSL ... )

### Quiz rapide 🤔
Un serveur web directement dans le LAN, bonne idée ?

#### Non ! 🚫

Si le serveur web est compromis, l'attaquant a un accès direct au réseau interne

→ C'est exactement pour ça que la DMZ existe !💡

## Les erreurs fréquentes 😬

### Ce qu'on voit trop souvent

* ❌ DMZ trop ouverte vers le LAN
* ❌ Services critiques mal isolés
* ❌ Règles floues ou non documentées
* ❌ Pas de journalisation des flux inter-zones

_Un pare-feu mal configuré donne une **fausse sensation de sécurité**_ 😰

## pfSense : le couteau suisse du pare-feu 🔧

### Pourquoi pfSense ?

* 🆓 Open-source et gratuit
* ️💻 Interface web intuitive
* 🧱 Parfait pour créer une DMZ
* 📊 Logs, monitoring, alertes intégrés

### Ce qu'on peut faire avec pfSense
* Créer l'interface DMZ
* Isoler par règles de firewall
* Créer des **alias** (groupes d'IPs, de ports)
*  Tracer et auditer les flux

On le fait en démo juste après !️ 💻

## NAT et redirections
_Rappel rapide_

[Vidéo sur NAT](https://www.youtube.com/watch?v=jq3SLuhIyPI)

[Tuto DMZ](https://github.com/O-clock-Aldebaran/SC03E03-DMZ)

le principe c'est que le serveur web sur la DMZ est exposé sur internet mais totalement isolé du LAN

pour ceux qui souhaiteraient également reproduire le VPN SITE A SITE (tout dans l'environnement promox) → [on a aussi ce guide]( https://github.com/O-clock-Aldebaran/SC03E03-VPN-SiteASite-Pfsense)

### Pour exposer un service en DMZ

* **DNAT** (redirection de port) : Internet → IP publique:443 → DMZ:443
* **Règle firewall** associée pour autoriser le flux

**Toujours** : _"autoriser ce qui est nécessaire, rien de plus" _ 🎯

### Exemple concret ️💻

Service web hébergé en DMZ — les règles pas à pas

### Étape 1 : Internet → DMZ
```apache
Source: Internet (any)
Destination: DMZ_WEB_SERVER
Ports: 80, 443
Action: ALLOW ✅
```

### Étape 2 : DMZ → LAN (base de données)
```apache
Source: DMZ_WEB_SERVER
Destination: LAN_DB_SERVER
Port: 3306 (MySQL)
Action: ALLOW ✅
```
*Uniquement ce port, et uniquement depuis ce serveur !*

### Étape 3 : LAN → DMZ (administration)
```apache
Source: LAN_ADMIN
Destination: DMZ_WEB_SERVER
Port: 22 (SSH)
Action: ALLOW ✅
```

### Étape 4 : Tout le reste
```apache
Source: any
Destination: any
Action: DENY ❌
```
**Deny by default**, toujours !🔒

### Objectif final 🎯

* ✅Un service exposé, **contrôlé**
* ✅Un LAN **protégé**
* ✅Des règles **lisibles et justifies**

_La segmentation réseau, c'est la base de toute architecture sécurisée_💡

Challenge du jour [Challenge C303](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/Challenge_C303.md)

#

