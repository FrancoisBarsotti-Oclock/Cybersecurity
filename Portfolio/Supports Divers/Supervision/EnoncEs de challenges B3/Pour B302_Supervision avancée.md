# 🛠️ Challenge 2 – Supervision avancée avec Zabbix

## Contexte professionnel

Suite au succès de votre première mission chez **TechSecure**, votre responsable vous confie une nouvelle tâche stratégique : **étendre la supervision à l’ensemble de l’infrastructure**.

Jusqu’à présent, seul le serveur Zabbix est supervisé. Il est désormais nécessaire d’ajouter les serveurs de production, de mettre en place des **tableaux de bord exploitables** et de **configurer des alertes pertinentes** pour l’équipe opérationnelle.

🎯 **Objectif final** :
Être capable de détecter automatiquement une indisponibilité ou une surcharge serveur et d’en avoir une vision claire via Zabbix.

---

## Objectifs pédagogiques

À l’issue de ce challenge, vous serez capable de :

* Installer et configurer un **agent Zabbix** sur un serveur distant
* Ajouter et superviser un **nouvel hôte** dans Zabbix
* Comprendre la **supervision active et passive**
* Créer un **dashboard personnalisé**
* Configurer des **triggers (alertes)** efficaces
* Comprendre le **cycle de vie d’une alerte Zabbix**

📌 **Notions clés abordées**

* Agent Zabbix
* Templates
* Items & Triggers
* Dashboards
* Seuils d’alerte
* Supervision active / passive

---

## ÉTAPE 1 – Préparation du serveur client

### Objectif

Préparer une nouvelle machine qui sera supervisée par le serveur Zabbix.

### Travail à réaliser

**1.1** – Créez ou utilisez une VM Debian **différente du serveur Zabbix**

* Nom suggéré : `web-server-01` ou `app-server-01`
* 1 Go de RAM minimum
* Connexion réseau fonctionnelle

**1.2** – Vérifiez la connectivité réseau :

```bash
ping adresse_ip_serveur_zabbix
```

**1.3** – Vérifiez l’adresse IP du serveur client :

```bash
ip a
```

**1.4** – Mettez le système à jour :

```bash
sudo apt update
sudo apt upgrade -y
```

**1.5 (Optionnel)** – Installez un service web pour enrichir la supervision :

```bash
sudo apt install apache2 -y
sudo systemctl status apache2
```

✔️ *À ce stade, le serveur client doit être accessible et fonctionnel.*

---

## ÉTAPE 2 – Installation de l’agent Zabbix 

### Objectif

Installer et configurer l’agent Zabbix pour permettre la supervision du serveur client.

### Travail à réaliser

**2.1** – Ajoutez le dépôt Zabbix (serveur client) :

```bash
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.0+debian12_all.deb
sudo apt update
```

**2.2** – Installez l’agent Zabbix 2 :

```bash
sudo apt install zabbix-agent2 -y
```

**2.3** – Configurez l’agent :

```bash
sudo nano /etc/zabbix/zabbix_agent2.conf
```

Modifiez les paramètres suivants :

```ini
Server=adresse_ip_serveur_zabbix
ServerActive=adresse_ip_serveur_zabbix
Hostname=web-server-01
```

⚠️ **IMPORTANT**
Le champ `Hostname` doit correspondre **exactement** (majuscules/minuscules incluses) au nom de l’hôte déclaré dans Zabbix.

ℹ️ Dans ce challenge, l’agent est configuré pour fonctionner en **mode actif et passif**.

**2.4** – Redémarrez l’agent :

```bash
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
sudo systemctl status zabbix-agent2
```

**2.5** – Vérifiez l’écoute sur le port 10050 :

```bash
sudo ss -tlnp | grep 10050
```

---

## ÉTAPE 3 – Ajout de l’hôte dans Zabbix

### Objectif

Déclarer le serveur client dans l’interface Zabbix.

### Travail à réaliser

**3.1** – Connectez-vous à l’interface web Zabbix

**3.2** – Allez dans **Data collection → Hosts**

**3.3** – Cliquez sur **Create host**

**3.4** – Configurez l’hôte :

* **Host name** : `web-server-01`
* **Templates** : `Linux by Zabbix agent`
* **Groups** : `Serveurs Web` (ou équivalent)
* **Interfaces** :

  * Type : Agent
  * IP : adresse IP du serveur client
  * Port : `10050`

**3.5** – Cliquez sur **Add**

**3.6** – Vérifiez la disponibilité de l’hôte :

* Icône **ZBX verte** = OK
* Sinon, vérifiez :

  * Configuration de l’agent
  * Firewall
  * Réseau

⏳ *La première remontée de données peut prendre 1 à 2 minutes.*

**3.7** – Allez dans **Monitoring → Latest data** et vérifiez les métriques.

---

## ÉTAPE 4 – Création d’un dashboard

### Objectif

Créer un tableau de bord clair et exploitable.

### Travail à réaliser

**4.1** – Allez dans **Monitoring → Dashboards**

**4.2** – Cliquez sur **Create dashboard**

**4.3** – Nom : `Dashboard TechSecure - Production`

**4.4** – Ajoutez les widgets suivants :

* **CPU**

  * Type : Graph (classic)
  * Item : CPU utilization ou System load

* **Mémoire**

  * Type : Graph (classic)
  * Item : Memory utilization ou Available memory

* **Disponibilité**

  * Type : Host availability

* **Bonus – Problèmes actifs**

  * Type : Problems

**4.5** – Organisez les widgets de manière lisible
**4.6** – Enregistrez le dashboard

---

## ÉTAPE 5 – Configuration des alertes

### Objectif

Créer des triggers pour détecter les incidents.

⚠️ *Note pédagogique* :
Les templates contiennent déjà des triggers. Ici, ils sont créés **manuellement à des fins d’apprentissage**.

### Triggers à créer

**Serveur hors ligne**

* Item : Zabbix agent availability
* Expression : `last() <> 1`
* Sévérité : High / Disaster

**CPU élevé**

* Item : CPU utilization
* Expression : `avg(5m) > 80`
* Sévérité : Warning

**Mémoire saturée**

* Item : Memory utilization
* Expression : `avg(5m) > 90`
* Sévérité : Average

Vérifiez les alertes dans **Monitoring → Problems**.

---

## ÉTAPE 6 – Test des alertes

### Tests

**6.1 – Agent arrêté**

```bash
sudo systemctl stop zabbix-agent2
```

Vérifiez l’apparition de l’alerte, puis redémarrez l’agent :

```bash
sudo systemctl start zabbix-agent2
```

**6.2 – Charge CPU (optionnel)**

```bash
sudo apt install stress -y
stress --cpu 1 --timeout 120s
```

**6.3 – Observations**

* Temps de déclenchement
* Affichage dans Zabbix
* Résolution automatique

---

## ÉTAPE 7 – Synthèse et réflexion

Répondez aux questions suivantes (3–4 lignes max) :

1. **Rôle de l’agent Zabbix**
2. **Templates Zabbix**
3. **Choix des seuils d’alerte**

### BONUS

* Graphiques
* Maps Zabbix
* Templates applicatifs
* Ajout d’un second serveur

---

## Conseils pratiques 💡

* Soyez patient : la supervision n’est pas instantanée
* Vérifiez toujours le hostname
* Logs utiles :
  * `/var/log/zabbix/zabbix_server.log`
  * `/var/log/zabbix/zabbix_agent2.log`
* En production, les communications peuvent être **chiffrées (TLS)**

---