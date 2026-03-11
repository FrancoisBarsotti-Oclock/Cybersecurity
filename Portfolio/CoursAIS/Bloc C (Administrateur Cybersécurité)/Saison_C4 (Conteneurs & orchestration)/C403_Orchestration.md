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



