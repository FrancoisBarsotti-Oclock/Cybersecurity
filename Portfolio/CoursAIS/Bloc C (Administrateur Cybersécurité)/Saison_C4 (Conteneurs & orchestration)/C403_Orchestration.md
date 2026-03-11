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

## Ports réseau 🔌

Avant de commencer, Docker Swarm utilise des ports spécifiques :

* **TCP 2377** : communication manager <> workers
* **TCP/UDP 7946** : découverte entre nœuds
* **TCP/UDP 4789** : trafic de données (overlay network)

⚠️ Ces ports doivent être ouverts sur **tous les nœuds** du cluster !

## Initialisation du cluster 🚀

### Créer le premier manager
```apache
docker swarm init -- advertise-addr <IP-DU-MANAGER>
```

Gette commande génère un token pour rejoindre le cluster.

### Rejoindre le cluster (worker)

```apache
docker swarm join --token <TOKEN> <IP-DU-MANAGER>:2377
```

### Récupérer les tokens

```apache
# Token pour ajouter un worker
docker swarm join-token worker

# Token pour ajouter un manager
docker swarm join-token manager
```
_Les tokens expirent au bout de 5 minutes — pensez à les regénérer si besoin_

## Démo 1 ️🖥️

### Initialisation d'un cluster Docker Swarm

