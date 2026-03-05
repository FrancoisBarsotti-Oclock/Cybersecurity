# 🌐 Session C309. SSO & WAF.
Notions du jour:
* SSO/IAM
* protocoles OIDC/SAML
* keycloak/authentik
* WAF (modsecurity & cloudflare)
* CDN (cloudflare…)

## SSO & IAM 🌐
_Centraliser la gestion des identités et des accès sur le réseau_

### Le problème de l'identité fragmentée 🤔
_Vous gérez 10 applications, chacune avec son propre compte ..._
### Le cauchemar du multi-compte
* Application 1 - mot de passe "Soleil2024!"
* Application 2 - mot de passe "Admin123"
* Application 3 - le même que l'appli 1 (oups
* Application 4 - le Post-it sous le clavier

Et quand un employé quitte l'entreprise ... on pense à désactiver tous ses comptes ?

### Les conséquences concrètes
* **Sécurité** : mots de passe réutilisés, partagés, peu complexes
* **Opérationnel** : onboarding/offboarding multiplié par le nombre d'apps
* **Audit** : impossible de savoir qui a accédé à quoi
* **Conformité** : RGPD, ISO 27001 imposent une traçabilité des accès

La gestion des identités fragmentée est l'un des risques les plus sous-estimés en entreprise.

### On a dit RADIUS ? 🧐
On a déjà vu une solution d'authentification centralisée : **RADIUS**

Mais RADIUS est **limité au réseau** :

* Wi-Fi entreprise ✅
* VPN ✅
* Commutateurs 802.1X ✅
* Application web, API, SaaS ... ❌

_Il faut une solution qui couvre aussi la couche applicative_

### La solution : SSO + IAM 🧩

### Single Sign-On (SSO)
**Une seule authentification pour accéder à toutes vos applications**

1. Vous vous connectez **une fois** auprès d'un fournisseur d'identité
2. Toutes vos applications vous reconnaissent automatiquement
3. Vous ne ressaisissez jamais votre mot de passe

_Analogie : c'est comme avoir une **carte d'accès badgée** qui ouvre toutes les portes du bâtiment_ 🏢

### Identity & Access Management (LAM)
L'IAM va plus loin que le SSO : c'est la **gestion complète** des identités

* **Qui** peut accéder à quoi ? - Gestion des rôles et permissions
* **Comment** s'authentifier ? - MFA, politique de mot de passe
* **Quand** ? - Accès temporaires, expiration
* **Depuis où** ? - IP, localisation, appareil
* **Audit** - Qui a fait quoi, à quelle heure

_SSO = le mécanisme de connexion / IAM = la gouvernance des accès_

## Les acteurs du SSO
### Les trois rôles clés 

```swift
┌─────────────────────────────────────────────────────────┐
│                     UTILISATEUR                         │
│                   (Resource Owner)                      │
└────────────────────┬────────────────────────────────────┘
                     │ veut accéder à
                     ▼
┌─────────────────────────────────────────────────────────┐
│                     APPLICATION                         │
│               (Service Provider / Client)               │
│   ex: GLPI, Nextcloud, Jenkins, votre app interne...    │
└────────────────────┬────────────────────────────────────┘
                     │ redirige vers
                     ▼
┌─────────────────────────────────────────────────────────┐
│             FOURNISSEUR D'IDENTITÉ (IdP)                │
│                (Identity Provider)                      │
│   ex: keycloak, Authentik, Okta, Azure AD...            │
└────────────────────┬────────────────────────────────────┘
```

