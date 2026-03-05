# 🌐 Session C309. SSO & WAF.
Notions du jour:
* 

## SSO & IAM
_Centraliser la gestion des identités et des accès sur le réseau_

### Le problème de l'identité fragmentée
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

### Vous avez dit RADIUS ?
On a déjà vu une solution d'authentification centralisée : **RADIUS**

Mais RADIUS est **limité au réseau** :

* Wi-Fi entreprise
* VPN
* Commutateurs 802.1X
* Application web, API, SaaS ...

_Il faut une solution qui couvre aussi la couche applicative_

### La solution : SSO + IAM

### Single Sign-On (SSO)
**Une seule authentification pour accéder à toutes vos applications**

1. Vous vous connectez **une fois** auprès d'un fournisseur d'identité
2. Toutes vos applications vous reconnaissent automatiquement
3. Vous ne ressaisissez jamais votre mot de passe

_Analogie : c'est comme avoir une **carte d'accès badgée** qui ouvre toutes les portes du bâtiment_

### Identity & Access Management (LAM)
L'IAM va plus loin que le SSO : c'est la **gestion complète** des identités

* **Qui** peut accéder à quoi ? - Gestion des rôles et permissions
* **Comment** s'authentifier ? - MFA, politique de mot de passe
* **Quand** ? - Accès temporaires, expiration
* **Depuis où** ? - IP, localisation, appareil
* **Audit** - Qui a fait quoi, à quelle heure

_SSO = le mécanisme de connexion / IAM = la gouvernance des accès_

### Les acteurs du SSO

### 🚧 En construction 🚧

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

```
🚧 En construction 🚧
```

_Ajouter un utilisateur dans Keycloak → il accède automatiquement à toutes les apps configurées Désactiver un compte dans Keycloak → accès révoqué partout, immédiatement_

## Les protocoles SSO
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

```
🚧 En construction 🚧
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

### Le protocole OpenlD Connect (OLDC)
· Couche d'identité construite sur OAuth 2.0 (2014)
. Ajoute un ID Token (JWT) qui contient l'identité de l'utilisateur
. Standardise les endpoints (/userinfo, /.well-known/openid-configuration)
. Le standard de facto pour les applications web modernes



```
🚧 En construction 🚧
```
_OpenID Connect = OAuth 2.0 + identite + standardisation_

### Le JWT (JSON Web Token)

Le token que vous allez rencontrer partout avec OIDC :


```
🚧 En construction 🚧
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

Voir 👉 [demo-keycloak](https://github.com/O-clock-Aldebaran/SC03E09-demo-keycloak)

