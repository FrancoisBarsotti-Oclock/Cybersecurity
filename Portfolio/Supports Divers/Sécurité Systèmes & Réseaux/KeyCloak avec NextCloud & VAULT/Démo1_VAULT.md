# Démo — Keycloak + HashiCorp Vault (SSO / OIDC)

_Mise en pratique : déploiement d'un SSO Keycloak et intégration avec Vault_

---

## Architecture du lab

```
        [Client navigateur]
         Win11 — 10.0.0.10
                │
   ┌────────────┴─────────────────────────────────┐
   │         vmbr2 — LAN A (10.0.0.0/16)          │
   └──┬──────────────┬──────────────┬─────────────┘
      │              │              │
  10.0.0.60      10.0.0.62     10.0.0.63
      │              │              │
  [keycloak]      [vault]       [web-app]
  Keycloak 26    Vault 1.x     App protégée
  port 8080      port 8200     (optionnel)
```

| Machine | Type | IP | Rôle |
|---------|------|----|------|
| keycloak | LXC | 10.0.0.60 | Serveur Keycloak (Identity Provider) |
| vault | LXC | 10.0.0.62 | HashiCorp Vault (gestion des secrets) |
| win11 | VM | 10.0.0.10 | Client navigateur pour les tests |

💡 Keycloak joue le rôle d'**Identity Provider (IdP)** — Vault s'authentifiera via OIDC auprès de Keycloak.

---

## Étape 1 — Installer Keycloak

--

### Prérequis : Java 17

Keycloak nécessite OpenJDK 17 pour fonctionner.

Sur le conteneur **keycloak** (10.0.0.60) :

```bash
apt update && apt install -y openjdk-17-jdk
java -version
# → openjdk version "17.x.x"
```

--

### Télécharger et installer Keycloak

```bash
# Télécharger l'archive Keycloak 26.5.4
wget https://github.com/keycloak/keycloak/releases/download/26.5.4/keycloak-26.5.4.tar.gz

# Extraire l'archive
tar -xvzf keycloak-26.5.4.tar.gz

# Déplacer dans /opt
mv keycloak-26.5.4 /opt/keycloak
```

--

### Créer le compte administrateur initial

```bash
# Créer un utilisateur admin (le mot de passe sera demandé interactivement)
/opt/keycloak/bin/kc.sh build
/opt/keycloak/bin/kc.sh bootstrap-admin user --optimized
```

> ⚠️ À ne faire qu'une seule fois, avant le premier démarrage.
> Notez le login/mot de passe saisi — c'est le compte **admin global**.

--

### Démarrer Keycloak (mode développement)

```bash
# Démarrage en mode dev (HTTP, pas de cert TLS requis)
/opt/keycloak/bin/kc.sh start-dev --http-port=8080
```

| Option | Signification |
|--------|---------------|
| `start-dev` | Mode développement (HTTP, logs verbeux) |
| `--http-port=8080` | Port d'écoute (défaut : 8080) |

> Pour la **production**, utiliser `start` avec TLS configuré.

--

### Démarrer Keycloak en arrière-plan

```bash
# Lancer en arrière-plan et rediriger les logs
nohup /opt/keycloak/bin/kc.sh start-dev --http-port=8080 \
    > /var/log/keycloak.log 2>&1 &

# Vérifier que le processus tourne
ps aux | grep keycloak

# Suivre les logs de démarrage (attendre "Keycloak is running")
tail -f /var/log/keycloak.log
```

--

### Vérifier que Keycloak répond

```bash
curl -s http://10.0.0.60:8080/realms/master | python3 -m json.tool | head -5
# → {"realm": "master", "public_key": "...", ...}
```

→ ✅ Keycloak est démarré et accessible

---

## Étape 2 — Configurer Keycloak : Realm et Client

--

### Accéder à la console d'administration

Depuis **Win11**, ouvrir un navigateur :

```
http://10.0.0.60:8080/admin
```

Se connecter avec le compte admin créé à l'étape précédente.

--

### Créer un nouveau Realm

