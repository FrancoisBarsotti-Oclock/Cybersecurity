# 🏆 Challenge C403: Orchestration.
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Hier soir, vous avez déployé GLPI avec Docker Compose sur une seule machine. C'est bien, mais pas suffisant pour une infrastructure de production.
>
>Votre responsable veut maintenant haute disponibilité : si un serveur tombe, l'application doit continuer à fonctionner. Pour ça, on va passer à Docker Swarm — le mode cluster intégré à Docker — et déployer GLPI en plusieurs replicas gérés via Portainer.
>
>💡 Docker Compose vs Docker Swarm
>
>* Compose → un seul hôte, idéal pour le développement
>* Swarm → plusieurs hôtes, orchestration, haute disponibilité
>* La bonne nouvelle : la syntaxe reste très proche, on réutilise le compose.yaml que vous avez créer lors de votre challenge !

👉 [Énoncé complet du challenge](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Supports%20Divers/Conteneurs%20%26%20Docker/EnoncEs%20de%20challenges/Pour%20C403.md) 👈

Voir 👉 [Cours C402](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C4%20(Conteneurs%20%26%20orchestration)/C403_Orchestration.md) 👈

---

## 🛠️ Environnement technique

| **Élément** | Docker | Docker2 | Docker3 |
| :---: | :---: | :---: | :---: | 
| **OS** | Debian 13.3.0 | = | = |
| **IP statique** | 10.0.0.8 | 10.0.0.12 | 10.0.0.13 | 
| **System** | Qemu Agent | = |  = |  
| **Disque** | 32 GiB | = | = |  
| **CPU** | 2 Sockets + 2 Cores (type host, si en local) | = | = |  
| **RAM** | 2048 MiB | = | = |   

