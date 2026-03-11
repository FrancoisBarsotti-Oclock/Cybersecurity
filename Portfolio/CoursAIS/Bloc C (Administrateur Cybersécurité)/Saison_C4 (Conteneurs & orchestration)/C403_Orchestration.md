# Session C403. Orchestration

**Notions du jour :**


## Orchestration : Docker Swarm 🎼

_Simplifiez la gestion de vos conteneurs_

### Pourquoi orchestrer ?
On sait lancer des conteneurs avec Docker Compose ...

Mais que se passe-t-il quand on a **plusieurs serveurs ?** 🤔

### Les vrais problèmes

* 💥 Un serveur tombe -> les conteneurs aussi
* 📈 Le trafic explose -> comment scaler ?
* 🔄 On doit mettre à jour -> sans coupure ?
* 🔐 Comment distribuer des secrets entre machines ?

**Docker Swarm** repond à tout ca ! 🎯

### Définition

Docker Swarm est un outil de **clustering et d'orchestration** intégré à Docker.

Il permet de gerer un ensemble de machines hôtes comme **un seul et unique hôte Docker**.

### Les avantages clés

* ⚡ **Haute disponibilité** : les apps restent en ligne même si un serveur tombe
* 🔐 **Sécurité** : secrets stockes et distribues de manière sécurisée
* 📈 **Scalabilité** : réplication et load-balancing automatiques
* 🚀 **Rolling updates** : mises à jour sans interruption
* ↩️ **Rolling back** : avec une seule commande on peut rollback.

## L’architecture du nœud 🏗️

### Deux types de noeuds
* 👑 **Managers** : gerent le cluster, planifient les services Les chefs d'orchestre
* ⚙️ **Workers** : executent les conteneurs Les musiciens

_Un manager peut aussi être worker (par défaut, il l'est !)_

### Le Quorum 👨‍👨‍👦‍👦 🗳️

Le nombre minimum de managers nécessaires pour que le cluster fonctionne.

**Formule** : Quorum = (nombre de managers / 2) + 1

### Recommandations Docker

| **Managers** | **Quorum** | **Pannes tolérées** |
| :--: | :--: | :--: |
| 1 | 1 | 0 ❌ |
| 3 | 2 | 1 ✅ |
| 5 | 3 | 2 ✅ |
| 7 | 4 | 3 ✅ |

Toujours un nombre **impair** de managers !

⚠️ _Avec un nombre pair, on risque un "split-brain" → le cluster se bloque_

### Élection du leader 👑

* Parmi les managers, un **leader** est élu
* C'est lui qui planifie les services
* Si le leader tombe → un nouveau est élu automatiquement

_Algorithme de consensus Raft - on ne rentre pas dans les détails_

## Ports réseau

Avant de commencer, Docker Swarm utilise des ports spécifiques :

* **TCP 2377** : communication manager <> workers
* **TCP/UDP 7946** : découverte entre nœuds
* **TCP/UDP 4789** : trafic de données (overlay network)

⚠️ Ces ports doivent être ouverts sur **tous les nœuds** du cluster !

## Initialisation du docker

### Créer le premier manager
```apache
docker swarm init -- advertise-addr <IP-DU-MANAGER>
```

Gette commande génère un token pour rejoindre le cluster.

