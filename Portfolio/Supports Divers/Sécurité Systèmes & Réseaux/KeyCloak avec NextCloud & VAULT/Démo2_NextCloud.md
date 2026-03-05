# Démo — Keycloak + Nextcloud (SSO / OIDC)

_Mise en pratique : intégration d'une deuxième application (Nextcloud) dans le SSO Keycloak_

---

## Architecture du lab (mise à jour)

```
        [Client navigateur]
         Win11 — 10.0.0.10
                │
   ┌────────────┴─────────────────────────────────────────┐
   │           vmbr2 — LAN A (10.0.0.0/16)                │
   └──┬──────────────┬──────────────┬──────────────┬──────┘
      │              │              │              │
  10.0.0.60      10.0.0.62     10.0.0.63      10.0.0.64
      │              │              │              │
  [keycloak]      [vault]       [web-app]     [nextcloud]
  Keycloak 26    Vault 1.x     (optionnel)   Nextcloud 30
  port 8080      port 8200                   port 80
```

| Machine | Type | IP | Rôle |
|---------|------|----|------|
| keycloak | LXC | 10.0.0.60 | Identity Provider (déjà configuré) |
| vault | LXC | 10.0.0.62 | HashiCorp Vault (déjà configuré) |
| nextcloud | LXC | 10.0.0.64 | Nextcloud (nouvelle application) |
| win11 | VM | 10.0.0.10 | Client navigateur pour les tests |

💡 Keycloak est déjà opérationnel (démo précédente). On ajoute Nextcloud comme **deuxième Service Provider** dans le même realm `demo`.

---

## Étape 1 — Déclarer un nouveau Client Nextcloud dans Keycloak

--

### Accéder à la console Keycloak

Depuis **Win11** :

```
http://10.0.0.60:8080/admin
```

Se connecter avec le compte admin, sélectionner le realm **`demo`**.

--

### Créer le client OIDC pour Nextcloud

Dans le realm `demo` :

1. **Clients** → **Create client**
2. Onglet **"General Settings"** :

| Champ | Valeur |
|-------|--------|
| Client type | OpenID Connect |
| Client ID | `nextcloud` |
| Name | `Nextcloud` |

3. Cliquer **"Next"**

--

### Paramètres du flux d'authentification

Onglet **"Capability config"** :

| Option | Valeur |
|--------|--------|
| Client authentication | ON (confidential) |
| Standard flow | ON |
| Direct access grants | OFF |

Cliquer **"Next"**

--

### URLs de redirection Nextcloud

Onglet **"Login settings"** :

| Champ | Valeur |
|-------|--------|
| Root URL | `https://10.0.0.64` |
| Valid redirect URIs | `https://10.0.0.64/index.php/apps/user_oidc/code` |
| Valid redirect URIs | `https://10.0.0.64/*` |
| Web origins | `https://10.0.0.64` |

Cliquer **"Save"**

--

### Récupérer le Client Secret

1. **Clients** → `nextcloud` → onglet **"Credentials"**
2. Copier la valeur du **Client secret**

```
# Exemple :
Client ID     : nextcloud
Client Secret : NcDeFg9876543210AbC
```

> 💡 Ce secret sera utilisé pour configurer l'app OIDC dans Nextcloud.

--

### Ajouter le mapper de groupes pour Nextcloud

Le client `nextcloud` a besoin que les groupes soient transmis dans le token JWT.

1. **Clients** → `nextcloud` → onglet **"Client scopes"**
2. Cliquer sur `nextcloud-dedicated`
3. **"Add mapper"** → **"By configuration"** → **"Group Membership"**
4. Configurer :

| Champ | Valeur |
|-------|--------|
| Name | `groups` |
| Token Claim Name | `groups` |
| Full group path | OFF |
| Add to ID token | ON |
| Add to access token | ON |

Cliquer **"Save"**

--

### Créer un groupe dédié Nextcloud

Dans le realm `demo` :

1. **Groups** → **"Create group"**
2. Name : `nextcloud-users`
3. Cliquer **"Create"**

--

### Ajouter Alice au groupe Nextcloud

1. **Users** → `alice` → onglet **"Groups"**
2. Cliquer **"Join Group"**
3. Sélectionner `nextcloud-users`
4. Cliquer **"Join"**

→ Alice appartient maintenant aux groupes `vault-readers` **et** `nextcloud-users`

---

## Étape 2 — Installer Nextcloud sur 10.0.0.64

--

### Préparer le système (LXC Debian/Ubuntu)

Sur le conteneur **nextcloud** (10.0.0.64) :