Un **Realm** est un espace d'identité isolé (équivalent d'un tenant).

1. Cliquer sur le menu déroulant **"master"** en haut à gauche
2. Cliquer sur **"Create realm"**
3. Renseigner :

| Champ | Valeur |
|-------|--------|
| Realm name | `demo` |
| Enabled | ON |

4. Cliquer sur **"Create"**

→ Vous êtes maintenant dans le realm `demo`

--

### Créer un Client OIDC (pour Vault)

Les **Clients** sont les applications qui délèguent l'authentification à Keycloak.

Dans le realm `demo` :

1. **Clients** → **Create client**
2. Onglet **"General Settings"** :

| Champ | Valeur |
|-------|--------|
| Client type | OpenID Connect |
| Client ID | `vault` |
| Name | `vault` |

3. Cliquer **"Next"**

--

### Paramètres du flux d'authentification

Onglet **"Capability config"** :

| Option | Valeur |
|--------|--------|
| Client authentication | ON (confidential) |
| Standard flow | ON |
| Direct access grants | ON (optionnel, pour tests) |

Cliquer **"Next"**

--

### URLs de redirection

Onglet **"Login settings"** :

| Champ | Valeur |
|-------|--------|
| Root URL | `http://10.0.0.62:8200` |
| Valid redirect URIs | `http://10.0.0.62:8200/ui/vault/auth/oidc/oidc/callback` |
| Valid redirect URIs | `http://10.0.0.62:8200/oidc/callback` |
| Web origins | `http://10.0.0.62:8200` |

Cliquer **"Save"**

--

### Récupérer le Client Secret

1. Aller dans **Clients** → `vault` → onglet **"Credentials"**
2. Copier la valeur du **Client secret**

```
# Exemple :
Client ID     : vault
Client Secret : AbCdEf1234567890XyZ
```

> 💡 Ce secret sera utilisé pour configurer l'auth method OIDC dans Vault.

--

### Créer un Mapper pour les groupes (claims JWT)

Pour transmettre les groupes Keycloak dans le token JWT :

1. **Clients** → `vault` → onglet **"Client scopes"**
2. Cliquer sur `vault-dedicated`
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

---

## Étape 3 — Créer des utilisateurs et groupes dans Keycloak

--

### Créer un groupe

Dans le realm `demo` :

1. **Groups** → **"Create group"**
2. Name : `vault-readers`
3. Cliquer **"Create"**

--

### Créer un utilisateur

1. **Users** → **"Create new user"**
2. Renseigner :

| Champ | Valeur |
|-------|--------|
| Username | `alice` |
| Email | `alice@demo.local` |
| First name | `Alice` |
| Last name | `Demo` |
| Email verified | ON |

3. Cliquer **"Create"**

--

### Définir le mot de passe de l'utilisateur

1. Aller dans **Users** → `alice` → onglet **"Credentials"**
2. Cliquer **"Set password"**
3. Renseigner :

| Champ | Valeur |
|-------|--------|
| Password | `Alice123!` |
| Temporary | OFF |

4. Cliquer **"Save"** puis **"Save password"**

--

### Ajouter l'utilisateur au groupe

1. Aller dans **Users** → `alice` → onglet **"Groups"**
2. Cliquer **"Join Group"**
3. Sélectionner `vault-readers`
4. Cliquer **"Join"**

→ ✅ Alice appartient au groupe `vault-readers`

--

### Vérifier le token OIDC (optionnel)

On peut tester la génération d'un token directement via l'API :

```bash
apt install -y curl
curl -s -X POST \
  "http://10.0.0.60:8080/realms/demo/protocol/openid-connect/token" \
  -d "client_id=vault" \
  -d "client_secret=AbCdEf1234567890XyZ" \
  -d "username=alice" \
  -d "password=Alice123!" \
  -d "grant_type=password" | python3 -m json.tool
```

