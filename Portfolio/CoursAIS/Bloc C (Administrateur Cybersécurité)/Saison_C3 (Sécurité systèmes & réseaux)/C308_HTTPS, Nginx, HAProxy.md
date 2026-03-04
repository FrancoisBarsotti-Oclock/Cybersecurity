# Session C308. HTTPS, Nginx, HAProxy.

Notions du jour
•	Certificats HTTPS sur un serveur web
•	reverse proxy avec Nginx (pour voir le principe)
•	load balancing avec HAProxy (idem)
•	défense contre les attaques DDOS, Syn-flood (un type de DDOS) et protections avec rate-limiting

## Reverse-proxy, Load-balancer & HTTPS 🌐
_Un point d'entrée unique, sécurisé et résilient_

## Partie 1 : HTTPS / TLS 🔒
_Pourquoi le cadenas dans le navigateur compte_

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

### Le certificat TLS 📜
Le certificat contient :

| **Élement**| **Rôle** |
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

### Let's Encrypt & Certbot 🤖

**Let's Encrypt** = autorite de certification **gratuite et automatisée**

**Certbot** = l'outil qui gère tout :
```apache
# Installer certbot
sudo apt install certbot python3-certbot-nginx

# Obtenir un certificat et configurer Nginx automatiquement
sudo certbot -- nginx -d monsite.example.com

# Renouvellement automatique (tous les 90 jours)
sudo certbot renew
```

Certbot peut aussi configurer HTTPS sur un **reverse-proxy** – on verra ça juste après

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

_Ca fonctionne, mais ça veut dire un certificat par serveur ... On verra comment mieux faire_ 💡

## Partie 2 : le Reverse-proxy 🔄
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

```py
                                   ┌─> Serveur 1 (10.0.0.20)
Client ──────> Reverse Proxy ──────┼─> Serveur 2 (10.0.0.21)
               (10.0.0.30)         └─> Serveur 3 (10.0.0.22)

✅ Un seul point d'entrée
✅ Certificat HTTPS uniquement sur le proxy
✅ Les serveurs back-end restent cachés
```

### Analogie 🏨
Le reverse-proxy, c'est comme la **réceptionniste d'un hôtel**

Le client s'adresse à la réception, qui transmet la demande au bon service (ménage, restaurant, technique)

Le client ne connaît pas et n'accède pas directement aux services internes

### Forward proxy vs Reverse proxy

|  | **Forward Proxy** | **Reverse Proxy** |
| --- | --- | --- |
| **Placé côté** | Client | Serveur |
| **Protège** | Les clients | Les serveurs |
| **Exemple** | Proxy Squid en entreprise | Nginx devant Apache |
| **Usage** | Filtrage web, cache, anonymat | HTTPS, LB, sécurité |

_Forward = protège les clients qui sortent. Reverse = protège les serveurs qui reçoivent_

### Terminaison TLS 🔐

Le gros avantage : **HTTPS sur le proxy, HTTP en interne**
```bash
Client ──── HTTPS (chiffré) ────> Reverse Proxy ──── HTTP (clair) ────> Back-end

🔒 → 🔓
(déchiffre ici)
```
* Un seul certificat à gérer et à renouveler
* Les back-ends restent simples (HTTP)
* Moins de charge CPU sur les back-ends

### Les avantages du reverse-proxy

| **Avantage** | **Explication** |
| --- | --- |
| **Terminaison TLS** | HTTPS géré par le proxy, back-ends en HTTP |
| **Sécurité** | Back-ends non exposés directement |
| **Cache** | Mise en cache des réponses statiques |
| **Compression** | Gzip/brotli avant envoi au client |
| **Centralisation** | Logs, certificats, règles en un seul endroit |

## Reverse-proxy avec Nginx 🟢
### Configuration de base

