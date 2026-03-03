# Session C3E07. PAM & fail2ban

Notions du jour
•	

## Le problème sans PAM
Sur Linux, beaucoup de services doivent authentifier un utilisateur : SSH, sudo, login local, su, cron ...

Sans PAM, chaque service aurait sa propre logique, ses propres règles, ses propres bugs

Imaginez devoir configurer les règles de mot de passe séparément dans SSH, sudo, login, su ...

### La solution : un système unique
PAM (**Pluggable Authentication Modules**) permet de brancher des **modules d'authentification** de maniere coherente sur tout le système

_**Analogie** : PAM, c'est un standard de prise électrique - tous les  appareils (services) utilisent le même format_

### Ce que PAM contrôle vraiment

PAM ne fait pas que "valider un mot de passe". Il intervient dans 4 phases distinctes :

### Les 4 types de modules
* **auth** - vérifier l'identité (mot de passe, clé, MFA ... )
* **account** - vérifier si le compte a le droit d'entrer (expiration, groupe, horaires)
* **password** - imposer des règles lors du changement de mot de passe
* **session** - appliquer des règles au démarrage/fin de session (logs, variables, quotas)

_Chaque type répond à une question différente sur l'utilisateur_

#### Exemple terrain : connexion SSH
Un utilisateur se connecte en SSH :

1. auth → PAM vérifie son identité (mot de passe ? clé ?)
2. account → le compte est-il autorisé ? expiré ?
3. session → on applique les limites et on journalise

Même logique pour SSH, sudo ou un login console : c'est la force de PAM 

### Un module = une tâche précise
Un module PAM fait une seule chose :

* pam_unix → vérifier un mot de passe local
* pam_ldap → interroger un annuaire LDAP
* pam_faillock → bloquer après X échecs
* pam_pwquality → imposer une complexité de mot de passe
* pam_limits → quotas et limites de ressources

On chaîne ces modules pour construire une politique de sécurité

### Les modules les plus fréquents
