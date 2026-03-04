# Démo — Anti-DDoS & Rate-limiting 🔥

_On repart de l'infra de la démo précédente : HAProxy tourne sur proxy (10.0.0.30), les 3 back-ends Apache sont actifs._

---

## Rappel de l'état initial

```
[Win11 10.0.0.10]
        │  HTTPS
        ▼
[proxy 10.0.0.30]  ← HAProxy (round-robin, health checks)
        │  HTTP
        ├─► web1 10.0.0.20
        ├─► web2 10.0.0.21
        └─► web3 10.0.0.22
```

Toutes les commandes suivantes s'exécutent **sur le conteneur proxy**, sauf mention contraire.

---

### Étape 1 — Rate-limiting avec HAProxy (stick-table) ⚖️

HAProxy est déjà en place : on ajoute le rate-limiting dans la config existante.

```bash
cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak2

cat > /etc/haproxy/haproxy.cfg << 'EOF'
global
    log /dev/log local0
    log /dev/log local1 notice
    maxconn 2000
    daemon
    ssl-default-bind-ciphers HIGH:!aNULL:!MD5
    ssl-default-bind-options no-sslv3 no-tlsv10 no-tlsv11

defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    option  forwardfor
    timeout connect 5000ms
    timeout client  50000ms
    timeout server  50000ms
    errorfile 503 /etc/haproxy/errors/503.http

# === FRONTEND HTTP → HTTPS ===
frontend http_front
    bind *:80
    http-request redirect scheme https code 301

# === FRONTEND HTTPS avec rate-limiting ===
frontend https_front
    bind *:443 ssl crt /etc/haproxy/ssl/proxy.pem

    # Stick-table : compte requêtes/min et connexions simultanées par IP
    stick-table type ip size 100k expire 60s store http_req_rate(60s),conn_cur

    http-request track-sc0 src
    tcp-request connection track-sc0 src

    # Bloquer si > 100 requêtes/minute par IP → 429
    http-request deny deny_status 429 if { sc_http_req_rate(0) gt 100 }

    # Bloquer si > 20 connexions simultanées par IP → 429
    tcp-request connection reject if { sc_conn_cur(0) gt 20 }

    default_backend web_servers

    stats enable
    stats uri /stats
    stats auth admin:admin
    stats refresh 5s

# === BACKEND ===
backend web_servers
    balance roundrobin
    option httpchk GET /
    http-check expect status 200

    server web1 10.0.0.20:80 check inter 5s fall 3 rise 2
    server web2 10.0.0.21:80 check inter 5s fall 3 rise 2
    server web3 10.0.0.22:80 check inter 5s fall 3 rise 2
EOF
```

```bash
haproxy -c -f /etc/haproxy/haproxy.cfg
systemctl restart haproxy
```

---

### Étape 2 — Tests 🧪

### Installer les outils

```bash
apt install -y apache2-utils siege
```

### Test 1 : sans rate-limiting (baseline)

Arrêter Nginx, relancer HAProxy **sans** les règles deny :

```bash
# Commenter temporairement les lignes deny dans haproxy.cfg, puis :
ab -n 200 -c 20 -k https://10.0.0.30/
```

Observer : 0 failed requests.

### Test 2 : avec HAProxy rate-limiting

Remettre la config complète de l'étape 3, puis :

```bash
# 200 requêtes, 50 simultanées — dépasse la limite de 100/min
ab -n 200 -c 50 -k https://10.0.0.30/
```

Observer dans la sortie :

```
Non-2xx responses:   XX     ← réponses 429 (bloquées)
Failed requests:     XX
```

---

## BONUS DE LA DEMO — Durcissement OS & Nginx

On peut aller plus loin en durcissant le système et en ajoutant du rate-limiting au niveau de Nginx (en frontal ou en complément de HAProxy).

---

### Étape BONUS 1 — Durcissement OS : SYN cookies 🛡️

On protège d'abord la couche TCP contre le SYN flood.