```nginx
server {
    listen 80;
    server_name monsite.example.com;

    location / {
        proxy_pass http://10.0.0.20;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
### Les directives clés

| **Directive** | **Rôle** |
| --- | --- |
| **proxy_pass** | URL du serveur back-end |
| **Host** | Transmet le nom d'hôte original |
| **X-Real-IP** | IP réelle du client (sinon le back-end ne voit que le proxy) |
| **X-Forwarded-For** | Chaîne d'IPs traversées |
| **X-Forwarded-Proto** | HTTP ou HTTPS côté client |

_ Sans ces headers, le back-end croit que toutes les requêtes viennent du proxy !_

### Nginx + HTTPS (terminaison TLS)

```nginx
# Redirection HTTP → HTTPS
server {
    listen 80;
    return 301 https://$host$request_uri;
    }

# Serveur HTTPS
server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/proxy.crt;
    ssl_certificate_key /etc/nginx/ssl/proxy.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    location / {
        proxy pass http://10.0.0.20; # ← HTTP vers le back-end
```
Le client parle en HTTPS au proxy, le proxy parle en HTTP au back-end

### Certbot + Nginx = HTTPS automatique 🤖

Si le serveur est public avec un nom de domaine :

```apache
sudo certbot --nginx -d monsite.example.com
```
Certbot va **automatiquement** :

* Obtenir un certificat Let's Encrypt
* Modifier la config Nginx pour activer SSL
* Configurer la redirection HTTP - HTTPS
* Planifier le renouvellement automatique

_Pas de certificat auto-signé, pas d'avertissement navigateur, zéro effort_

## Reverse-proxy avec Apache 🔴

### Les modules nécessaires
```apache
a2enmod proxy proxy_http ssl rewrite headers
```
### Configuration de base

```apache
<VirtualHost *:443>
    ServerName monsite.example.com
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/proxy.crt
    SSLCertificateKeyFile /etc/apache2/ssl/proxy.key
    ProxyPreserveHost On
    ProxyPass / http://10.0.0.20/
    ProxyPassReverse / http://10.0.0.20/
</VirtualHost>
```
### Nginx vs Apache : Comparaison
| **Fonction** | **Nginx** | **Apache** |
| --- | --- | --- |
| Relayer | proxy_pass | ProxyPass + ProxyPassReverse |
| Conserver le Host | proxy_set_header Host | ProxyPreserveHost On |
| Activer SSL | ssl_certificate | SSLCertificateFile |

En pratique, **Nginx est généralement préféré** comme reverse-proxy : plus léger et plus performant pour cette tâche

## Partie 3 : le Load-balancer ⚖️

_Répartir la charge, survivre aux pannes_

### Le problème : un seul serveur
```apache
1000 clients ──────> 1 serveur ──> 💥 Surchargé !
```

 Un serveur a une capacite limitée. Trop de clients → le site rame ou tombe

 ### La solution : le load-balancer
```apache
                                   ┌─> Serveur 1 (333 req.)
1000 clients ──> Load Balancer ────┼─> Serveur 2 (333 req.)
                                   └─> Serveur 3 (334 req.)
                                   ✅ Charge répartie !
```
Un **load-balancer** distribue les requêtes entre plusieurs serveurs. Si un serveur tombe, les autres prennent le relais.

### Algorithmes de répartition

| **Algorithme** | **Fonctionnement** | **Usage** |
| --- | --- | --- |
| **Round Robin** | Chaque serveur à tour de rôle (1, 2, 3, 1, 2,3...) | Serveurs identiques |
| **Least
Connections** | Vers le serveur le moins chargé | Requêtes de durée variable |
| **Source (IP Hash)** | Même serveur pour la même IP client | Sessions persistantes |
| **Weighted RR** | Round Robin avec poids | Serveurs de capacités
différentes |

### Load-balancer vs reverse-proxy
Un load-balancer **est** un reverse-proxy avec des fonctionnalités en plus :

| **Fonctionnalité** | **Reverse-proxy seul** | **Load-balancer** |
| --- | --- | --- |
| Masquer les back-ends | ✅ | ✅ |
| Terminaison TLS | ✅ | ✅ |
| Répartition de charge | ❌ | ✅ |
| Health checks | ❌ | ✅ |
| Haute disponibilité | ❌ | ✅ |

### HAProxy ⚖️
_Le standard du load-balancing open-source_

### C'est quoi HAProxy ?
* Load-balancer **et** reverse-proxy
* Open-source, extrêmement performant
* Utilisé par GitHub, Stack Overflow, Reddit ...
* Supporte HTTP, HTTPS et TCP

_C'est l'outil de référence pour la répartition de charge en production_

### Architecture de la config
HAProxy s'organise en **4 sections** :
```apache
┌──────────────────────────────────┐
│ global → paramètres globaux      │
│ defaults → valeurs par défaut    │
│                                  │
│ frontend → ce que voit le        │
│ client (entrée)                  │
│                                  │
│ backend → les serveurs           │
│ réels (sortie)                   │
└──────────────────────────────────┘
```
_fontend = porte d’entrée / backend = coulisses_

### Exemple de configuration
```apache
global
    log /dev/log local0
    maxconn 2000
    daemon

defaults
    mode http
    option httplog
    option forwardfor
    timeout connect 5s
    timeout client 50s
    timeout server 50s

frontend https_front
    bind *:443 ssl crt /etc/haproxy/ssl/proxy.pem
    default backend web servers
```
### Les options du backend
| **Option** | **Rôle** |
| --- | --- |
| balance roundrobin | Répartition à tour de rôle |
| option httpchk GET / | Health check : envoie un GET / au back-end |
| http-check expect status 200 | UP si le serveur répond 200 |
| check inter 5s | Vérification toutes les 5 secondes |
| fall 3 | 3 échecs → serveur marqué **DOWN** ↓ |
| rise 2 | 2 succès → serveur marqué **UP** ↑ |

```apache
cat /etc/ssl/proxy.crt /etc/ssl/proxy.key > /etc/haproxy/ssl/proxy.pem
chmod 600 /etc/haproxy/ssl/proxy.pem
```
### Health checks : la haute disponibilité 💓

HAProxy vérifie en permanence que les back-ends sont vivants
```apache
                    check OK ✅ check OK ✅ check FAIL ❌
                    ┌──────┐ ┌──────┐ ┌──────┐
                    │ web1 │ │ web2 │ │ web3 │
                    │ UP   │ │ UP   │ │ DOWN │
                    └──────┘ └──────┘ └──────┘
                        ↑               ↑
HAProxy ────────────────┴───────────────┘
        (ne route plus vers web3)
```
* Un serveur tombe → exclu automatiquement
* Il revient → réintégré automatiquement
* Aucune intervention manuelle nécessaire

### La page de stats HAProxy 📊
```apache
frontend https_front
    stats enable
    stats uri /stats
    stats auth admin:password
    stats refresh 5s
```
Accessible sur https://proxy/stats — on y voit :

* L'état de chaque serveur (vert/rouge)
* Le nombre de requêtes par serveur
* Les temps de réponse
* Les connexions actives

_Très utile pour le monitoring et le debugging !_

## Récap' : qui fait quoi ? 📋
```apache
┌─────────────────────────────────────────────────┐
│                    HTTPS / TLS                  │
│ Chiffre la communication client ↔ serveur       │
└────────────────────┬────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────┐
│                 Reverse-proxy                   │
│ Point d'entrée unique, terminaison TLS,         │
│ masque les back-ends                            │
│ (Nginx, Apache)                                 │
└────────────────────┬────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────┐
│                 Load-balancer                   │
│ Répartit la charge, health checks,              │
│ haute disponibilité                             │
```
### Les outils qu'on a vus

| **Outil** | **Rôle** | **Pointfort** |
| --- | --- | --- |
| **Certbot/ Let's Encrypt** | Certificats TLS gratuits | Automatique |
| **Nginx** | Reverse-proxy + serveur web | Léger, performant |
| **Apache** | Reverse-proxy + serveur web | Flexible, modules riches |
| **HAProxy** | Load-balancer + reverse-proxy | Répartition, health checks, stats |

# Disponibilité sous attaque 🔥
_DDoS, SynFlood, rate-limiting : comprendre et résister_

### Un scénario réel pour commencer 🎬
Il est 14h. Votre site est en ligne.

```apache
14:00 → 50 req/s  ✅ normal
14:01 → 200 req/s ⚠️ inhabituel
14:02 → 800 req/s 🔴 saturation
14:03 → le site ne répond plus
```
_Votre service vient d’être victime d’une attaque DDoS. Que s’est-il passé ?_

### Ce qui s’est passé côté serveur
```ruby
                [Serveur web]
                RAM : 8 Go

req 1   → thread crée - 10 Mo RAM

req 2   → thread cree - 10 Mo RAM
…
req 800  → thread créé - 8 Go RAM - SATURÉ

req 801  → refusée (plus de mémoire)
req 802  → refusée
…
Vrais utilisateurs - timeout ❌
```
_Le serveur a une capacité finie. L’attaquant l’exploite._

### Anatomie d'une attaque DDoS 💥
**DDoS = Distributed Denial of Service**

_Objectif_ : rendre un service **indisponible** en le surchargeant

### DoS vs DDoS
```ruby
DoS — une seule source

    [Attaquant] ──────────────────► [Serveur]
                100 Mbps

    → Facile à bloquer : on bannit l'IP

DDoS — sources multiples (botnet)
    [Bot 1] ──┐
    [Bot 2] ──┤
    [Bot 3] ──┼──────────────────► [Serveur]
    [Bot 4] ──┤ 1 000 sources
    [Bot N] ──┘ × 10 Mbps chacun
                = 10 Gbps total
```
### Comment se crée un botnet ?
```apache
Attaquant
    │
    ▼
Serveur C2 (Command & Control)
    │
    ├──► Bot (PC infecté, Paris)
    ├──► Bot (caméra IP piratée, Tokyo)
    ├──► Bot (NAS vulnérable, Londres)
    └──► Bot (smartphone compromis, New York)
Sur ordre : "attaque target.com maintenant"
→ Tous les bots envoient des requêtes simultanément
```
_Les bots sont souvent des objets connectés mal sécurisés (IoT)_

### Les catégories d’attaques DDoS
| **Couche** | **Type** | **Exemple** | **Débit** |
| --- | --- | --- | --- |
| L3/L4 | Volumétrique | UDP Flood, ICMP Flood | Tbps |
| L4 | Protocole | SynFlood, ACK Flood | Mpps |
| L7 | Applicatif | HTTP Flood, Slowloris | req/s |

* **Volumétrique** : sature la bande passante réseau
* ** Protocole** : épuise les ressources réseau (tables de connexions)
* ** Applicatif** : épuise les ressources du serveur (CPU, RAM)

_Les attaques L7 sont les plus sournoises : trafic HTTP légitime en apparence_

### Exemples réels marquants
* **GitHub (2018)** : 1,35 Tbps via amplification Memcached
* **OVHcloud (2020)** : 1,1 Tbps depuis un botnet de caméras IP
* **Cloudflare (2023)** : 71 millions de req/s - record mondial à l'époque

_Ces attaques dépassent largement la capacité d'un seul serveur à se défendre_

### Mitigation # Protection
* **Protection** : empêcher l'attaque d'arriver (CDN, scrubbing center ... )
* ** Mitigation** : réduire l'impact d'une attaque en cours (rate-limiting, ban IP ... )

**On ne supprime jamais une DDoS** - on l'absorbe ou on la rend inefficace.

### Pourquoi le firewall seul ne suffit pas 🧱
```yaml
Firewall réseau — ce qu'il voit :
┌─────────────────────────────────────┐
│ IP src: 1.2.3.4                     │
│ IP dst: 10.0.0.1                    │
│ Port: 80 ← autorisé✅               │
│ Protocole: TCP                      │
│                                     │
│ Contenu HTTP : ??? ← invisible      │
└─────────────────────────────────────┘
```
* Le port 80/443 **doit** être ouvert → le firewall laisse tout passer
* 1 000 requêtes HTTP depuis 1 000 IPs : chacune est "légitime" vue du firewall

_Le firewall protège le périmètre réseau, pas contre la surcharge applicative_

### Les couches de défense
Pas une seule solution, mais plusieurs niveaux :
```apache
Internet
    │
[1] CDN / Scrubbing (Cloudflare...) ← absorbe le volume
    │
[2] Firewall L3/L4 + iptables ← bloque les IPs, SynFlood
    │
[3] Reverse proxy (Nginx / HAProxy) ← rate-limiting par IP
    │
[4] Application ← validation métier
```
_Chaque couche arrête ce qu'elle peut voir. Ensemble, elles couvrent tout le spectre._

## Couche OS : le SynFlood en détail 🔬
### Le TCP 3-way handshake (rappel)
```apache
Client            Serveur
│                    │
│──── SYN ──────────►│ "Je veux me connecter"
│                    │ ← entrée dans la table (LISTEN)
│◄─── SYN-ACK ───────│ "OK, j'attends ta confirmation"
│                    │
│──── ACK ──────────►│ "C'est confirmé"
│                    │ ← connexion établie (ESTABLISHED)
```
_La table des connexions en attente a une taille limitée_

### Le SynFlood : exploiter cette limite
```apache
Attaquant (IPs falsifiées)    Serveur
│                               │
│──── SYN (IP: 1.2.3.4) ───────►│ → table[1] ⏳
│──── SYN (IP: 5.6.7.8) ───────►│ → table[2] ⏳
│──── SYN (IP: 9.0.1.2) ───────►│ → table[3] ⏳
│──── SYN (IP: ...) ────────────►│ → table[N] ← PLEINE
│                               │
│                               │ ACK des vrais clients ?
Vrai client ── SYN ─────────────►│ → rejeté ❌ (table pleine)
```
_LesIPssont forgées: les SYN-ACK du serveur partent dansle vide, la table reste saturée_

### Contre-mesure n°1 : SYN cookies (noyau Linux)
```apache
# Activer les SYN cookies (pas de table = pas de saturation)
sudo sysctl -w net.ipv4.tcp_syncookies=1

# Augmenter la file d'attente SYN
sudo sysctl -w net.ipv4.tcp_max_syn_backlog=4096

# Réduire le nombre de retransmissions SYN-ACK (libère plus vite)
sudo sysctl -w net.ipv4.tcp_synack_retries=2
```

#### Rendre permanent dans /etc/  . conf :
```apache
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 4096
net.ipv4.tcp_synack_retries = 2
```
### Contre-mesure n°2 : limiter les SYN avec iptables
```apache
# Accepter max 10 connexions SYN/seconde par IP (burst 20)
iptables -A INPUT -p tcp --syn \
    -m hashlimit \
    --hashlimit-name syn_limit \
    --hashlimit 10/sec \
    --hashlimit-burst 20 \
    --hashlimit-mode srcip \
    -j ACCEPT

# Rejeter tout SYN qui dépasse la limite
iptables -A INPUT -p tcp --syn -j DROP
```
* hashlimit : table de comptage par IP (dans le noyau)
* burst 20 : autoriser un pic ponctuel jusqu'à 20, puis limiter à 10/s

### Contre-mesure n°3 : nftables (alternative moderne à iptables)
```apache
# nftables est le successeur d'iptables (Debian 10+, RHEL 8+)
sudo apt install nftables

# /etc/nftables.conf
table inet filter {
    chain input {
        type filter hook input priority 0; policy drop;

        # Limiter les SYN à 10/seconde par IP
        tcp flags syn \
            limit rate over 10/second \
            drop
        # Accepter les connexions établies
        ct state established,related accept
        tcp dport { 22, 80, 443 } accept
```
_nftables = syntaxe plus claire, meilleures performances, remplace iptables/ip6tables/arptables_

## Couche applicative : le rate-limiting 🚦
_Limiter le débit par IP au niveau du reverse proxy_

### Le principe du rate-limiting
```apache
IP 1.2.3.4 :
│ req 1 (t=0s) → OK ✅
│ req 2 (t=0.1s) → OK ✅
│ req 3 (t=0.2s) → OK ✅
│ ...
│ req 101 (t=5s) → BLOQUÉE 🚫 (limite : 100/min)
│ req 102 (t=5s) → BLOQUÉE 🚫
│
IP 4.5.6.7 (utilisateur légitime) :
│ req 1 (t=5s) → OK ✅ (quota non atteint)
```
_Le rate-limiting ne stoppe pas l'attaque, mais la rend inoffensive pour les autres_

### Rate-limiting avec Nginx
Nginx intègre nativement le rate-limiting via limit_req_zone :
```ruby
# Dans nginx.conf (bloc http)
http {
    # Définir une zone de comptage : 10 Mo pour ~160 000 IPs
    # Limite : 10 requêtes par seconde par IP
    limit_req_zone $binary_remote_addr zone=general:10m rate=10r/s;
    # Zone stricte pour les endpoints sensibles
    limit_req_zone $binary_remote_addr zone=login:1m rate=1r/s;
}
```
### Appliquer les zones dans un vhost Nginx
```ruby
server {
    listen 80;

    # Appliquer la limite sur toutes les routes
    location / {
        limit_req zone=general burst=20 nodelay;
        limit_req_status 429; # 429 = Too Many Requests
        proxy_pass http://backend;
    }
    # Zone stricte sur le formulaire de connexion
    location /login {
        limit_req zone=login burst=5;
        limit_req_status 429;
        proxy_pass http://backend;
}
```
* burst=20 : absorber un pic de 20 requêtes supplémentaires
* nodelay : traiter le burst immédiatement (sans mise en file)
* 429 : code HTTP standard pour "Too Many Requests"

### Comprendre le burst
```yaml
Limite : 10 req/s, burst=20

Sans burst :
├── req 1 → OK ├── req 11 → 🚫 bloquée immédiatement

Avec burst=20 :
├── req 1..20 → OK (le burst absorbe le pic)
├── req 21 → 🚫 bloquée
├── chaque seconde : 10 créneaux se libèrent

Avec burst=20 nodelay :
├── req 1..20 → OK (traitées immédiatement, pas en file)
├── req 21 → 🚫 bloquée
```
_Le burst évite de pénaliser un utilisateur qui charge une page avec beaucoup de ressources_

### Rate-limiting avec HAProxy
HAProxy utilise les **stick-tables** pour compter les requêtes :
```py
frontend http-in
    bind *:80

    # Table de comptage : IP → taux de requêtes sur 60s + connexions actives
    stick-table type ip size 100k expire 60s \
        store http_req_rate(60s),conn_cur

    # Enregistrer chaque requête dans la table
    http-request track-sc0 src
    tcp-request connection track-sc0 src

    # Bloquer si > 100 requêtes/minute
    http-request deny if { sc_http_req_rate(0) gt 100 }
    
    # Bloquer si > 20 connexions simultanées (anti-SynFlood)
    tcp-request connection reject if { sc conn cur(0) gt 20 }
```
### Limiter la capacité globale dans HAProxy
```apache
global
    # Limite totale de connexions simultanées (toutes IPs confondues)
    maxconn 10000

frontend http-in
    bind *:80
    # Limite par process HAProxy
    maxconn 2000
```
* maxconn global : protège les ressources système
* maxconn frontend : fine-grained par frontend

_Si maxconn est atteint, les nouvelles connexions attendent en file plutôt que de crasher_

### Nginx vs HAProxy : lequel choisir ?
| **Critère** | **Nginx limit_req** | **HAProxy stick-table** |
| --- | --- | --- |
| Facilité de config | ✅simple | Moyenne |
| Granularité | Par location/vhost | Par frontend/backend |
| Compteurs multiples | ❌(1 zone = 1 compteur) | ✅(plusieurs dans une table) |
| Statistiques en direct | ❌ | ✅(page stats HAProxy) |
| Idéal pour | Serveur web + proxy | Load balancer dédié |

_Les deux peuvent coexister : Nginx en frontal, HAProxy en load balancer derrière_

## Tester sa protection
_Une protection non testée est une protection incertaine_

### ApacheBench : test de charge simple
```yaml
sudo apt install apache2-utils

# 1000 requêtes total, 50 simultanées
ab -n 1000 -c 50 http://localhost/
```
#### Sortie pertinente :

```yaml
Concurrency Level: 50
Requests per second: 847.32 [#/sec]
Failed requests: 312 ← celles bloquées par le rate-limiting
Non-2xx responses: 312 ← réponses 429/503
Percentage of the requests served within a certain time (ms)
    50%      12
    99%     245
   100%    1832 (longest request)
```
_On veut voir des "Failed requests" - c'est la preuve que le rate-limiting fonctionne_

# 💡 Note à bien retenir! 🧠
Pour surveiller son serveur en temps réel
```yaml
apt install -y htop
htop -H
```

---

### Siege: test plus réaliste
```ruby
sudo apt install siege

# 100 utilisateurs simultanés pendant 30 secondes
siege -c 100 -t 30s http://localhost/
```
```swift
Transactions: 2847 hits
Availability: 68.32 % ← ~32% bloquées = rate-limiting actif
Elapsed time: 30.01 secs
Response time: 0.09 secs
Transaction rate: 94.87 trans/sec
Failed transactions: 901
```
### Vérifier les logs en temps réel
```apache
# Nginx : voir les 429 en direct
tail -f /var/log/nginx/access.log | grep " 429 "

# HAProxy : stats en temps réel (si stats activées)
echo "show stat" | socat stdio /var/run/haproxy/admin.sock | cut -d',' -f1,2,48

# Nombre de connexions actives
ss -s
```
## Défense en profondeur : la vue d'ensemble ️

### Quelle couche arrête quoi ?
```apache
Internet
    │
    ▼
[Cloudflare / CDN] → attaques volumétriques (Gbps, Tbps)
    │ bots, scanners, IP malveillantes
    ▼
[Firewall / iptables] → SYN flood, scan de ports
    │ hashlimit, connexions SYN
    ▼
[Nginx / HAProxy] → HTTP flood par IP
    │ rate-limiting, maxconn
    ▼
[Application] → brute-force login, scraping
    │ CAPTCHA, tokens CSRF, fail2ban
    ▼
[Base de données] → injection, surcharge de requêtes
```
### Les seuils recommandés (point de départ)

| **Mécanisme** | **Seuil de départ** | **À ajuster selon** |
| --- | --- | --- |
| SYN cookies | Toujours actif | — |
| iptables hashlimit SYN | 10/s, burst 20 | Trafic légitime |
| Nginx limit_req (général) | 10 req/s, burst 20 | Profil d'utilisation |
| Nginx limit_req (login) | 1 req/s, burst 5 | Jamais plus |
| HAProxy http_req_rate | 100/min par IP | Analytics trafic |
| HAProxy conn_cur | 20 simultanées | Type d'application |

_Ces valeurs sont des points de départ - un audit du trafic normal est indispensable avant de déployer_

### Processus de mise en place recommandé
```swift
1. Observer     → analyser les logs, mesurer le trafic normal
        ↓
2. Définir      → fixer des seuils au-dessus du pic légitime connu
        ↓
3. Déployer     → en mode log-only d'abord (pas de blocage)
        ↓
4. Ajuster      → affiner selon les faux positifs observés
        ↓
5. Activer      → passer en mode blocage
        ↓
6. Tester       → ab/siege pour vérifier que ça bloque bien
        ↓
7. Surveiller   → alertes sur les métriques (Grafana, Zabbix...)
```
## ### Ce qu'il faut retenir 🧠
* Un DDoS exploite la **capacité finie** des ressources (RAM, connexions, CPU)
* Le **SynFlood** sature la table des connexions TCP - tcp_syncookies + iptables hashlimit
* Le **rate-limiting** limite les requetes par IP : Nginx limit_req_zone, HAProxy stick-table
* Le burst absorbe les pics légitimes sans pénaliser les vrais utilisateurs
* La défense est **multicouche** : pas une seule solution mais un empilement

### Les commandes à retenir
```ruby
# Tester le rate-limiting
ab -n 1000 -c 50 http://localhost/

# Activer les SYN cookies
sysctl -w net.ipv4.tcp_syncookies=1

# Voir les connexions en cours
SS -S

# Voir les logs Nginx en direct
tail -f /var/log/nginx/access.log
```

---

## Pour plus d'information 📚

Voir 👉 [Démonstration Reverse-proxy, Load-balancer & HTTPS](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/Reverse-proxy%2C%20Load-balancer%20%26%20HTTPS.md)

Voir 👉 [Démonstration Anti-DDoS & Rate-limiting](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/Anti-DDoS%20%26%20Rate-limiting.md)
