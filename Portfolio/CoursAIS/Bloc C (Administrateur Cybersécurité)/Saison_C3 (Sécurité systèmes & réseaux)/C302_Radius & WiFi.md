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

![01-Authentification interfaces](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C3%20(S%C3%A9curit%C3%A9%20syst%C3%A8mes%20%26%20r%C3%A9seaux)/images%20C3/images%20C302/C302_01-Authentification%20interfaces.png)

Pour voir les comptes configurés : `show aaa user all`

## Sécurité WiFi 📶

_Comprendre WPA1, WPA2, WPA3 pour faire les bons choix_

### Pourquoi la sécurité Wi-Fi est importante

* Les réseaux mal sécurisés peuvent entraîner :
    * Intrusions
    * Vols de données
    * Piratage des appareils connectés
* WPA (Wi-Fi Protected Access) a été développé pour améliorer la sécurité
* Mais toutes les versions ne se valent pas

### Pourquoi parler de loi ?
* La sécurité Wi-Fi n'est pas qu'une question technique
* Les intrusions réseau sont pénalement répréhensibles
* La démonstration en laboratoire # autorisation réelle
* Importance du cadre légal pour les professionnels IT

### Accès frauduleux à un système
En France (Code pénal - Article 323-1) :

* Accéder ou se maintenir frauduleusement dans un système de traitement automatisé de données est un délit
* Peines possibles :
    * Jusqu'à 2 ans d'emprisonnement
    * 60 000 € d'amende
* Peines aggravées si modification ou suppression de données

### Interception de communications
* Intercepter des communications sans autorisation est illégal
* Même sur un réseau Wi-Fi mal protégé
* Le fait qu'un réseau soit vulnérable ne rend pas son accès légal

### Cas pratique : Wi-Fi voisin
Si le Wi-Fi du voisin est mal sécurisé, peut-on l'utiliser ?
Réponse : NON
* Accès sans autorisation = infraction
* Même sans mot de passe
* Même sans intention malveillante

### Responsabilité en entreprise
* Obligation de sécuriser les systèmes d'information
* Protection des données personnelles (RGPD)
* Risques :
    * Sanctions financières
    * Atteinte à l'image
    * Responsabilité du dirigeant

### Pour éviter que le WiFi soit utilisé pour du contenu louche, on peut :
-	Proxy (reverse proxy)
-	Blocage d’adresses (url)
-	Mettre une CGU (Condition Générale d’utilisation)

## RGPD et Wi-Fi
* Les journaux de connexion peuvent contenir des données personnelles
* Obligation de :
    * Protéger les accès
    * Limiter la conservation des logs
    * Sécuriser les infrastructures réseau

### Pourquoi le WiFi mérite une vraie vigilance 🔍

Le WiFi, c'est un réseau "dans l'air". Contrairement à l'Ethernet, n'importe qui peut écouter ce qui se passe, simplement en étant à portée. Ça ne veut pas dire que le WiFi est dangereux par nature, mais qu'il faut **le configurer correctement** pour qu'il soit sécurisé. Le problème n'est pas le WiFi : le problème, c'est le **mauvais WiFi**.

### WEP : ce n'est pas une option 🚫

WEP est un vieux protocole de chiffrement qui a été cassé depuis longtemps. Avec les outils actuels, il se casse rapidement et de manière fiable. Il n'y a **pas de "bonne pratique" WEP** : soit on l'abandonne, soit on accepte que le réseau soit compromis.

### WPA1 : un patch, pas une solution durable 🩹

WPA1 a été créé pour corriger les failles de WEP sans changer tout le matériel. Il introduit TKIP, qui était une amélioration, mais qui est aujourd'hui considéré comme trop faible. En pratique : WPA1 est un protocole **de transition** qui n'a plus sa place dans un réseau moderne.

### WPA2 : la base solide (quand c'est bien fait) ✅

WPA2 a introduit le chiffrement AES, qui est robuste. C'est toujours la base la plus utilisée, notamment en entreprise. Mais WPA2 n'est pas magique : sa sécurité dépend surtout de la façon dont on l'emploie. Un WPA2 mal configuré reste un WPA2 fragile.

### WPA2 Personal : simple, mais limité 🔑

WPA2 Personal repose sur une clé partagée (PSK). C'est facile à mettre en place, mais ça pose trois problèmes :
* tout le monde a le même mot de passe
* si la clé fuit, tout le réseau est compromis
* impossible de tracer "qui a fait quoi"

C'est acceptable pour de petits réseaux, mais pas pour une entreprise.

### WPA2 Enterprise : la vraie réponse pro 🧩

WPA2 Enterprise remplace la clé partagée par une authentification individuelle via **RADIUS**. Chaque utilisateur a ses identifiants, et l'accès peut être tracé et contrôlé. C'est plus lourd à déployer, mais c'est **nettement plus sécurisé** et surtout **gérable à l'échelle**.

### WPA3 : une évolution logique 🛡️

WPA3 renforce WPA2, notamment contre le brute-force. Il améliore aussi le chiffrement sur les réseaux publics, où l'on ne peut pas toujours authentifier les utilisateurs. En résumé : WPA3 est mieux, mais pas toujours supporté par tous les équipements, donc il faut gérer la compatibilité.

## WPA3 Personal vs Enterprise ⚖️

* **WPA3 Personal** protège mieux un reseau "grand public" (même avec un mot de
passe pas parfait).
* **WPA3 Enterprise** renforce encore l'authentification et le chiffrement, mais exige un
écosystème compatible.

Le choix dépend surtout du matériel en place.

### Comment choisir dans la vraie vie 🎯

* Petit réseau, peu de postes : WPA2 Personal avec un mot de passe solide.
* PME / entreprise : WPA2 Enterprise avec RADIUS.
* Réseau sensible : WPA3 Enterprise si l'infra le permet, sinon WPA2 Enterprise renforcé + segmentation.

### Les erreurs terrain qu'on voit souvent ⚠️

* mot de passe trop faible ou partagé partout
* anciens équipements jamais mis à jour
* réseau invité mélangé au réseau interne

Ce sont souvent ces erreurs qui font tomber un réseau, pas le protocole lui-même.!

## Ce qu'il faut retenir 🧠

* WEP et WPA1 : à éviter, point.
* WPA2 reste une base solide **si bien configuré**.
* WPA3 améliore encore la sécurité, mais demande du matériel compatible.
* Le vrai gain pro : **WPA2/3 Enterprise + RADIUS**.

--- 

Challenge du jour [Challenge C302](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/Challenge_C302.md) : Radius

#