```bash
apt update && apt upgrade -y

# Installer Apache, PHP et les extensions nécessaires à Nextcloud
apt install -y apache2 \
    php php-cli php-common \
    php-curl php-gd php-xml php-mbstring \
    php-zip php-intl php-bcmath \
    php-gmp php-imagick php-apcu \
    libapache2-mod-php \
    mariadb-server \
    unzip wget curl \
    mariadb-client-compat bzip2

rm /var/www/html/index.html
```

--

### Configurer MariaDB

```bash
# Démarrer MariaDB
systemctl enable --now mariadb

# Sécuriser l'installation
mysql_secure_installation
# → Répondre Y à toutes les questions, définir un mot de passe root
```

```bash
# Créer la base de données Nextcloud
mysql -u root -p << 'EOF'
CREATE DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER 'ncuser'@'localhost' IDENTIFIED BY 'NcDbPass42!';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'ncuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
EOF
```

--

### Télécharger et installer Nextcloud

```bash
# Télécharger la dernière version de Nextcloud
wget https://download.nextcloud.com/server/releases/latest.tar.bz2

# Extraire dans le répertoire web
tar -xjf latest.tar.bz2 -C /var/www/html/

# Ajuster les permissions
chown -R www-data:www-data /var/www/html/nextcloud
chmod -R 750 /var/www/html/nextcloud
```

--

### Générer un certificat auto-signé

Nextcloud **impose HTTPS** pour utiliser OpenID Connect.

```bash
# Générer un certificat auto-signé
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/nextcloud.key \
    -out /etc/ssl/certs/nextcloud.crt \
    -subj "/CN=10.0.0.64/O=Lab/C=FR"
```

--

### Configurer Apache pour Nextcloud (HTTPS)

```bash
# Activer les modules Apache nécessaires
a2enmod rewrite headers env dir mime ssl

# Créer le VirtualHost HTTPS
cat > /etc/apache2/sites-available/nextcloud.conf << 'EOF'
<VirtualHost *:80>
    ServerName 10.0.0.64
    Redirect permanent / https://10.0.0.64/
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot /var/www/html/nextcloud
    ServerName 10.0.0.64

    SSLEngine on
    SSLCertificateFile    /etc/ssl/certs/nextcloud.crt
    SSLCertificateKeyFile /etc/ssl/private/nextcloud.key

    <Directory /var/www/html/nextcloud/>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/nextcloud_error.log
    CustomLog ${APACHE_LOG_DIR}/nextcloud_access.log combined
</VirtualHost>
EOF

# Activer le site et désactiver le site par défaut
a2ensite nextcloud.conf
a2dissite 000-default.conf
systemctl restart apache2
```

--

### Finaliser l'installation via CLI (occ)

```bash
cd /var/www/html/nextcloud

sudo -u www-data php occ maintenance:install \
    --database="mysql" \
    --database-name="nextcloud" \
    --database-user="ncuser" \
    --database-pass="NcDbPass42!" \
    --admin-user="admin" \
    --admin-pass="Admin123!"
```

```
# → Nextcloud was successfully installed
```

--

### Ajouter l'IP dans les trusted domains

```bash
sudo -u www-data php occ config:system:set trusted_domains 1 \
    --value="10.0.0.64"

# Forcer HTTPS dans la config Nextcloud
sudo -u www-data php occ config:system:set overwrite.cli.url \
    --value="https://10.0.0.64"
sudo -u www-data php occ config:system:set overwriteprotocol \
    --value="https"

# Autoriser les requêtes vers des serveurs locaux (Keycloak sur LAN)
# Nextcloud 28+ bloque par défaut les IPs privées (protection SSRF)
sudo -u www-data php occ config:system:set allow_local_remote_servers \
    --value=true --type=boolean
```

> ✅ Nextcloud est accessible sur `https://10.0.0.64`
>
> ⚠️ Le navigateur affichera un avertissement de certificat auto-signé — cliquer **"Avancer quand même"**.

---

## Étape 3 — Installer et configurer l'app OIDC dans Nextcloud

--

### Installer l'application `user_oidc`

Nextcloud dispose d'une application officielle pour l'authentification OIDC.

```bash
cd /var/www/html/nextcloud

# Installer l'app via occ
sudo -u www-data php occ app:install user_oidc

# Vérifier l'installation
sudo -u www-data php occ app:list | grep user_oidc
# → user_oidc: 6.x.x
```

--

### Activer l'application

```bash
sudo -u www-data php occ app:enable user_oidc
# → user_oidc enabled
```

