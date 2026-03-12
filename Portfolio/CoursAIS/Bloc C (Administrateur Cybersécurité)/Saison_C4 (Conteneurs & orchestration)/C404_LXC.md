# 🐧Session C403. Linux Containers (LXC) 

**Notions du jour :**

## LXC : Linux Containers 🐧📦

_L’autre façon de conteneuriser_

### Définition 

LXC (Linux Containers) est une technologie de conteneurisation au niveau du **système d'exploitation**.

La ou Docker conteneurise des **applications**, LXC conteneurise des **systèmes complets** 💻

### Analogie Maison 🏠

* 🐋 **Docker** = un studio meuble pour une app Juste ce qu'il faut, rien de plus
* 🐧**LXC** = un appartement complet avec tout le confort Un vrai système avec init,  services, utilisateurs ...
* 🖥️ **VM** = une maison individuelle avec son propre terrain Systeme complet + noyau dédié + matériel virtualisé

## VM vs LXC vs Docker 📊

_Trois niveaux d'isolation_

### Comparatif

|  | **VM** | **LXC** | **Docker** |
| :--: | :--: | :--: | :--: |
| **Noyau** | Dédié | Partagé | Partagé |
| **Init system** | Oui | Oui | NOn |
| **Poids** | Go | ~100 Mo | ~10 Mo |
| **Démarrage** | Minutes | Secondes | Secondes |
| **Isolation** | Forte | Moyenne | Moyenne |
| **Usage** | Multi-OS | Systèmes Linux | Applications |

### Quand utiliser quoi ? 🤔

* **VM** : besoin d'un OS différent (Windows sur Linux) ou isolation forte
* **LXC** : besoin d'un système Linux complet mais léger
* **Docker** : déployer des applications de façon portable

_Ce ne sont pas des concurrents, ce sont des outils complémentaires !_ 🤝