### Le flux SSO étape par étape
1. L'utilisateur accède à l'application (GLPI, Nextcloud ... )
2. L'application **redirige** vers l'IdP (Keycloak)
3. L'utilisateur **s'authentifie** auprès de Keycloak (login/mdp + MFA)
4. Keycloak **émet un token** (preuve d'identité)
5. L'application **valide le token** auprès de Keycloak
6. L'utilisateur est connecte, sans avoir touche l'application directement

_L'application ne gère jamais le mot de passe - c'est toujours l'IdP qui s'en charge_

### La fédération d'identités

**Un seul IdP pour N applications**

```Apache
             ┌──────────┐
             │ Keycloak │ ← IdP central
             │ (IdP)    │
             └────┬─────┘
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
┌─────────┐  ┌─────────┐   ┌─────────┐
│ GLPI    │  │Nextcloud│   │ Jenkins │
└─────────┘  └─────────┘   └─────────┘
```

_Ajouter un utilisateur dans Keycloak → il accède automatiquement à toutes les apps configurées Désactiver un compte dans Keycloak → accès révoqué partout, immédiatement_

## Les protocoles SSO 🔗
_Les applications et les IdP ont besoin d'un **langage commun** pour communiquer_

### Pourquoi des protocoles standardisés ?
* Interopérabilité : un Keycloak doit parler à un Nextcloud, un Jenkins, un AWS ...
* Sécurité : les mécanismes sont éprouvés et auditables
* Portabilité : changer d'IdP sans réécrire les applications

Les trois grands protocoles :

* **SAML 2.0** - le vétéran (2005), format XML
* **OAuth 2.0** - l'autorisation moderne
* **OpenID Connect** - l'authentification moderne (sur OAuth)

### Une confusion très fréquente
```
"OAuth 2.0, c'est pour l'authentification"
```
### Non !

|  | **OAuth 2.0** | **OpenID Connect** |
| --- | --- | --- |
| **Objectif** | **Autorisation** | **Authentification** |
| Question  | "Que peut faire cette app ?"  | "Qui est cet utilisateur ?" |
| Résultat | Access Token (droits) | ID Token (identité) |
| Format | Token opaque ou JWT | JWT (obligatoirement) |

_OAuth 2.0 dit "cette app peut lire tes emails". OpenID Connect dit "cet utilisateur s'appelle Alice"._

### Keycloak : la référence open-source
* Développé par **Red Hat**, open-source (Apache License)
* Supporte SAML 2.0, OAuth 2.0, OpenID Connect, LDAP, AD
* Interface d'administration web complète
* Gestion fine des rôles, groupes, politiques de mot de passe
* MFA intégré (TOTP, WebAuthn)
* Extensible : thèmes, providers, scripts

_Idéal pour les entreprises de toutes tailles qui veulent le contrôle total_

### Le protocole SAML 2.0
* Standard XML, créé en 2005
* Communique via des **assertions XML** signées
* Très répandu dans les applications d'entreprise historiques
* Flux via redirections HTTP (POST binding)

```html
<!-- Extrait d'une assertion SAML -->
<saml:Assertion>
    <saml:Subject>
        <saml:NameID>alice@entreprise.com</saml:NameID>
    </saml:Subject>
    <saml:AttributeStatement>
        <saml:Attribute Name="role">
            <saml:AttributeValue>admin</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```
_Points forts : très supporté, éprouvé | Points faibles : XML verbeux, intégration complexe_

### Le protocole OAuth 2.0
* Standard moderne (RFC 6749, 2012), basé sur des **tokens**
* Permet à une application d'agir **au nom d'un utilisateur** sans son mot de passe
* Exemple concret : "Se connecter avec Google" - Google autorise l'app à lire votre profil

#### Les 4 rôles OAuth :

1. **Resource Owner** : l'utilisateur
2. **Client** : l'application qui demande l'accès
3. **Authorization Server** : I'IdP (Keycloak)
4. **Resource Server** : l'API / la ressource protégée

_OAuth seul ne dit pas qui vous êtes - il dit ce que vous pouvez faire_

### Le protocole OpenlD Connect (OIDC)
* **Couche d'identité construite sur OAuth 2.0** (2014)
* Ajoute un **ID Token** (JWT) qui contient l'identité de l'utilisateur
* Standardise les endpoints (/userinfo, /.well-known/openid-configuration)
* Le standard de facto pour les applications web modernes

```yaml
// Contenu d'un ID Token (JWT décodé)
{
    "sub": "alice-123",
    "email": "alice@entreprise.com",
    "name": "Alice Dupont",
    "roles": ["admin", "user"],
    "iss": "https://keycloak.exemple.com/realms/monrealm",
    "exp": 1740000000
}
```
_OpenID Connect = OAuth 2.0 + identite + standardisation_

### Le JWT (JSON Web Token) 🎫

Le token que vous allez rencontrer partout avec OIDC :

```css
eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJhbGljZSIsInJvbGVzIjpbImFkbWluIl19.signature
      │                        │                        │
    Header (algo)       Payload (données)       Signature (RSA)
```

* **Header** : algorithme de signature (RS256, HS256 ... )
* **Payload** : les claims (sub, email, roles, exp ... )
* **Signature** : signée par l'IdP, vérifiable par l'application

_L'application vérifie la signature avec la clé publique de l'IdP - pas besoin d'appeler l'IdP à chaque requête_

### Comparaison des protocoles

| **Protocole** | **Usage** | **Format** | **Complexité** | **Support** |
| --- | --- | --- | --- | --- |
| SAML 2.0 | Auth + Fédération | XML | Élevée | Apps d'entreprise
historiques |
| OAuth 2.0 | **Autorisation** API | Token
(JWT) | Moyenne | APIs, apps mobiles |
| OpenID
Connect | **Authentification** | JWT | Moyenne | Apps web modernes |

_En pratique : Keycloak supporte les trois - choisir selon ce que supporte l'application_

## Choisir son IdP 🧰

### Keycloak : la référence open-source

* Développé par **Red Hat**, open-source (Apache License)
* Supporte SAML 2.0, OAuth 2.0, OpenID Connect, LDAP, AD
* Interface d'administration web complète
* Gestion fine des rôles, groupes, politiques de mot de passe
* MFA intégré (TOTP, WebAuthn)
* Extensible : thèmes, providers, scripts

_Idéal pour les entreprises de toutestailles qui veulent le contrôle total_

### Authentik : l'alternative moderne

* Open-source, Python/Django, interface moderne
* Supporte SAML, OAuth, OIDC, LDAP, RADIUS
* Plus simple à déployer que Keycloak (Docker natif)
* Flows d'authentification très configurables (visuellement)
* Idéal pour les petites équipes ou les projets personnels

_Point de vigilance : gestion des autorisations(rôles/permissions) moins avancée que Keycloak_

### Les IdP cloud/SaaS
| **Solution** | **Éditeur** | **Usage typique** |
| --- | --- | --- |
| **Okta** | Okta | Grandes entreprises, riche en intégrations |
| **Auth0** | Okta | Développeurs, APIs, B2C |
| **Azure AD / Entra ID** | Microsoft | Écosystème Microsoft |
| **Google Identity** | Google | Workspace, apps Google |
| **Ping Identity** | Ping | Enterprise, bancaire |

_Cloud = simplicité etscalabilité, mais dépendance externe et coût_

## Keycloak en pratique 🔧
### Installation (méthode directe)
```ps
# Java 17+ requis
apt install -y openjdk-17-jdk

# Télécharger Keycloak
wget https://github.com/keycloak/keycloak/releases/download/26.5.4/keycloak-26.5
tar -xvzf keycloak-26.5.4.tar.gz
mv keycloak-26.5.4 /opt/keycloak
```

### Installation (méthode Docker — recommandée pour les tests)
```yaml
# docker-compose.yml
services:
    keycloak:
        image: quay.io/keycloak/keycloak:26.5.4
        environment:
            KC_BOOTSTRAP_ADMIN_USERNAME: admin
            KC_BOOTSTRAP_ADMIN_PASSWORD: admin
        command: start-dev
        ports:
            - "8080:8080"
```
```yaml
docker compose up -d
# Accessible sur http://localhost:8080
```

### Premier démarrage (sans Docker)
```apache
# Créer un administrateur bootstrap
/opt/keycloak/bin/kc.sh bootstrap-admin user

# Démarrer en mode développement
/opt/keycloak/bin/kc.sh start-dev

# Pour la production (HTTPS requis)
/opt/keycloak/bin/kc.sh start \
    --hostname=keycloak.exemple.com \
    --https-certificate-file=/etc/ssl/cert.pem \
    --https-certificate-key-file=/etc/ssl/key.pem
```
⚠️ _start-dev désactive HTTPS et le cache distribué → uniquement pour lestests_

### Les concepts clés de Keycloak

**Realm** : un espace isolé (tenant)

* Le realm master est réservé à l'administration de Keycloak
* Créer un realm par environnement ou par organisation
**Client**: une application qui délègue l'auth à Keycloak

**Ex**: un client nextcloud, un client jenkins, un client glpi

**Utilisateur / Groupe / Rôle** :
* Utilisateur = un compte
* Groupe = ensemble d'utilisateurs
* Rôle = une permission ou un niveau d'accès

### Créer un realm et un client
```swift
Interface Keycloak → Create realm → Nom : "entreprise"
→ Clients → Create client
    Client ID : nextcloud
    Client type : OpenID Connect
    Root URL : https://nextcloud.exemple.com

→ Credentials → noter le Client Secret (pour les apps confidentielles)
```
_Chaque application = un client distinct dans Keycloak_

### Les types de clients OIDC
| **Type** | **Usage** | **Secret ?** |
| --- | --- | --- |
| **Public** | Apps mobile, SPA (sans backend) | Non (PKCE obligatoire) |
| **Confidential** | Apps backend (web, API) | Oui (client_secret) |
| **Bearer-only** | APIs pures (vérifient les tokens) | Non |

**PKCE** (Proof Key for Code Exchange) : sécurise le flux sans secret pour les apps publiques

### Gérer les utilisateurs et les rôles
```swift
→ Users → Add user → username: alice, email: alice@exemple.com
→ Credentials → Set password → Temporary: OFF

→ Roles → Create role → "admin", "user", "viewer"

→ Groups → Create group → "IT", "Comptabilité"
→ Group → Role mapping → assigner des rôles au groupe
→ User → Groups → ajouter l'utilisateur au groupe
```
_Meilleure pratique : affecter lesrôles aux groupes, pas directement aux utilisateurs_

### Configurer l'authentification MFA
```sql
→ Authentication → Policies
    OTP Policy : Time-based (TOTP), période 30s, algorithme SHA1

→ Authentication → Flows → Browser
    Ajouter l'étape "OTP Form" après le formulaire de login
    → Required (obligatoire) ou Conditional (si l'utilisateur a configuré un OTP)

→ Users → alice → Credentials → Required Actions
    → Configure OTP (forcé au prochain login)
```

_Keycloak supporte TOTP (Google Authenticator) et WebAuthn (clés FIDO2)_

### Intégration avec un annuaire LDAP / Active Directory
```apache
→ User Federation → Add LDAP provider
    
    Connection URL : ldap://dc.exemple.com:389
    Bind DN : cn=keycloak,ou=services,dc=exemple,dc=com
    Bind Credential : [mot de passe]
    Users DN : ou=users,dc=exemple,dc=com

    Vendor : Active Directory

→ Synchronize All Users
```

_Les utilisateurs AD peuvent alorsse connecter avec leurs credentials AD danstoutesles appsintégrées à Keycloak_

## Intégrer une application à Keycloak 🔌

### Côté application : ce qu'il faut configurer
L'application a besoin de 4 informations :
```swift
Discovery URI : https://keycloak.exemple.com/realms/entreprise
Client ID : nextcloud
Client Secret : [valeur dans Keycloak → Clients → Credentials]
Redirect URI : https://nextcloud.exemple.com/index.php/apps/user_oidc/code
```

_Ces informations permettent à l'app de démarrer le flux OIDC et de valider les tokens_

### Exemple : Nextcloud + Keycloak (OIDC)
```nginx
# Installer le plugin OIDC dans Nextcloud
sudo -u www-data php occ app:install user_oidc
sudo -u www-data php occ user_oidc:provider keycloak \
    --clientid="nextcloud" \
    --clientsecret="HIqpWifodzQhiUt5p7mRj3mhGHFTix0P" \
    --discoveryuri="http://10.0.0.60:8080/realms/demo/.well-known/openid-configu
    --unique-uid=0 \
    --check-bearer=0 \
    --mapping-display-name="name" \
    --mapping-email="email" \
    --mapping-groups="groups" \
    --group-provisioning=1
```

### Exemple : application générique (OpenID Connect)
Le flux standard Authorization Code (avec PKCE) :
```swift
1. App redirige → Keycloak
    GET /realms/entreprise/protocol/openid-connect/auth
    ?client_id=monapp
    &response_type=code
    &redirect_uri=https://monapp.com/callback
    &scope=openid email profile
    &code_challenge=... (PKCE)

2. Keycloak authentifie → retourne un "code"
    GET https://monapp.com/callback?code=abc123

3. App échange le code contre les tokens
    POST /realms/entreprise/protocol/openid-connect/token
        code=abc123&client_secret=...

4. Keycloak retourne : access token + id token + refresh token
```

### Valider un token dans votre application
```py
import jwt
import requests
# Récupérer les clés publiques de Keycloak
jwks_url = "https://keycloak.exemple.com/realms/entreprise/protocol/openid-conne
jwks = requests.get(jwks_url).json()
# Décoder et valider le token
decoded = jwt.decode(
    token,
    jwks,
    algorithms=["RS256"],
    audience="monapp",
    issuer="https://keycloak.exemple.com/realms/entreprise"
)
```

_L'application vérifie la signature avec la clé publique → aucun appel à Keycloak pour chaque requête_

## Bonnes pratiques IAM 🛡️

### Principe du moindre privilège
>Donner à chaque utilisateur et application exactement les droits dont ils ont besoin, rien de plus

* Créer des rôles précis (lecture-seule, éditeur, admin)
* Éviter un rôle admin fourre-tout
* Réviser les droits régulièrement (accès review)

### Gestion du cycle de vie des identités
| **Événement** | **Action dans l'IdP** |
| --- | --- |
| Arrivée employé | Créer compte + assigner groupe |
| Changement de poste | Modifier les groupes/rôles |
| Départ employé | **Désactiver immédiatement** le compte |
| Prestataire temporaire | Compte avec date d'expiration |
| Compte de service | Compte dédié, secret rotatif |

_La désactivation dansl'IdP suffit à révoquer tousles accèssimultanément_

Sécuriser Keycloak en production
```ini
# Variables essentielles pour la production
KC_DB=postgres # BDD externe (pas H2 in-memory)
KC_DB_URL=jdbc:postgresql://...
KC_HOSTNAME=keycloak.exemple.com
KC_HTTPS_CERTIFICATE_FILE=...
KC_PROXY=edge # Si derrière un reverse-proxy

# Politique de session
→ Realm Settings → Sessions
SSO Session Max : 8h (temps max de session)
SSO Session Idle : 30min (inactivité)
```

### Surveiller les événements Keycloak
```apache
→ Realm Settings → Events

Types d'événements à surveiller :
    ✅ LOGIN_ERROR → tentatives de brute force
    ✅ LOGOUT → traçabilité des déconnexions
    ✅ UPDATE_PASSWORD → changements de mot de passe
    ✅ ADMIN_ACTION → modifications de config (audit)

→ Brute Force Detection : activer !
    Max Login Failures : 5
    Wait Increment : 60s
```

## Ce qu'il faut retenir 🧠

### Les points clés
* **SSO** = une authentification pour toutes les apps - expérience utilisateur et
sécurité
* **IAM** = gouvernance complète des identités, rôles, et accès
* **IdP** = le composant central qui authentifie et émet les tokens
* **OAuth 2.0** = framework d'autorisation (pas d'authentification)
* **OpenID Connect** = couche **d'authentification** sur OAuth 2.0
* **JWT** = token auto-contenu et verifiable sans appel a l'IdP
* **Keycloak** = IdP open-source complet : OIDC + SAML + LDAP + MFA

### Architecture cible
```ini
Utilisateur
    │
[Keycloak]       ← IdP central (auth, MFA, rôles)
    │ tokens JWT
┌───┼───────────┬────────────┐
▼   ▼           ▼            ▼
[GLPI] [Nextcloud] [Jenkins] [Votre App]
```
_Désactiver un compte dans Keycloak = perte d'accès à touslesservices en quelques secondes_

Voir 👉 [démo-keycloak](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/KeyCloak%20avec%20NextCloud%20%26%20VAULT/ReadMe.md)

## WAF – Web Application Firewall 🛡️

_Filtrer le trafic HTTP malveillant avant qu'il n'atteigne l'application_







---

## Pour plus d'information 📚

Voir 👉 [démo-keycloak](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/KeyCloak%20avec%20NextCloud%20%26%20VAULT/ReadMe.md)

Voir 👉 [Démo WAF avec ModSecurity + OWASP CRS](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20&%20R%C3%A9seaux/D%C3%A9mo%20WAF.md)