--

### Configurer le provider OIDC (Keycloak)

```bash
sudo -u www-data php occ user_oidc:provider keycloak \
    --clientid="nextcloud" \
    --clientsecret="NcDeFg9876543210AbC" \
    --discoveryuri="http://10.0.0.60:8080/realms/demo/.well-known/openid-configuration" \
    --unique-uid=0 \
    --check-bearer=0 \
    --mapping-display-name="name" \
    --mapping-email="email" \
    --mapping-groups="groups" \
    --group-provisioning=1
```

| Paramètre | Signification |
|-----------|---------------|
| `--clientid` | Client ID déclaré dans Keycloak |
| `--clientsecret` | Secret récupéré dans Keycloak |
| `--discoveryuri` | URL d'autodiscovery du realm Keycloak |
| `--unique-uid=0` | Utilise le claim `preferred_username` comme UID (pas le `sub`) |
| `--mapping-groups` | Claim JWT contenant les groupes |
| `--group-provisioning=1` | Crée automatiquement les groupes dans Nextcloud |

> ⚠️ En v8.5, le sous-commande `add` n'existe plus — l'identifiant (`keycloak`) est un argument positionnel direct.

--

### Vérifier la configuration OIDC

```bash
# Afficher le provider "keycloak"
sudo -u www-data php occ user_oidc:provider keycloak
```

```
identifier: keycloak
clientid: nextcloud
discoveryuri: http://10.0.0.60:8080/realms/demo/.well-known/openid-configuration
```

> ⚠️ En v8.5, `user_oidc:provider list` n'existe pas — passer l'identifiant directement pour afficher un provider.

--

### Désactiver l'écran de login par mot de passe (optionnel)

Pour forcer l'authentification uniquement via OIDC :

```bash
sudo -u www-data php occ config:app:set user_oidc allow_multiple_user_backends \
    --value="0"
```

> ⚠️ À ne faire qu'**après** avoir validé le SSO — sinon plus d'accès si Keycloak est down.

---

## Étape 4 — Tester l'authentification SSO

--

### Accéder à Nextcloud avec le SSO

Depuis **Win11**, ouvrir un navigateur :

```
https://10.0.0.64
```

1. La page de login Nextcloud affiche un bouton **"Se connecter avec keycloak"**
2. Cliquer sur ce bouton
3. Être redirigé vers la page de login Keycloak (`http://10.0.0.60:8080/...`)
4. Se connecter avec `alice` / `Alice123!`
5. Être redirigé vers Nextcloud, connecté en tant qu'Alice

--

### Vérifier la session Alice dans Nextcloud

