# 🏆 Challenge B302 (CP7): Supervision avancée avec Zabbix.
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Suite au succès de votre première mission chez **TechSecure**, votre responsable vous confie une nouvelle tâche stratégique : **étendre la supervision à l’ensemble de l’infrastructure**.
>
>Jusqu’à présent, seul le serveur Zabbix est supervisé. Il est désormais nécessaire d’ajouter les serveurs de production, de mettre en place des **tableaux de bord exploitables et de configurer des alertes pertinentes** pour l’équipe opérationnelle.
>
>🎯 **Objectif final** : Être capable de détecter automatiquement une indisponibilité ou une surcharge serveur et d’en avoir une vision claire via Zabbix.
>

👉 [Énoncé complet du challenge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Supervision/EnoncEs%20de%20challenges%20B3/Pour%20B302_Supervision%20avanc%C3%A9e.md)

Voir 👉 [Cours B302](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B3%20(Supervision)/B302_Zabbix%20installation.md)

## Étape 1 (0): Préparation du serveur client

| **Élement** | **OS** | **IP** | **Rôle** |
| :--: | :--: | :--: | :--: |
| **Web-server-01** | Debian 13.1.2 | `10.0.0.2416` | Serveur client |

Dès fois, les pings ne sortent des Debian récentes (12/13), n'ayant pas les droits d'ouvrir un socket RAW, ce qui est nécessaire avec soit **setuid root** soit la capacité **CAP_NET_RAW**. Pour solutionner, il faut:

```nginx
# Vérifier les capabilities
sudo getcap /bin/ping

# si rien n'apparait
sudo setcap cap_net_raw+ep /bin/ping

# Normalement là on pourra ping, sinon il faudra remettre le setuid
sudo chmod u+s /bin/ping
```

### installation d'un service web pour enrichir la suppervision

```nginx
sudo apt install apache2 -y
sudo systemctl status apache2
```
## Étape 2 (6): Installation de l'agent Zabbix

On suit la [Documentation officielle d'installation Agent Zabbix](https://www.zabbix.com/documentation/current/en)

![01-Choix Agent Zabbix](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B302%20(CP7)/B302_CP7_01-Choix%20Agent%20Zabbix.png)

### Configuration de l'agent

avec `sudo nano /etc/zabbix/zabbix_agent.conf` on modifie les paramètres suivants:

```nginx
# Pour rappel: l'ip du serveur zabbix est 10.0.0.100 pour cet exo
Server=adresse_ip_serveur_zabbix
ServerActive=adresse_ip_serveur_zabbix
Hostname=web-server-01
```

⚠️ IMPORTANT Le champ `Hostname` doit correspondre exactement (majuscules/minuscules incluses) au nom de l’hôte déclaré dans Zabbix.

### Redémarrage de l'agent

```nginx
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
sudo systemctl status zabbix-agent2
```
Si cela ne passe pas la première fois, il faudra les relancer

![02-Relance de systemctl](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B302%20(CP7)/B302_CP7_02-Relance%20de%20systemctl.png)

### Vérification de l'écoute sur le port 10050

```nginx
sudo ss -tlnp | grep 10050
```

![03-Écoute sur le port 10050](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20B3/images_B3/images%20B302%20(CP7)/B302_CP7_03-%C3%89coute%20sur%20le%20port%2010050.png)

## Étape 3 (7): Ajout de l’hôte dans Zabbix



### 🚧 En construction 🚧

---