# Session B305 : Correction Atelier Nagios et NCPA.

### Notions du jour

* Pourquoi NCPA ?

    * Agent multi-plateforme (Windows, Linux, macOS) avec API REST et active checks.
    * Découverte simple des métriques via interface Web locale (https://hote:5693/).
    * Compatibilité Nagios Core/Nagios XI, remonte CPU, RAM, disques, services, processus, réseau, etc.

* Architecture & modes
    * Actif : Nagios appelle l’agent (check_ncpa.py) en HTTP(S) (port 5693).
    * Passif : l’agent envoie vers le serveur (NSCA/NRDP) à intervalle régulier.
    * API : endpoints REST + perfdata (chemins /api), authentification par token.

* Installation rapide
    * Linux (Deb/RPM) : installer le paquet NCPA, démarrer et ouvrir le port 5693/tcp.
    * Windows : exécuter l’installer MSI, définir le token, démarrage auto du service.
    * Après installation : accéder à https://IP:5693/ (certificat auto-signé par défaut).

* Configuration de l’agent (extraits)

                                                        `C:Program Files (x86)NagiosNCPAetc`

* Fichier : //usr//local//ncpa//etc//ncpa.cfg (Linux) ou cpa.cfg                                (Windows).

* Paramètres clés :
* `[api]` → `community_string = VOTRE_TOKEN_FORT`
* `[listener]` → `ip = 0.0.0.0`, `port = 5693`, `ssl = 1`
* `[passive]` (optionnel) → `handlers = nrdp`, `nrdp_url`, `token`, `interval`

* Nagios côté serveur
    * Plugin client : check_ncpa.py (sur le serveur Nagios, dans //usr//local//nagios//libexec).
    * Commande type :



