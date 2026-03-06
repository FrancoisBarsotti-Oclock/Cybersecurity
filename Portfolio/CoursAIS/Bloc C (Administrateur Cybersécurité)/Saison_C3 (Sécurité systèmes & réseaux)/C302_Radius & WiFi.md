# 🔐 Session C302. Radius & WiFi.

Notions du jour : 
* ACL standards et étendues
* NAC, 802.1x
* Radius (intro et découverte via challenge)
* sécurité WiFi

## Serveur Radius

* **NAC** = Logique d’ensemble (auth)

**802.1x** = Protocole de la carte réseau (identifiant/mdp) – contrôle d’accès par port

**Radius** = système d’authentification centralisé

On peut passer au AD/LDAP par Radius

Pour activer Radius sur un serveur Cisco, on active le AAA dans Sevices et met la passerelle dans le client IP, puis « Switch » dans Client Name.

"Secret" sera la connexion qui se fait entre le switch et la connexion radius (clé de chiffrement).

`User set up` sera pour mettre les usernames et mdp pour authentification.

Après on passe à la configuration du switch

En interface vlan → `ip addr` → `no shutdown` → ping le Radius

Pour mettre le Switch en AAA : `aaa new-model`

Pour y mettre le « secret » de Radius : radius-server → `radius-server host <ip> key <secret>`

**Quel type d’authentification** : aaa authentication dot1x default group radius

**Pour activer le processus** : dot1x → dot1x `system-auth-control`

**Configuration d’interfaces** : `interface fa0/2` → `switchport mode access` → `dot1x pae authenticator`

**Bloquer le trafic qui échoue authentification** : authentication port-control → `dot1x pae authenticator auto`

**Attention** : l’authentification doit se faire sur les interfaces qui connectent vers les postes (jamais sur celle qui connecte vers le serveur Radius)

Pour que les postes concernés puissent pinger, il faudra leur fournir de l’identifiant et mdp sur : Desktop → IP Configuration → Activer 802.1X avec les identifiants créés précédemment

![01-Authentification interfaces]()

Pour voir les comptes configurés : `show aaa user all`

## Sécurité WiFi 📶

_Comprendre WPA1, WPA2, WPA3 pour faire les bons choix_

### Pourquoi la sécurité Wi-Fi est importante



### 🚧 En construction 🚧