Depuis **Win11** (connecté en tant qu'Alice) :

1. Cliquer sur l'avatar en haut à droite → **"Paramètres personnels"**
2. Vérifier :

| Champ | Valeur attendue |
|-------|----------------|
| Nom d'utilisateur | `alice` (ou le `sub` JWT) |
| Nom affiché | `Alice Demo` |
| Email | `alice@demo.local` |
| Backend | `user_oidc` |

--

### Vérifier les groupes dans Nextcloud

```bash
# Depuis nextcloud (10.0.0.64)
sudo -u www-data php occ user:info alice
```

```
user_id: alice
display_name: Alice Demo
email: alice@demo.local
groups:
  - nextcloud-users
```

→ ✅ Alice appartient au groupe `nextcloud-users` (hérité de Keycloak)

--

### Tester la déconnexion SSO

1. Dans Nextcloud, se déconnecter
2. Être redirigé vers la page Keycloak
3. Ouvrir maintenant `http://10.0.0.62:8200/ui` (Vault)
4. Cliquer **"Sign in with OIDC"**
5. Keycloak **ne redemande pas** les credentials (session SSO active)
6. Vault est accessible directement

→ ✅ Le SSO fonctionne entre les deux applications (Vault **et** Nextcloud)

---

## Étape 5 — Vérification end-to-end (multi-applications)

--

### Flux SSO complet — deux applications

```
Alice (navigateur)
    │
    ├─1─► Nextcloud : "Se connecter avec keycloak"
    │
    ├─2─► Keycloak : page de login (première fois)
    │
    ├─3─► Alice saisit ses credentials (une seule fois)
    │
    ├─4─► Keycloak émet un token JWT signé
    │       (claims: sub=alice, groups=[vault-readers, nextcloud-users])
    │
    ├─5─► Nextcloud valide le JWT → Alice connectée
    │       → groupe "nextcloud-users" synchronisé
    │
    └─6─► Plus tard : Alice accède à Vault (http://10.0.0.62:8200)
              → Keycloak réutilise la session existante
              → Pas de nouveau login requis ✅
```

--

### Tableau de vérification

| Test | Résultat attendu |
|------|-----------------|
| Keycloak opérationnel | `curl http://10.0.0.60:8080/realms/demo` → JSON |
| Nextcloud accessible | `curl https://10.0.0.64` → page HTML |
| Discovery OIDC Nextcloud | `occ user_oidc:provider keycloak` → config affichée |
| Login Alice via SSO | Bouton Keycloak → redirection → connectée |
| Groupes synchronisés | `occ user:info alice` → `nextcloud-users` |
| SSO entre apps | Vault accessible sans re-login après Nextcloud |

---

## Dépannage

--

### Nextcloud ne trouve pas le provider OIDC

```bash
# Vérifier que l'URL de découverte est accessible depuis nextcloud
curl -s "http://10.0.0.60:8080/realms/demo/.well-known/openid-configuration" \
    | python3 -m json.tool | grep -E "issuer|authorization"

# Vérifier la config de l'app
sudo -u www-data php occ user_oidc:provider keycloak
```

--

### Erreur "redirect_uri mismatch"

```bash
# Vérifier que l'URI de callback dans Keycloak correspond exactement
# Keycloak → Clients → nextcloud → Valid redirect URIs :
# → https://10.0.0.64/index.php/apps/user_oidc/code
```

> Nextcloud envoie `/index.php/apps/user_oidc/code` (avec `index.php`) dans la requête OIDC.
> La valeur configurée dans Keycloak doit correspondre exactement.

--

### L'utilisateur n'est pas créé après le premier login

```bash
# Vérifier les logs Nextcloud
tail -f /var/www/html/nextcloud/data/nextcloud.log | python3 -m json.tool

# Vérifier que l'app est bien activée
sudo -u www-data php occ app:list | grep user_oidc

# Vérifier les claims du token JWT (adapter le secret)
curl -s -X POST \
  "http://10.0.0.60:8080/realms/demo/protocol/openid-connect/token" \
  -d "client_id=nextcloud" \
  -d "client_secret=NcDeFg9876543210AbC" \
  -d "username=alice" \
  -d "password=Alice123!" \
  -d "grant_type=password" \
  | python3 -c "
import sys,json,base64
d=json.load(sys.stdin)
token=d['access_token']
payload=token.split('.')[1]
payload += '=' * (4 - len(payload) % 4)
print(json.dumps(json.loads(base64.b64decode(payload)), indent=2))
"
```

→ Vérifier la présence de `"email"`, `"name"` et `"groups"` dans le payload

--

### Les groupes ne se synchronisent pas dans Nextcloud

Vérifier la configuration du mapper dans Keycloak :

1. **Clients** → `nextcloud` → **Client scopes** → `nextcloud-dedicated`
2. Vérifier la présence du mapper **Group Membership** avec `Token Claim Name = groups`
3. Vérifier que `Full group path = OFF`

```bash
# Vérifier la configuration du provider (mapping groupes)
sudo -u www-data php occ user_oidc:provider keycloak
# → Vérifier que group-provisioning=1 et mapping-groups=groups
```

---

## Récap' de la pratique

```
Démo 2 (acquis) :
    Keycloak → realm "demo" + client "vault" + Alice + vault-readers
              │
Démo 3 (nouveau) :
    Étape 1 : Client OIDC "nextcloud" ajouté dans Keycloak
              │    + groupe "nextcloud-users" + Alice membre
              │
    Étape 2 : Nextcloud installé (Apache + PHP + MariaDB)
              │
    Étape 3 : App user_oidc configurée → Keycloak comme provider
              │
    Étape 4 : SSO validé → Alice connectée via Keycloak
              │
    Étape 5 : SSO cross-apps → Vault + Nextcloud, un seul login
```

| Concept | Ce qu'on a fait |
|---------|----------------|
| **Multi-SP** | Deux Service Providers (Vault + Nextcloud) sur un seul IdP |
| **Client OIDC** | Un client Keycloak par application |
| **app user_oidc** | Module Nextcloud officiel pour l'auth OIDC |
| **Provisioning JIT** | Utilisateur créé dans Nextcloud au premier login |
| **Group sync** | Groupes Keycloak propagés dans Nextcloud |
| **SSO réel** | Un seul login Keycloak pour accéder aux deux apps |
| **Least privilege** | Groupes distincts par application (`vault-readers` / `nextcloud-users`) |
