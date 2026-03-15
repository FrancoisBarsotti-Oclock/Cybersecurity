# Session B305 : Correction Atelier Nagios et NCPA.

### Notions du jour

* **Pourquoi NCPA ?**

    * Agent multi-plateforme (Windows, Linux, macOS) avec API REST et active checks.
    * Découverte simple des métriques via interface Web locale (https://hote:5693/).
    * Compatibilité Nagios Core/Nagios XI, remonte CPU, RAM, disques, services, processus, réseau, etc.

* **Architecture & modes**
    * **Actif** : Nagios appelle l’agent (check_ncpa.py) en HTTP(S) (port 5693).
    * **Passif** : l’agent envoie vers le serveur (NSCA/NRDP) à intervalle régulier.
    * **API** : endpoints REST + perfdata (chemins /api), authentification par token.

* **Installation rapide**
    * **Linux (Deb/RPM)** : installer le paquet NCPA, démarrer et ouvrir le port 5693/tcp.
    * **Windows** : exécuter l’installer MSI, définir le token, démarrage auto du service.
    * Après installation : accéder à https://IP:5693/ (certificat auto-signé par défaut).

* **Configuration de l’agent (extraits)**

                                                        `C:Program Files (x86)NagiosNCPAetc`

* Fichier : //usr//local//ncpa//etc//ncpa.cfg (Linux) ou cpa.cfg                                (Windows).

* Paramètres clés :
* `[api]` → `community_string = VOTRE_TOKEN_FORT`
* `[listener]` → `ip = 0.0.0.0`, `port = 5693`, `ssl = 1`
* `[passive]` (optionnel) → `handlers = nrdp`, `nrdp_url`, `token`, `interval`

* **Nagios côté serveur**
    * Plugin client : check_ncpa.py (sur le serveur Nagios, dans //usr//local//nagios//libexec).
    * Commande type :

    ```nginx
    define command{
        command_name    check_ncpa
        command_line    /usr/bin/python3 $USER1$/check_ncpa.py -H $HOSTADDRESS$ -t $_SERVICETOKEN$ -P 5693 -M $ARG1$ -w $ARG2$ -c $ARG3$
    }
    ```
* Exemples de services :
```nginx
# Charge CPU (1 minute)
define service{
  host_name       srv-app-01
  service_description CPU Load 1m
  check_command   check_ncpa!cpu/percent --aggregate=avg!80!95
}

# Disque C:
define service{
  host_name       win-file-01
  service_description Disk C: Free
  check_command   check_ncpa!disk/logical/C:/percent_free!20!10
}
```

* **Découverte & chemins API utiles**

    * Découverte : `https://IP:5693/api` (navigation arborescente des points).
    * Exemples de chemins :
        * `cpu//percent` (avec `--aggregate=avg`)
        * `memory//virtual//used_percent`
        * `disk//logical//C://used_percent` (Windows) ou `disk//logical//` (Linux root)
        * `processes` + `--name` pour vérifier qu’un processus tourne
        * `services` (Windows) ou `system/uptime`

* **Sécurité & bonnes pratiques**
    * Utiliser un **token fort**, limiter l’accès (pare-feu/IP), activer TLS (par défaut).
    * Sur Internet : placer derrière VPN/Proxy/Tunnel, ou restreindre par ACL réseau.