→ Repérer le champ `access_token` et le décoder sur [jwt.io](https://jwt.io)

→ Vérifier que le claim `groups` contient `vault-readers`

---

## Étape 4 — Installer HashiCorp Vault

--

### Ajouter le dépôt officiel HashiCorp

Sur le conteneur **vault** (10.0.0.62) :

```bash
# Importer la clé GPG HashiCorp
apt install -y gpg curl
wget -O - https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

# Ajouter le dépôt APT
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    tee /etc/apt/sources.list.d/hashicorp.list

# Installer Vault
apt update && apt install -y vault

# Vérifier la version
vault version
# → Vault v1.x.x
```

--

### Démarrer Vault en mode développement

```bash
# Démarrer Vault sur toutes les interfaces, port 8200
vault server -dev -dev-listen-address="0.0.0.0:8200"
```

> ⚠️ Le mode `-dev` est **uniquement pour le lab** :
> - Stockage en mémoire (les données sont perdues à l'arrêt)
> - TLS désactivé
> - Root token affiché dans les logs

--

### Récupérer et exporter le Root Token

Au démarrage, Vault affiche :

```
Root Token: hvs.XXXXXXXXXXXXXXXXXXXXXXXX
```

Dans un autre terminal sur **vault** :

```bash
# Exporter les variables d'environnement
export VAULT_ADDR="http://127.0.0.1:8200"
export VAULT_TOKEN="hvs.XXXXXXXXXXXXXXXXXXXXXXXX"

# Vérifier que Vault est accessible
vault status
```

| Champ | Valeur attendue |
|-------|----------------|
| Initialized | true |
| Sealed | false |
| Version | 1.x.x |

--

### Démarrer Vault en arrière-plan (lab)

```bash
# Dans un screen ou tmux
screen -S vault
vault server -dev -dev-listen-address="0.0.0.0:8200"
# Ctrl+A puis D pour détacher le screen
```

Ou avec nohup :

```bash
nohup vault server -dev -dev-listen-address="0.0.0.0:8200" \
    > /var/log/vault.log 2>&1 &

# Le token se trouve dans les logs :
grep "Root Token" /var/log/vault.log
```

---

## Étape 5 — Configurer l'authentification OIDC dans Vault

--

### Activer la méthode d'authentification OIDC

```bash
export VAULT_ADDR="http://127.0.0.1:8200"
export VAULT_TOKEN="hvs.XXXXXXXXXXXXXXXXXXXXXXXX"

vault auth enable oidc
# → Success! Enabled oidc auth method at: oidc/
```

--

### Configurer l'OIDC avec Keycloak comme provider

```bash
vault write auth/oidc/config \
    oidc_discovery_url="http://10.0.0.60:8080/realms/demo" \
    oidc_client_id="vault" \
    oidc_client_secret="AbCdEf1234567890XyZ" \
    default_role="reader"
```

| Paramètre | Signification |
|-----------|---------------|
| `oidc_discovery_url` | URL du realm Keycloak (expose le `.well-known`) |
| `oidc_client_id` | Client ID déclaré dans Keycloak |
| `oidc_client_secret` | Secret récupéré dans Keycloak |
| `default_role` | Rôle Vault appliqué par défaut |

--

### Vérifier la découverte OIDC

```bash
# Keycloak expose automatiquement ses métadonnées OIDC
curl -s "http://10.0.0.60:8080/realms/demo/.well-known/openid-configuration" \
    | python3 -m json.tool | grep -E "issuer|authorization|token|jwks"
```

→ Vault utilise cette URL pour valider les tokens JWT émis par Keycloak.

--

### Créer un rôle Vault lié aux groupes Keycloak

```bash
vault write auth/oidc/role/reader \
    bound_audiences="vault" \
    allowed_redirect_uris="http://10.0.0.62:8200/ui/vault/auth/oidc/oidc/callback" \
    allowed_redirect_uris="http://10.0.0.62:8200/oidc/callback" \
    user_claim="sub" \
    groups_claim="groups" \
    token_policies="reader-policy" \
    token_ttl="1h"
```

| Paramètre | Signification |
|-----------|---------------|
| `bound_audiences` | Le token doit être destiné au client `vault` |
| `user_claim` | Champ du JWT utilisé comme identifiant user |
| `groups_claim` | Champ du JWT contenant les groupes |
| `token_policies` | Politique Vault appliquée après login |
| `token_ttl` | Durée de vie du token Vault |

--

### Créer une politique Vault (reader-policy)

```bash
vault policy write reader-policy - << 'EOF'
# Autoriser la lecture de secrets dans le chemin demo/
path "secret/data/demo/*" {
  capabilities = ["read", "list"]
}

# Autoriser la liste à la racine
path "secret/metadata/demo/*" {
  capabilities = ["list"]
}
EOF
```

---

## Étape 6 — Créer des secrets dans Vault

--

### Écrire un secret de test

```bash
# Activer le moteur de secrets KV v2
vault secrets enable -path=secret kv-v2

# Créer un secret
vault kv put secret/demo/config \
    db_host="10.0.0.100" \
    db_user="appuser" \
    db_password="SuperSecret42!"

# Vérifier
vault kv get secret/demo/config
```

```
====== Secret Path ======
secret/data/demo/config

======= Data =======
Key           Value
---           -----
db_host       10.0.0.100
db_user       appuser
db_password   SuperSecret42!
```

--

### Mapper le groupe Keycloak → politique Vault

```bash
# Créer un groupe externe dans Vault
vault write identity/group \
    name="vault-readers-group" \
    type="external" \
    policies="reader-policy"

# Récupérer l'ID du groupe
vault read identity/group/name/vault-readers-group
GROUP_ID=$(vault read -field=id identity/group/name/vault-readers-group)

# Récupérer l'accessor de la méthode OIDC
OIDC_ACCESSOR=$(vault auth list -format=json | \
    python3 -c "import sys,json; d=json.load(sys.stdin); \
    print(d['oidc/']['accessor'])")

# Créer l'alias du groupe (lien entre groupe Keycloak et groupe Vault)
vault write identity/group-alias \
    name="vault-readers" \
    mount_accessor="${OIDC_ACCESSOR}" \
    canonical_id="${GROUP_ID}"
```

→ Les membres du groupe `vault-readers` dans Keycloak auront la politique `reader-policy` dans Vault.

---

## Étape 7 — Tester l'authentification OIDC

--

### Via l'interface Web de Vault

Depuis **Win11**, ouvrir un navigateur :

```
http://10.0.0.62:8200/ui
```

1. Sélectionner la méthode d'authentification **OIDC**
2. Cliquer sur **"Sign in with OIDC Provider"**
3. Être redirigé vers la page de login Keycloak
4. Se connecter avec `alice` / `Alice123!`
5. Être redirigé vers Vault avec un token valide

--

### Via la CLI Vault

```bash
# Depuis la machine vault ou tout client avec vault installé
vault login -method=oidc -path=oidc role=reader
```

→ Un lien s'ouvre dans le navigateur → se connecter avec Keycloak → le token est injecté dans la CLI

--

### Tester l'accès au secret après login

```bash
# Après authentification OIDC (token stocké dans ~/.vault-token)
vault kv get secret/demo/config
```

→ ✅ Alice peut lire le secret

```bash
# Tenter d'écrire → doit échouer (politique reader uniquement)
vault kv put secret/demo/config db_host="attacker"
# → Error: 1 error occurred: * permission denied
```

→ ✅ L'accès en écriture est bien refusé

--

### Vérifier les informations du token

```bash
vault token lookup
```

```
Key                  Value
---                  -----
display_name         oidc-alice
policies             [default reader-policy]
ttl                  59m50s
meta                 map[role:reader username:alice]
```

→ On voit que le token est lié à l'utilisateur `alice` et à la politique `reader-policy`

---

## Étape 8 — Vérification end-to-end

--

### Récapitulatif du flux OIDC

```
Alice (navigateur)
    │
    ├─1─► Vault UI : "Login with OIDC"
    │
    ├─2─► Keycloak : page de login
    │
    ├─3─► Alice saisit ses credentials
    │
    ├─4─► Keycloak émet un token JWT signé
    │       (claims: sub=alice, groups=[vault-readers])
    │
    ├─5─► Vault valide le JWT (via JWKS de Keycloak)
    │       → mappe le groupe "vault-readers" → policy "reader-policy"
    │
    └─6─► Vault émet un token interne
              → Alice peut lire secret/demo/*
```

--

### Tableau de vérification

| Test | Résultat attendu |
|------|-----------------|
| Démarrage Keycloak | `curl http://10.0.0.60:8080/realms/master` → JSON |
| Démarrage Vault | `vault status` → `Sealed: false` |
| Discovery OIDC | `curl .../well-known/openid-configuration` → JSON |
| Login Alice via UI | Redirection Keycloak → Vault → token valide |
| Lecture secret | `vault kv get secret/demo/config` → données |
| Écriture secret | `vault kv put ...` → `permission denied` |

---

## Dépannage

--

### Keycloak ne démarre pas

```bash
# Vérifier les logs
tail -f /var/log/keycloak.log

# Vérifier que le port 8080 est libre
ss -tlnp | grep 8080

# Vérifier Java
java -version
```

--

### Vault ne valide pas les tokens Keycloak

```bash
# Vérifier que l'URL de découverte est accessible depuis Vault
curl -s "http://10.0.0.60:8080/realms/demo/.well-known/openid-configuration"

# Vérifier la config OIDC de Vault
vault read auth/oidc/config

# Tester manuellement un token JWT
vault write auth/oidc/oidc/auth_url \
    redirect_uri="http://10.0.0.62:8200/oidc/callback" \
    state="test" nonce="test" role="reader"
```

--

### Les groupes ne sont pas transmis dans le JWT

Vérifier que le mapper `groups` est bien configuré dans Keycloak :

1. **Clients** → `vault` → **Client scopes** → `vault-dedicated`
2. Vérifier la présence du mapper **Group Membership** avec `Token Claim Name = groups`
3. Vérifier que `Full group path = OFF` (sinon le claim contient `/vault-readers` au lieu de `vault-readers`)

```bash
# Décoder un token pour vérifier les claims
curl -s -X POST \
  "http://10.0.0.60:8080/realms/demo/protocol/openid-connect/token" \
  -d "client_id=vault&client_secret=...&username=alice&password=Alice123!&grant_type=password" \
  | python3 -c "
import sys,json,base64
d=json.load(sys.stdin)
token=d['access_token']
payload=token.split('.')[1]
payload += '=' * (4 - len(payload) % 4)
print(json.dumps(json.loads(base64.b64decode(payload)), indent=2))
"
```

→ Vérifier la présence de `"groups": ["vault-readers"]`

--

### Erreur "redirect_uri mismatch"

```bash
# Vérifier que les URIs de redirection dans Vault correspondent exactement
# à celles déclarées dans Keycloak (Client → Valid redirect URIs)

vault read auth/oidc/role/reader
# → allowed_redirect_uris doit correspondre exactement
```

---

## Récap' de la pratique

```
Étape 1 : Keycloak installé (Java + tar.gz)
              │
Étape 2 : Realm "demo" + Client OIDC "vault" configurés
              │
Étape 3 : Utilisateur "alice" + groupe "vault-readers" créés
              │
Étape 4 : Vault installé et démarré (mode dev)
              │
Étape 5 : Auth method OIDC configuré → Keycloak comme provider
              │
Étape 6 : Secrets créés + politiques et groupes mappés
              │
Étape 7 : Login Alice via OIDC → accès au secret validé
```

| Concept | Ce qu'on a fait |
|---------|----------------|
| **Keycloak Realm** | Espace d'identité isolé `demo` |
| **Client OIDC** | Application `vault` déclarée dans Keycloak |
| **Claims JWT** | Groupes transmis dans le token (`groups_claim`) |
| **Vault OIDC auth** | Validation des tokens JWT signés par Keycloak |
| **Group mapping** | Groupe Keycloak → politique Vault automatique |
| **Least privilege** | Politique `reader` : lecture seule sur `secret/demo/*` |
| **Audit trail** | Vault et Keycloak journalisent chaque accès |
