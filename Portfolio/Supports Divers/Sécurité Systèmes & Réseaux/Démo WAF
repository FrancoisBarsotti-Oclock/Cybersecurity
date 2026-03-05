# Démo — WAF avec ModSecurity + OWASP CRS

_Version simplifiée — 2 machines, 30 minutes_

---

## Architecture

```
  [attacker]          [waf-proxy]            [web]
  curl / nikto  →→→  Nginx + ModSec  →→→  Apache
  10.0.0.83          10.0.0.80              10.0.0.81
```

| Machine | Type | IP | Rôle |
|---------|------|----|------|
| web | LXC Debian 12 | 10.0.0.81 | Apache — backend |
| waf-proxy | LXC Debian 12 | 10.0.0.80 | Nginx + ModSecurity + OWASP CRS |

Les attaques sont lancées **depuis waf-proxy lui-même** (pas besoin d'une 3e VM).

---

## Étape 1 — Préparer le backend (web)

--

### Créer le conteneur web

Dans Proxmox → **Créer CT** :

| Champ | Valeur |
|-------|--------|
| CT ID | 181 |
| Hostname | web |
| Template | debian-12 |
| Disque | 4 Go |
| CPU | 1 |
| RAM | 256 Mo |
| Bridge | vmbr2 |
| IPv4 | 10.0.0.81/16 |
| GW | 10.0.0.1 |

--

### Installer Apache

```bash
apt update && apt install -y apache2 curl
echo "<h1>Web backend OK</h1>" > /var/www/html/index.html
systemctl enable --now apache2
```

Vérification rapide :

```bash
curl http://10.0.0.81
# → <h1>Web backend OK</h1>
```

---

## Étape 2 — Préparer le proxy WAF

--

### Créer le conteneur waf-proxy

| Champ | Valeur |
|-------|--------|
| CT ID | 180 |
| Hostname | waf-proxy |
| Template | debian-12 |
| Disque | 4 Go |
| CPU | 1 |
| RAM | 512 Mo |
| Bridge | vmbr2 |
| IPv4 | 10.0.0.80/16 |
| GW | 10.0.0.1 |

--

### Installer Nginx + ModSecurity + CRS

Sur **waf-proxy** :

```bash
apt update && apt install -y \
    nginx \
    libnginx-mod-http-modsecurity \
    modsecurity-crs curl
```

> `modsecurity-crs` installe le paquet Debian officiel qui place les règles dans `/usr/share/modsecurity-crs/`

--

### Vérifier les fichiers installés

```bash
ls /usr/lib/nginx/modules/ | grep modsecurity
# → ngx_http_modsecurity_module.so

ls /usr/share/modsecurity-crs/rules/ | head -5
# → règles CRS prêtes à l'emploi

ls /etc/modsecurity/crs/
# → crs-setup.conf
# → REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
# → RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf
```

---

## Étape 3 — Configurer ModSecurity

--

### Créer la config de base

Le paquet Debian ne fournit pas de `modsecurity.conf-recommended`. On le crée avec le minimum requis :

```bash
cat > /etc/modsecurity/modsecurity.conf << 'EOF'
SecRuleEngine DetectionOnly
SecRequestBodyAccess On
SecResponseBodyAccess Off
SecAuditEngine RelevantOnly
SecAuditLog /var/log/modsec_audit.log
SecAuditLogParts ABIJDEFHZ
SecAuditLogType Serial
EOF

# Créer le fichier d'audit log (requis avant le démarrage)
touch /var/log/modsec_audit.log
```

```bash
# Vérifier le mode (DetectionOnly par défaut)
grep SecRuleEngine /etc/modsecurity/modsecurity.conf
# → SecRuleEngine DetectionOnly
```

--

### Créer le fichier de règles Nginx

```bash
cat > /etc/nginx/modsecurity.conf << 'EOF'
Include /etc/modsecurity/modsecurity.conf
Include /etc/modsecurity/crs/crs-setup.conf
Include /usr/share/modsecurity-crs/rules/*.conf
EOF
```

---

## Étape 4 — Configurer Nginx en reverse proxy

--

### Créer le VirtualHost

```bash
cat > /etc/nginx/sites-available/waf.conf << 'EOF'
server {
    listen 80;
    server_name _;

    modsecurity on;
    modsecurity_rules_file /etc/nginx/modsecurity.conf;

    location / {
        proxy_pass http://10.0.0.81/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
EOF

ln -s /etc/nginx/sites-available/waf.conf /etc/nginx/sites-enabled/
rm -f /etc/nginx/sites-enabled/default
```

--

### Démarrer

```bash
nginx -t && systemctl reload nginx
```

Test — accès normal via le proxy :

```bash
curl http://10.0.0.80
# → <h1>Web backend OK</h1>  ✅
```

---

## Étape 5 — Observer les attaques (mode DetectionOnly)

--

### Ouvrir les logs en temps réel

**Terminal 1 :**

```bash
tail -f /var/log/modsec_audit.log
```

> Les détections ModSecurity vont dans l'audit log, pas dans le error.log nginx.

**Terminal 2 (pour envoyer les attaques) :**

--

### Test : Injection SQL

```bash
# Les guillemets simples doivent être encodés en %27
curl "http://10.0.0.80/?id=1%27%20OR%20%271%27%3D%271"
# Equivalent à : http://10.0.0.80/?id=1' OR '1'='1
```

→ La requête **passe** (DetectionOnly) mais l'audit log affiche :

```log
[id "949110"] [msg "Inbound Anomaly Score Exceeded (Total Score: 33)"]
[id "942100"] [msg "SQL Injection Attack Detected via libinjection"]
[severity "CRITICAL"] [tag "OWASP_CRS/3.3.7"]
```

> Le CRS utilise l'**anomaly scoring** : chaque règle ajoute des points (SQLi CRITICAL = +5).
> Le score total (ici 33) est comparé au seuil (5 par défaut) — dépassé → alerte.

--

### Test : XSS

```bash
# Les balises < > encodées en %3C %3E
curl "http://10.0.0.80/?q=%3Cscript%3Ealert(1)%3C%2Fscript%3E"
# Equivalent à : http://10.0.0.80/?q=<script>alert(1)</script>
```

→ Log :

```log
[id "941100"] [msg "XSS Attack Detected via libinjection"]
```

--

### Test : Path Traversal

```bash
curl "http://10.0.0.80/?file=../../../../etc/passwd"
```

→ Log :

```log
[id "930100"] [msg "Path Traversal Attack"]
```

---

## Étape 6 — Bloquer les attaques (mode Prevention)

--

### Passer en mode Prevention

```bash
sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' \
    /etc/modsecurity/modsecurity.conf

nginx -t && systemctl reload nginx
```

--

### Vérifier que le trafic légitime passe toujours

```bash
curl http://10.0.0.80/
# → 200 OK ✅

curl "http://10.0.0.80/?nom=Alice&age=25"
# → 200 OK ✅
```

--

### Les attaques sont maintenant bloquées

```bash
curl -v "http://10.0.0.80/?id=1%27%20OR%20%271%27%3D%271"
# → HTTP/1.1 403 Forbidden  🚫

curl -v "http://10.0.0.80/?q=%3Cscript%3Ealert(1)%3C%2Fscript%3E"
# → HTTP/1.1 403 Forbidden  🚫

curl -v "http://10.0.0.80/?file=../../../../etc/passwd"
# → HTTP/1.1 403 Forbidden  🚫
```

--

### Tableau récap'

| Attaque | Sans WAF | DetectionOnly | Prevention |
|---------|----------|---------------|------------|
| SQL Injection | passe | passe + log | 403 bloqué |
| XSS | passe | passe + log | 403 bloqué |
| Path Traversal | passe | passe + log | 403 bloqué |
| Requête normale | passe | passe | passe |

---

## Récap' des étapes

```text
web (10.0.0.81)         Apache backend simple
waf-proxy (10.0.0.80)   Nginx + ModSecurity + CRS (paquet apt)
                        │
                        ├─ DetectionOnly → log les attaques sans bloquer
                        └─ Prevention   → HTTP 403 sur toute requête suspecte
```
