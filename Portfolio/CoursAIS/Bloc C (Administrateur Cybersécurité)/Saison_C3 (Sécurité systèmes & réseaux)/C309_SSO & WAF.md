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

### Pourquoi un WAF ?
Les firewalls classiques ne suffisent plus

* Un firewall L3/L4 filtre sur **IP, port, protocole**
* Les attaques modernes passent sur le port **443 (HTTPS)** ... autorisé partout
* Injection SQL, XSS, traversée de répertoire : **tout passe en http**

_Le WAF se positionne là où le firewall réseau est aveugle : à la couche applicative_

### Rappel : les attaques web les plus courantes (OWASP Top 10)
| **Rang** | **Catégorie** | **Exemple** |
| --- | --- | --- |
| A01 | Contrôle d'accès défaillant | Accès à /admin sans auth |
| A02 | Échec cryptographique | HTTP en clair, credentials en clair |
| A03 | Injection | `' OR 1=1 --` dans un formulaire |
| A04 | Conception non sécurisée | Absence de validation côté serveur |
| A07 | Authentification défaillante | Brute force, session fixation |
| A09 | Journalisation insuffisante | Attaque non détectée |

_Source : owasp.org 2021– liste mise à jour tous les 4 ans_

[Dernier Top 10 OWASP : 2025](https://owasp.org/Top10/2025/) 

### Ce que ces attaques ont en commun
* Elles transitent toutes par **HTTP/HTTPS**
* Le firewall réseau les laisse **passer** (port 80/443 ouvert)
* L'application backend les **recoit** si rien ne filtre avant

👉 Il faut une **inspection au niveau du contenu http**

### C'est quoi un WAF ? 🧱
Un **Web Application Firewall** est un équipement (physique, logiciel ou cloud) qui :

1. **Intercepte** toutes les requêtes HTTP/HTTPS avant qu'elles arrivent à l'application
2. **Analyse** leur contenu (URL, headers, body, cookies)
3. **Bloque ou journalise** les requêtes malveillantes selon des règles

_Analogie : le WAF est un **videur à l'entrée du club** - il inspecte chaque client avant de le laisser entrer_

### Position du WAF dans l'architecture
```ini
Internet
    │
[Firewall L3/L4]  ← filtre IP/port
    │
  [WAF]           ← filtre le contenu HTTP
    │
[Reverse Proxy]   ← ex: Nginx, HAProxy
    │
[Application]     ← backend (PHP, Python, Java...)
    │
[Base de données]
```

_Le WAF peut être intégré au reverse proxy ou être un équipement dédié_

### WAF vs Firewall réseau
| **Critère** | **Firewall réseau** | **WAF** |
| --- | --- | --- |
| Couche OSI | L3/L4 | L7 |
| Filtre sur | IP, port, protocole | URL, headers, body, cookies |
| Voit le contenu HTTP | ❌ | ✅ |
| Protège contre SQLi/XSS | ❌ | ✅ |
| Protège contre port scan | ✅ | ❌ |
| Complexité | Faible | Élevée |

_Les deux sont complémentaires — le WAF ne remplace pas le firewall réseau_

### Les deux modes de fonctionnement d'un WAF

* **Mode détection** (Détection Only) :
    * La requête **passe**, mais l'alerte est **journalisée**
    * Idéal pour auditer sans bloquer - on observe ce qui se passe ! 
    * ⚠️ Ne protège pas, sert uniquement à ajuster les règles

* **Mode prévention** (Prevention / Block) :
    * La requête suspecte est **bloquée** (réponse 403)
    * Protection active
    * Risque de **faux positifs** (bloquer des requêtes légitimes)

_On commence toujours en mode détection, puis on passe en prévention après ajustement_

### Les règles d'un WAF
Un WAF fonctionne avec des **règles** (ou signatures) :

* Chaque règle décrit un **pattern** à détecter (regex, correspondance de chaîne, score ... )
* Les règles sont regroupées en **rulesets**
* Le plus connu : **l'OWASP ModSecurity Core Rule Set (CRS)**
    * ~200 règles couvrant OWASP Top 10
    * Maintenu activement, open-source
    * Utilisable avec ModSecurity, Nginx, AWS WAF ...

### Types de WAF 📦
Trois grandes familles selon le déploiement :

| **Type** | **Exemples** | **Avantages** | **Inconvénients** |
| --- | --- | --- | --- |
| **Réseau** (appliance) | F5 BIG-IP, Fortiweb | Performances, dédié | Coût élevé |
| **Hôte** (logiciel) | ModSecurity, NAXSI | Gratuit, contrôle total | Charge sur le serveur |
| **Cloud** | Cloudflare, AWS WAF | Facile, scalable | Dépendance externe |

### WAF hôte : le plus accessible
* Installé directement sur le serveur web (Nginx, Apache)
* **ModSecurity** : le WAF open-source de référence
* **NAXSI** : WAF léger spécifique à Nginx (approche whitelist)

_C'est ce que nous allons installer dans la démo_

## ModSecurity 🔧

_Le WAF open-source le plus utilisé au monde_

### Présentation de ModSecurity
* Créé en 2002, maintenu par **OWASP** et la communauté
* Module pour **Apache** et **Nginx** (via libmodsecurity)
* Analyse les requêtes et réponses HTTP selon des règles
* Compatible avec **l'OWASP Core Rule Set (CRS)**
* Version actuelle : **ModSecurity v3** (libmodsecurity3)

### Architecture ModSecurity v3
```ps
Requête HTTP
    │
[Nginx / Apache]
    │
[ModSecurity v3 (libmodsecurity3)]
    │ ↓ analyse
[OWASP CRS (règles)]
    │
Bloquer / Journaliser / Laisser passer
    │
[Application backend]
```
_v3 = connecteurséparé du moteur → plus modulaire que v2_

### Installation de ModSecurity sur Nginx (Debian/Ubuntu)

```py
# Installer les dépendances
sudo apt update
sudo apt install -y nginx libnginx-mod-http-modsecurity

# Activer le module dans Nginx
sudo sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' \
    /etc/modsecurity/modsecurity.conf-recommended
sudo cp /etc/modsecurity/modsecurity.conf-recommended \
    /etc/modsecurity/modsecurity.conf
```

### Activer ModSecurity dans la config Nginx

```apache
# Dans /etc/nginx/sites-available/default (ou votre vhost)
server {
    listen 80;
    server_name mon-site.com;
    
    # Activer ModSecurity
    modsecurity on;
    modsecurity_rules_file /etc/nginx/modsecurity/main.conf;
    
    location / {
        proxy_pass http://localhost:8080;
    }
}
```

```apache
# Créer le répertoire de règles
sudo mkdir -p /etc/nginx/modsecurity
sudo cp /etc/modsecurity/modsecurity.conf /etc/nginx/modsecurity/
```

### Installation de ModSecurity sur Apache

```ps
# Installer le module Apache
sudo apt install -y libapache2-mod-security2

# Activer le module
sudo a2enmod security2

# Copier la config de base
sudo cp /etc/modsecurity/modsecurity.conf-recommended \
    /etc/modsecurity/modsecurity.conf
```

### Activer ModSecurity dans Apache
```apache
# Dans /etc/apache2/mods-enabled/security2.conf
<IfModule security2_module>
    # Inclure la config principale
    Include /etc/modsecurity/modsecurity.conf

    # Inclure les règles OWASP CRS (une fois installé)
    Include /etc/modsecurity/crs/crs-setup.conf
    Include /etc/modsecurity/crs/rules/*.conf
</IfModule>
```

```apache
sudo systemctl restart apache2
```

### Configuration de base : modsecurity.conf

Les paramètres essentiels à connaître :
```swift
# Mode : DetectionOnly ou On (blocage)
SecRuleEngine On

# Taille max du body analysé (eviter les gros uploads)
SecRequestBodyLimit 13107200
SecRequestBodyNoFilesLimit 131072

# Journaliser les alertes
SecAuditLog /var/log/modsecurity/audit.log
SecAuditLogParts ABCIJDEFHKZ

# Format des logs
SecAuditLogType Serial
```
### Installer l'OWASP Core Rule Set (CRS)
```ps
# Télécharger le CRS depuis GitHub
cd /tmp
wget https://github.com/coreruleset/coreruleset/archive/v4.0.0.tar.gz
tar -xzf v4.0.0.tar.gz

# Installer dans le bon répertoire
sudo mv coreruleset-4.0.0 /etc/modsecurity/crs

# Copier la config par défaut
sudo cp /etc/modsecurity/crs/crs-setup.conf.example \
/etc/modsecurity/crs/crs-setup.conf
```

### Structure du CRS
```swift
/etc/modsecurity/crs/
├── crs-setup.conf ← configuration globale du CRS
└── rules/
    ├── REQUEST-901-... ← initialisation
    ├── REQUEST-920-... ← anomalies de protocole HTTP
    ├── REQUEST-930-... ← attaques sur les fichiers locaux (LFI)
    ├── REQUEST-931-... ← inclusion de fichiers distants (RFI)
    ├── REQUEST-932-... ← injection de commandes OS
    ├── REQUEST-933-... ← injections PHP
    ├── REQUEST-941-... ← attaques XSS
    ├── REQUEST-942-... ← injections SQL
    ├── REQUEST-943-... ← session fixation
    └── RESPONSE-950-... ← fuites de données dans les réponses
```
### Le système de score du CRS
Le CRS utilise un **système de score par anomalie** :
* Chaque règle qui matche **ajoute des points** à la requête
* Si le score dépasse un seuil → requête bloquée
* Seuil par défaut : **5 points** pour les requêtes entrantes
```apache
# Dans crs-setup.conf
SecAction \
    "id:900110,\
    phase:1,\
    nolog,\
    pass,\
    t:none,\
    setvar:tx.inbound_anomaly_score_threshold=5"
```
_Avantage : une seule signature incertaine ne bloque pas, il faut plusieurs indices_

### Tester ModSecurity avec une requête malveillante
```apache
# Test d'une injection SQL basique
curl "http://localhost/?id=1' OR '1'='1"

# → Réponse attendue en mode blocage : HTTP 403 Forbidden

# Vérifier les logs
sudo tail -f /var/log/modsecurity/audit.log

# Tester XSS
curl "http://localhost/?q=<script>alert(1)</script>"
```

### Lire les logs ModSecurity
```swift
--a1b2c3d4-A--
[27/Feb/2026:10:15:33] 192.168.1.100 "GET /?id=1'+OR+'1'='1"
--a1b2c3d4-B--
GET /?id=1'+OR+'1'='1 HTTP/1.1
Host: localhost

--a1b2c3d4-F--
HTTP/1.1 403 Forbidden

--a1b2c3d4-H--
Message: Warning. Pattern match "..." at ARGS:id.
[id "942100"] [msg "SQL Injection Attack Detected via libinjection"]
[severity "CRITICAL"] [tag "OWASP_CRS"]
```
* **id** : identifiant de la règle déclenchée
* **msg** : description de l'attaque détectée
* **severity** : niveau de criticité

### Gérer les faux positifs

Un faux positif = une requête légitime bloquée par erreur
```apache
# Désactiver une règle pour un chemin précis
SecRule REQUEST_URI "@beginsWith /api/upload" \
    "id:1001,phase:1,pass,nolog,\
    ctl:ruleRemoveById=200002"

# Désactiver une règle globalement (à éviter)
SecRuleRemoveById 942100

# Exclure un paramètre d'une règle
SecRuleUpdateTargetById 942100 "!ARGS:search"
```

_Lesfaux positifssont normaux au début — c'est pour ça qu'on commence en DetectionOnly_

## Cloudflare ☁️
_Bien plus qu'un simple WAF_

### Qu'est-ce que Cloudflare ?

* Fondé en 2009, l'un des plus grands réseaux mondiaux
* **+330 PoP** (Points of Presence) dans 100+ pays
* Traite ~20% du trafic web mondial
* Services : CDN, WAF, DDoS, DNS, Zero Trust, tunnels ...
* Plan **gratuit** disponible (avec limitations)

_Cloudflare s'est positionné comme une infrastructure invisible mais essentielle du web_

### Comment Cloudflare fonctionne
```apache
Visiteur (Paris)
    │
[Cloudflare PoP Paris] ← le trafic s'arrête ici
    │ ↓
┌────────────────────┐
│ • Anti-DDoS        │
│ • WAF              │
│ • CDN (cache)      │
│ • Bot Management   │
└────────────────────┘
    │ ↓ seulement si OK
[Votre serveur d'origine]
```

_Votre serveur ne voit jamaisle trafic malveillant — Cloudflare l'absorbe_

### Activer Cloudflare sur un domaine

1. Créer un compte sur cloudflare. com
2. Ajouter votre domaine
3. Cloudflare scanne vos DNS existants
4. **Changer les nameservers** chez votre registrar vers ceux de Cloudflare
5. Une fois actif, tout le trafic passe par Cloudflare
```swift
Avant : visiteur → DNS → votre IP → votre serveur
Après : visiteur → DNS Cloudflare → PoP Cloudflare → votre serveur
```

_Votre IP réelle est masquée derrière les IPs Cloudflare_

### Service 1 : CDN (Content Delivery Network) 🌍

* **Mise en cache** des contenus statiques (images, CSS, JS) sur les PoP
* Le visiteur reçoit le contenu depuis le PoP **le plus proche**
* Réduit la latence et la charge sur votre serveur
```apache
Visiteur à Tokyo → PoP Tokyo (cache) → réponse en 5ms
Sans CDN → Serveur à Paris → réponse en 200ms
```

### Configurer le cache Cloudflare

Dans l'interface Cloudflare > **Caching** :

* **Caching Level** : Standard, No Query String, Ignore Query String
* **TTL du cache navigateur** : durée de cache côté client
* **Page Rules** : règles spécifiques par URL
```apache
# Forcer le cache sur les assets statiques
URL pattern : example.com/static/*
Cache Level : Cache Everything
Edge Cache TTL : 1 month
```

_Le cache statique est gratuit et illimité sur tousles plans_

### Service 2 : Protection DDoS 🛡️

* Protection **volumétrique** automatique (jusqu'à plusieurs Tbps)
* Détection et mitigation en **moins de 3 secondes**
* S'adapte automatiquement sans intervention manuelle

#### Plans :

* **Gratuit** : protection DDoS L3/L4 basique
* **Pro** : protection applicative (L7)
* **Business/Enterprise** : protection avancée avec SLA

### Service 3 : WAF Cloudflare 🔍

* WAF managé basé sur des règles OWASP et Cloudflare
* ~100 règles dans le plan gratuit
* **Firewall Rules** : règles personnalisées (IP, pays, user-agent, score bot...)
```apache
# Exemple de règle Cloudflare (firewall rule)
Bloquer tous les accès depuis la Corée du Nord
AND dont le chemin commence par /admin

→ Action : Block
```

_Plussimple à configurer que ModSecurity — mais moins de contrôle_

### Service 4 : DNS Cloudflare

* DNS autoritaire **ultra-rapide** (résolution en ~11ms)
* DNS récursif public : 1.1.1.1 et 1.0.0.1
    * Respectueux de la vie privée (no-log certifié)
    * Le DNS résolveur le plus rapide au monde (Cloudflare)
* **DNSSEC** disponible en un clic

### Service 5 : Zero Trust & Tunnels 🔐

* **Cloudflare Tunnel** (cloudflared) :
    * Expose un service local sur internet **sans ouvrir de port**
    * Connexion sortante chiffrée → le serveur initie la connexion
    * Idéal pour exposer un service derrière un NAT ou un firewall strict
```apache
# Installer et créer un tunnel
curl -L --output cloudflared.deb \
    https://github.com/cloudflare/.../cloudflared.deb
sudo dpkg -i cloudflared.deb
cloudflared tunnel login
cloudflared tunnel create mon-tunnel
cloudflared tunnel route dns mon-tunnel app.exemple.com
cloudflared tunnel run mon-tunnel
```

### Service 6 : Cloudflare Access (Zero Trust) 🧩

* **ZTNA** : Zero Trust Network Access
* Remplace les VPN traditionnels
* Authentification via Google, GitHub, SAML, OTP...
* Chaque accès est vérifié individuellement (pas de réseau de confiance)
```swift
Employé → Cloudflare Access → vérification identité → ressource interne
                            ↑
                    (Google SSO, MFA, device check)
```

### Cloudflare : récapitulatif des services
| **Service** | **Plan gratuit** | **Utilité principale** |
| --- | --- | --- |
| CDN | ✅illimité | Accélération, cache |
| DNS | ✅ | Résolution rapide, sécurisée |
| Anti-DDoS L3/L4 | ✅ | Protection volumétrique |
| WAF basique | ✅limité | Filtrage OWASP |
| WAF avancé | Pro (20$/mois) | Règles personnalisées |
| DDoS L7 | Pro | Protection applicative |
| Zero Trust | ✅(50 users) | Accès sans VPN |
| Tunnel | ✅ | Exposition sans port |

### Limites et considérations de Cloudflare

#### Avantages :

* Très simple à mettre en place
* Efficace dès le plan gratuit
* Absorbe les attaques sans impact sur votre serveur

#### Inconvénients :

* **Tout le trafic passe par Cloudflare** - dépendance à un tiers
* Cloudflare peut voir le trafic HTTPS en clair (terminaison TLS)
* Si Cloudflare tombe, votre site aussi (si l'IP est masquée)
* Certains pays bloquent les IPs Cloudflare

_Décision architecturale importante : confiance dans un tiers vs contrôle total_

## Autres solutions WAF 🔍

_Le marché est large selon le contexte_

### AWS WAF

* WAF managé intégré à l'écosystème AWS
* Fonctionne avec **CloudFront, ALB, API Gateway**
* Règles préconfigurées (OWASP, bots, IPs malveillantes)
* Facturation à la requête (pas d'abonnement fixe)
```rust
Cas d'usage idéal : infrastructure hébergée sur AWS
Avantage : intégration native avec les services AWS
Inconvénient : coût variable, lock-in AWS
```

### Azure WAF

* Intégré à **Azure Application Gateway** et **Azure Front Door**
* Basé sur l'OWASP CRS
* Détection de bots, rate limiting, IP filtering

```rust
Cas d'usage idéal : infrastructure hébergée sur Azure
Avantage : intégration native avec l'écosystème Microsoft
```

### Imperva (ex-Incapsula)

* Solution WAF **enterprise** cloud + appliance
* Très utilisée dans les grandes entreprises
* Analyse comportementale, protection API
* Coût élevé, mais fonctionnalités avancées

### NAXSI (WAF Nginx)

* Module Nginx open-source (approche **whitelist**)
* Plus léger que ModSecurity
* Logique inversée : tout est bloqué, sauf ce qui est autorisé
* Moins de faux positifs... mais configuration plus longue

```apache
# NAXSI : exemple de règle de base
MainRule "str:select" "msg:select keyword" "mz:ARGS" "s:$SQL:4" id:1000;
```

### Tableau comparatif
| **Solution** | **Type** | **Coût** | **Facilité** | **Flexibilité** | 
| --- | --- | --- | --- | --- | 
| **ModSecurity** | Hôte (open-source) | Gratuit | Moyenne | Très haute | 
| **NAXSI** | Hôte (open-source) | Gratuit | Difficile | Haute | 
| **Cloudflare** | Cloud | Gratuit → Pro | Très facile | Moyenne | 
| **AWS WAF** | Cloud | Pay-per-use | Facile | Haute | 
| **Azure WAF** | Cloud | Pay-per-use | Facile | Haute | 
| **Imperva** | Cloud + Appliance | Élevé | Facile | Haute | 
| **F5 BIG-IP** | Appliance | Très élevé | Moyenne | Très haute | 

## Choisir son WAF 🎯

_Les bonnes questions à se poser_

### Critères de choix

1. **Infrastructure** : hébergée où ? (on-premise, cloud, hybride)
2. **Budget** : gratuit, abonnement mensuel, appliance ?
3. **Compétences internes** : équipe sécu disponible ?
4. **Niveau de contrôle** : besoin de règles sur mesure ?
5. **Conformité** : PCI-DSS, RGPD, ISO 27001 imposent parfois des choix

### Scénarios typiques
| **Contexte** | **Solution recommandée** |
| --- | --- |
| Petit site, pas de budget | Cloudflare gratuit |
| VPS auto-hébergé | ModSecurity + OWASP CRS |
| Startup, croissance rapide | Cloudflare Pro |
| AWS, infrastructure cloud | AWS WAF |
| Grande entreprise, exigences élevées | F5, Imperva |
| Besoin de contrôle total | ModSecurity sur-mesure |

## Ce qu'il faut retenir 🧠

* Un WAF opère au **niveau http** là où le firewall réseau est aveugle
* Il fonctionne avec des **règles** (signatures + scores) et deux modes : détection / prévention
* **ModSecurity + OWASP CRS** : la référence open-source, disponible sur Nginx et Apache
* **Cloudflare** : solution cloud tout-en-un (CDN, WAF, DDoS, DNS, Zero Trust)
* Commencer **toujours en mode détection**, ajuster les règles, puis passer en prévention
* Un WAF ne remplace ni le firewall réseau, ni les **bonnes pratiques de développement**

### Les couches de défense complémentaires

```swift
Internet
    │
[Firewall L3/L4] ← bloque les ports, IP malveillantes
    │
[Anti-DDoS / CDN] ← absorbe les attaques volumétriques (Cloudflare)
    │
[WAF] ← filtre SQLi, XSS, traversées de répertoire...
    │
[Reverse Proxy] ← rate limiting, TLS termination
    │
[Application] ← validation des entrées côté serveur
    │
[Base de données] ← requêtes paramétrées, moindre privilège
```

_La sécurité en profondeur : chaque couche protège des menaces différentes_

---

## Pour plus d'information 📚

Voir 👉 [démo-keycloak](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20%26%20R%C3%A9seaux/KeyCloak%20avec%20NextCloud%20%26%20VAULT/ReadMe.md)

Voir 👉 [Démo WAF avec ModSecurity + OWASP CRS](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/S%C3%A9curit%C3%A9%20Syst%C3%A8mes%20&%20R%C3%A9seaux/D%C3%A9mo%20WAF.md)
