# Session B303 : Agent Zabbix.
_« Un outil pour les superviser tous »_

### Notions du jour :

* **Zabbix Agent** :
    * Logiciel installé sur un hôte pour remonter les métriques vers le serveur Zabbix
    * Deux types :
        * **Zabbix Agent (classique)** : léger, très utilisé, fonctionne en push ou pull
        * **Zabbix Agent 2** : plus modulaire, supporte des plugins (MySQL, Docker…)
    * Communique avec le serveur sur le port 10050 (par défaut)
    * Peut fonctionner avec une configuration minimale (/etc/zabbix/zabbix_agentd.conf)
* **Mode actif vs passif** :
    * **Passif** : le serveur interroge l’agent
    * **Actif** : l’agent envoie régulièrement des données au serveur
    * Le choix dépend de l’environnement réseau, de la sécurité et des performances
* **Surveillance avec agent** :
    * CPU, mémoire, disque, état des services
    * Scripts personnalisés via UserParameter
    * Support multiplateforme : Linux, Windows, BSD, etc.


### 🚧 En construction 🚧

Challenge du jour 👉 [Challenge_B303](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/Challenge_B303.md)

---