```bash
# Vérifier l'état actuel
sysctl net.ipv4.tcp_syncookies

# Activer les SYN cookies + durcir la file SYN
sysctl -w net.ipv4.tcp_syncookies=1
sysctl -w net.ipv4.tcp_max_syn_backlog=4096
sysctl -w net.ipv4.tcp_synack_retries=2
```

Rendre permanent :

```bash
cat >> /etc/sysctl.conf << 'EOF'
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 4096
net.ipv4.tcp_synack_retries = 2
EOF

sysctl -p
```

---

### Étape BONUS 2 — Limiter les SYN avec iptables 🔥

```bash
# Max 10 nouvelles connexions SYN/seconde par IP (burst 20)
iptables -A INPUT -p tcp --syn \
    -m hashlimit \
    --hashlimit-name syn_limit \
    --hashlimit 10/sec \
    --hashlimit-burst 20 \
    --hashlimit-mode srcip \
    -j ACCEPT

# Rejeter tout SYN au-delà
iptables -A INPUT -p tcp --syn -j DROP

# Vérifier les règles
iptables -L INPUT -v --line-numbers
```

> 💡 Ces deux étapes (sysctl + iptables) se font sur **tous les serveurs exposés**, pas seulement le proxy.

---

### Étape BONUS 3 — Rate-limiting avec Nginx (comparaison) 🟢

On relance Nginx en frontal **à la place de HAProxy** pour comparer l'approche.

```bash
systemctl stop haproxy

# Définir les zones de comptage dans nginx.conf
sed -i '/http {/a \    limit_req_zone $binary_remote_addr zone=general:10m rate=10r/s;\n    limit_req_zone $binary_remote_addr zone=login:1m rate=1r/s;' \
    /etc/nginx/nginx.conf
```

Modifier le vhost :

```bash
cat > /etc/nginx/sites-available/reverse-proxy.conf << 'EOF'
server {
    listen 80;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/proxy.crt;
    ssl_certificate_key /etc/nginx/ssl/proxy.key;
    ssl_protocols       TLSv1.2 TLSv1.3;

    # Limite générale : 10 req/s, burst 20
    location / {
        limit_req zone=general burst=20 nodelay;
        limit_req_status 429;
        proxy_pass http://10.0.0.20/;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # Limite stricte sur une route "sensible" : 1 req/s, burst 5
    location /admin {
        limit_req zone=login burst=5;
        limit_req_status 429;
        return 200 'Admin simulé';
        add_header Content-Type text/plain;
    }
}
EOF
```

```bash
nginx -t && systemctl restart nginx
```

---

### Test BONUS 4 : avec Nginx rate-limiting

```bash
systemctl stop haproxy && systemctl start nginx

# 500 requêtes, 50 simultanées — dépasse 10 req/s
ab -n 500 -c 50 https://10.0.0.30/
```

Voir les 429 en direct :

```bash
tail -f /var/log/nginx/access.log | grep " 429 "
```

### Test BONUS 5 : Siege (plus réaliste)

```bash
siege -c 100 -t 20s https://10.0.0.30/
```

```
Availability:   ~65 %    ← ~35% bloquées = rate-limiting actif
Failed transactions: XXX
```

---

## Résumé : ce qu'on a mis en place

```
[OS - proxy]         SYN cookies + iptables hashlimit
                          ↓ bloque SYN flood
[HAProxy]            stick-table : 100 req/min, 20 conn simultanées
                          ↓ bloque HTTP flood par IP
[Nginx]              limit_req_zone : 10 req/s général, 1 req/s /admin
                          ↓ bloque les rafales
```

| Mécanisme | Commande de vérification |
|-----------|--------------------------|
| SYN cookies actifs | `sysctl net.ipv4.tcp_syncookies` |
| Règles iptables | `iptables -L INPUT -v` |
| Connexions actives | `ss -s` |
| Logs 429 Nginx | `tail -f /var/log/nginx/access.log` |
| Stats HAProxy | `https://10.0.0.30/stats` |