* On utilise [Killercoda]( https://killercoda.com/)
* Objectif : 1 manager + 1 worker
* Commande utile : docker node ls pour vérifier

### Gestion des nœuds 🔧

### Promotion et rétrogradation
```apache
# Promouvoir un worker en manager
docker node promote <NODE-ID>

# Rétrograder un manager en worker
docker node demote <NODE-ID>
```

_Bonne pratique : toujours demote un manager avant de l'arrêter_

### Pause d'un noeud

Mettre un noeud en pause → **plus de nouvelles tâches**, mais les existantes continuent.
```apache
docker node update -- availability pause <NODE-ID>
docker node update -- availability active <NODE-ID>
```

### Drain d'un noeud

Drainer un noeud → **tout s'arrête** sur ce noeud, les tâches migrent ailleurs.
```apache
docker node update -- availability drain <NODE-ID>
docker node update -- availability active <NODE-ID>
```
⚠️ Différence clé : **pause** = les services existants tournent encore.
**Drain** = tout est migré !

### Labels (étiquettes) 🏷️

Regrouper les noeuds pour contrôler le placement des services :
```apache
# Ajouter un LabeL
docker node update -- label-add env=production <NODE-ID>

# Supprimer un Label
docker node update -- label-rm env <NODE-ID>
```

_Utile pour séparer les nœuds "publics" (web) des nœuds "privés" (BDD)_ 🔒

## Démo 2 🖥️
### Gestion des noeuds

* Créer un nouveau worker et le joindre au cluster
* Le promouvoir en manager
* Le drainer
* Le supprimer du cluster

_Commande utile : docker node Ls pour suivre l'état_

## Les services 🎯
_L’unité de déploiement dans Swarm_

### Conteneur vs Service

* **Conteneur** : une instance unique sur une machine
* **Service** : une abstraction qui peut avoir **plusieurs réplicas** répartis sur le cluster

_Le service, c'est ce qu'on veut. Les conteneurs, c'est ce qui tourne_ 💡

### Avantages des services

* 📈 **Scalabilité** : ajouter/retirer des réplicas à la volée
* ⚡**Haute dispo** : si un replica tombe, Swarm en relance un
* 🌐 **Load-balancing** integre (routing mesh)

### Créer un service
```apache
docker service create -- name web -- replicas 3 \
-- publish 80:80 nginx
```
3 conteneurs Nginx repartis sur le cluster, accessibles sur le port 80 de **n'importe quel nœud** ! 🎉

### Mode global
Un service déployé sur **chaque nœud** du cluster :
```apache
docker service create -- name monitoring -- mode global prometheus
```
_Parfait pour le monitoring, les agents de log, etc._

### Contraintes de placement

Contrôler **où** les services s'exécutent :
```apache
Uniquement sur les workers
docker service create -- name web \
-- constraint node.role == worker nginx

# Uniquement sur Les noeuds Labellisés "production"
docker service create -- name api \
-- constraint node. labels.env == production myapi
```

## Mise à jour et Rollback 🔄

### Rolling update

Mettre à jour un service **sans interruption** :
```apache
docker service update -- image nginx:1.25 \
-- update-delay 10s \
-- update-parallelism 2 \
web
```

* -- update-delay : délai entre chaque batch
* -- update-parallelism : combien de réplicas à la fois

### Rollback

Quelque chose ne va pas ? On revient en arrière :
```apache
docker service update -- rollback web
```

Swarm restaure automatiquement la version précédente 🔙

### Suppression et mise en veille

```apache
# Supprimer un service
docker service rm web

# Ou Le mettre "en veille" (0 réplica)
docker service update -- replicas 0 web
```

_Mettre à O réplicas = garder la config sans consommer de ressources_ 💤

## Démo 3 🖥️
### Déploiement et mise à jour d'un service

* Déployer Nginx 1.18 avec 2 réplicas
* Contraindre sur un worker spécifique (label)
* Mettre à jour vers Nginx 1.25
* Faire un rollback

## Backup & restauration 💾

### Pourquoi c'est important
Le backup Swarm sauvegarde l'**état du cluster**, pas les données des conteneurs !

_Les données (volumes) doivent être sauvegardées séparément_

### Les 4 étapes

1. **Sauvegarder** le dossier /var/lib/docker/swarm/ du leader
2. **Maintenance** : faire les opérations nécessaires
3. **Restaurer** le dossier sauvegardé
4. **Réinitialiser** avec docker swarm init -- force-new-cluster

## Commandes essentielles-Recap 📋

### Cluster
```apache
docker swarm init # Initialiser
docker swarm join # Rejoindre
docker node ls # Lister les noeuds
docker node promote/demote # Promouvoir/rétrograder
```

###  Services
```apache
docker service create # Créer un service
docker service ls # Lister les services
docker service ps <name> # Voir les tâches d'un service
docker service update # Mettre à jour
docker service rm # Supprimer
```

### En résumé 📝

* 🐝 **Swarm** = clustering Docker natif
* 👑 **Managers** planifient, Workers exécutent
* 🎯 **Services** = conteneurs distribués + scalables
* 🔄 **Rolling updates** = mises à jour sans coupure
* 💾 **Backup** du cluster pour la résilience

Swarm est parfait pour un premier pas dans l'orchestration ! 🚀

## 💡 Note Debug et commandes d'intérêt
```apache
# Pour changement de privilèges 

for id in 205 206 207; do pct stop $id && sed -i 's/unprivileged: 1/unprivileged: 0/' /etc/pve/lxc/$id.conf && pct start $id; done

# equivalent de RUN
sudo docker service create nginx

# Si je veux rajouter 10 d’un coup
sudo docker service create –-replicas 10 nginx

# Si je veux le réduire de 10 à 1
sudo docker service update –-replicas 1  nginx

# Si je veux toutes les effacer d’un coup (en gardant une trace du service)
sudo docker service update –-replicas 0 nginx

# Pour voir tout l’historique
sudo docker service ps web

# 1. Sauvegarde
tar -czvf /tmp/swarm-backup.tar.gz /var/lib/docker/swarm/

# 2. (maintenance / simulation panne)

# 3. Restauration
tar -xzvf /tmp/swarm-backup.tar.gz -C /

# 4. Réinitialisation
docker swarm init --force-new-cluster --advertise-addr <IP>
```

--- 

Challenge du jour 👉 [Challenge_C402](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C4/Challenge_C403.md) 👈

#