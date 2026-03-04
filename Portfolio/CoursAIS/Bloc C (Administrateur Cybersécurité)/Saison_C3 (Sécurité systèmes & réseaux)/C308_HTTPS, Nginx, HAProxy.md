# Session C308. HTTPS, Nginx, HAProxy.

Notions du jour
•	Certificats HTTPS sur un serveur web
•	reverse proxy avec Nginx (pour voir le principe)
•	load balancing avec HAProxy (idem)
•	défense contre les attaques DDOS, Syn-flood (un type de DDOS) et protections avec rate-limiting

## Reverse-proxy, Load-balancer & HTTPS
Un point d'entrée unique, sécurisé et résilient

## Partie 1 : HTTPS / TLS
Pourquoi le cadenas dans le navigateur compte

### HTTP vs HTTPS

```ps
HTTP (port 80) :
Client --- "login=admin&pass=1234" ---→ Serveur
            \  ⚠️ Lisible par nous
HTTPS (port 443) :
Client --- 🔒 données chiffrées 🔒 --→ Serveur
                ✅ Illisible sans la clé    
```

En HTTP, tout transite **en clair** : identifiants, mots de passe, données perso ...
N'importe qui sur le réseau peut intercepter (man-in-the-middle)

### TLS en 30 secondes

**HTTPS** = HTTP + **TLS** (Transport Layer Security, successeur de SSL)
1. Le client se connecte sur le port 443
2. Le serveur envoie son **certificat** (contient sa clé publique)
3. Le client vérifie le certificat (signé par une autorité de confiance ?)
4. Ils négocient une **clé de session** (chiffrement symétrique)
5. Toutes les données sont chiffrées avec cette clé
_On a vu la crypto symétrique et asymétrique en SA1 - c'est ici que ça sert concrètement !_

### Le certificat TLS
Le certificat contient :

| **Élement**| | **Rôle** |
| --- | --- |
| **Nom de domaine** | Pour quel site le certificat est valide |
| **Clé publique** | Utilisée pour l'échange de clés initial |
| **Autorité de certification (CA)** | Qui a signé (Let's Encrypt, DigiCert...) |
| **Data d'expiration** | Durée de vie limitée |

### Types de certificats

| **Type** | **Usage** | **Navigateur** |
| --- | --- | --- |
| **Auto-signé** | Lab, développement | ❌ Avertissement |
| **Let's Encrypt** | Production, sites publics | ✅ Reconnu |
| **Commercial** (DigiCert...) | Entreprises, e-commerce | ✅ Reconnu + garantie |

_En lab, on utilise des certificats auto-signés. Le principe est le même, seule la confiance change._

### Let's Encrypt & Certbot

Let's Encrypt = autorite de certification gratuite et automatisée
Certbot = l'outil qui gère tout :
```apache
# Installer certbot
sudo apt install certbot python3-certbot-nginx

# Obtenir un certificat et configurer Nginx automatiquement
sudo certbot -- nginx -d monsite.example.com

# Renouvellement automatique (tous les 90 jours)
sudo certbot renew
```

Certbot peut aussi configurer HTTPS sur un reverse-proxy – on verra ça juste après
### HTTPS directement sur Apache
On peut activer HTTPS sur n'importe quel serveur Apache :

```apache
# Activer le module SSL
a2enmod ssl

# Générer un certificat auto-signé (lab)
openssl req -x509 -nodes -days 365 -newkey rsa: 2048 \
-keyout /etc/apache2/ssl/web.key \
-out /etc/apache2/ssl/web.crt \
-subj "/C=FR/0=Lab/CN=web.lab.local"
```
```apache
<VirtualHost *: 443>
ServerName web. lab. local
DocumentRoot /var/www/html
SSLEngine on
SSLCertificateFile /etc/apache2/ssl/web.crt
SSLCertificateKeyFile /etc/apache2/ssl/web.key
</VirtualHost>
```

_Ca fonctionne, mais ça veut dire un certificat par serveur ... On verra comment mieux faire_

## Partie 2 : le Reverse-proxy
_L'intermédiaire entre le client et vos serveurs_

### Le problème :  sans reverse-proxy
```swift
Client A --→ Serveur 1 (10.0.0.20)
Client B --→ Serveur 2 (10.0.0.21)
Client C --→ Serveur 3 (10.0.0.22)

❌ Chaque serveur est exposé directement
❌ Certificat HTTPS sur chaque serveur
❌ Pas de point de contrôle centralisé
```

### La solution : le reverse-proxy


## Pour plus d'information 📚

Voir 👉 [Démonstration Reverse-proxy, Load-balancer & HTTPS](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/Reverse-proxy%2C%20Load-balancer%20%26%20HTTPS.md)

Voir 👉 [Démonstration Anti-DDoS & Rate-limiting](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/Anti-DDoS%20%26%20Rate-limiting.md)
