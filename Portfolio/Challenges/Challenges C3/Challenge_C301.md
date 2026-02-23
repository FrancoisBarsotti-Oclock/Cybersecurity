# 🏆 Challenge C301: Segmentation VLAN & Contrôle d'accès (ACL)
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Vous êtes technicien réseau et vous intervenez chez MédiaSud, une PME de 45 collaborateurs spécialisée dans l'édition numérique. Jusqu'ici, l'ensemble du réseau interne fonctionnait sur un unique réseau plat (flat network) : tous les postes, serveurs et équipements partagent le même domaine de broadcast.
>
>Suite à un audit de sécurité, plusieurs problèmes ont été identifiés :
>
>* Un stagiaire a accidentellement accédé au serveur de paie depuis son poste.
>* Des visiteurs connectés au Wi-Fi pouvaient voir les partages réseau internes.
>* Aucune restriction n'existe sur l'accès à l'administration des équipements réseau.
>
>Le responsable IT vous confie la mission de segmenter le réseau par service et de mettre en place des règles de filtrage pour appliquer une politique d'accès claire et justifiable.

👉 [Énoncé complet du challenge](https://github.com/O-clock-Aldebaran/SC03E01-VLAN) 👈

Voir 👉 Cours C301 👈

---

## 🛠️ Environnement technique
| **Élément** | **Spécification** |
| :---: | :---: |
| **Outil de simulation** | Cisco Packet Tracer (version 8.x2.2.0400) |
| **Routeur** | 	1× routeur 2911 pour le routage inter-VLAN |
| **Switch** | 1× switch 2960 (L2)  |
| **Serveurs** | 2× serveurs : 1 serveur RH, 1 serveur Comptabilité |
| **Postes** | 4× PC : 1 Direction, 1 RH, 1 Comptabilité, 1 Visiteur |

## 🌐 Plan d'adressage

On personalise les postes, en suivant le plan d'adressage proposé et la topologie souhaitée.

| **VLAN** | **Nom** | **Réseau** | **Passerelle** | **Poste** | **IP du poste** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **10** | **Direction** | 192.168.10.0/24 | 192.168.10.1 | **Direct** | `192.168.10.2` |
| **20** | **Ressources Humaines** | 192.168.20.0/24 | 192.168.20.1 | **RH** | `192.168.20.2` |
| **30** | **Comptabilité** | 192.168.30.0/24 | 192.168.30.1 | **Compta** | `192.168.30.3` |
| **40** | **Visiteurs** | 192.168.40.0/24 | 192.168.40.1 | **Foreigners** | `192.168.40.4` |
| **99** | **Serveur** | 192.168.99.0/24 | 192.168.99.1 | **SRV-RH** | `192.168.99.10` |
| **99** | **Serveur** | 192.168.99.0/24 | 192.168.99.1 | **SRV-Compta** | `192.168.99.20` |

## 🖥️ Topologie à réaliser sur Packet Tracer:

                    [Routeur Inter-VLAN]
                          │
                    Trunk (802.1Q)
                          │
                     [Switch L2]
                    ┌──┬──┬──┬──┐
                    │  │  │  │  │
                  DIR RH CPT VIS SRV

### 1. Câblage

Distribution de câbles Copper Straight-Through 

![01-Câblage](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_01-C%C3%A2blage.png)

### 2. Configuration des VLAN

Sur le switch, on commence par nommer les VLAN 

![02-ConfVLAN](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_02-ConfVLAN.png)

Puis, on configure leur premier accèss

![03-AccèsVLAN](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_03-Acc%C3%A8sVLAN.png)

Et on établie le mode trunk pour tous les VLAN

![04-SwitchTrunk](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_04-SwitchTrunk.png)

Même si à ce stade, il n'est pas encore possible d'avoir une communication entre VLAN, on peut vérifier que chaque interface soit up/up (avec la commande `show ip interface brief`) et relire la configuration globale (avec `show running-config`).

Pour établir la connexion entre VLAN, il nous faut:

* configurer le routeur (le mettre en up et faire des sous-interfaces, sans oublier le `ip routing` à la fin de la configuration).

![05-RouteurSous-interfaces](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_05-RouteurSous-interfaces.png)

* Activer le Trunk sur le switch

![06-ActiverTrunk](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_06-ActiverTrunk.png)

* Permettre le trunk sur les VLAN

![07-AllowTrunk](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_07-AllowTrunk.png)

* S'assurer que toutes les Passerelles soient mises à jour, en accord avec l'ip du routeur (dans ce cas, il va finir toujour par `.254`)

![08-Passerelle](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_08-Passerelle.png)

Et à partir de là, nos machines pourront communiquer même si dans des VLAN différentes

![09-Ping-OK](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C3/images%20C3/Challenge%20C301_09-Ping-OK.png)

### 3. ACL standard (restriction de l'administration)

On créé l'ACL standard pour le réseau de la Direction et on l'active en l'appliquant sur les lignes vty 0 4 du routeur

![10-ACLstandard]

**Note 1**: La liste APL standard peut être numérotée de **1 à 99** et de **1300 à 1999**.

**Note 2**: Dans Cisco Packet Tracer, la commande `line vty 0 4` désigne les lignes Virtual Teletype (VTY), utilisées pour les connexions distantes (Telnet ou SSH) sur un routeur ou un switch Cisco.

👉 Concrètement

* VTY = accès distant via le réseau
* 0 4 = lignes numérotées de 0 à 4
* Donc : 5 sessions simultanées maximum (0,1,2,3,4)

Et une petite vérification de la configuration sera toujours la bienvenue

![11-VérifACLstandard]

### 4. ACL étendues (filtrage inter-VLAN)

Pour filtrer le flux entre les VLANs il nous faut l'application d'une ACL étendue: Nous cherchons à ce que le flux ait la configuration suivante:

| **#** | **Test** | **Résultat attendu** |
| :---: | :---:| :---: |
| 1 | Ping de Direction vers SRV-RH | ✅ Succès |
| 2 | Ping de Direction vers SRV-COMPTA | ✅ Succès |
| 3 | Ping de RH vers SRV-RH | ✅ Succès |
| 4 | Ping de RH vers SRV-COMPTA | 	❌ Échec |
| 5 | Ping de Comptabilité vers SRV-COMPTA | ✅ Succès |
| 6 | Ping de Comptabilité vers SRV-RH | ❌ Échec |
| 7 | Ping de Visiteur vers n'importe quel serveur | ❌ Échec |
| 8 | Ping de Visiteur vers la passerelle Internet (simulée) | ✅ Succès |
| 9 | 	Telnet/SSH vers le routeur depuis Direction | ✅ Succès |
| 10 | 	Telnet/SSH vers le routeur depuis RH ou Visiteur | ❌ Échec |

**Note**: Une liste ACL étendue peut être numérotée de **100 à 199** et de **200 à 2699**.

![12-ACLétenduePings]

Et pour finir, on gère l'accès Telnet/SSH vers le routeur. Puis, pour protéger l'accès au routeur lui-même, on appliquera plutôt l'ACL sur les lignes VTY

![13-SSH&Telnet]

### 5. Tests de validation



### 📚 Ressources:

* [Listes de contrôle d'accès (ACL) avec Cisco](https://www.it-connect.fr/les-listes-de-controle-dacces-acl-avec-cisco/)

* [Cisco - Configure IP Access Lists](https://www.cisco.com/c/en/us/support/docs/security/ios-firewall/23602-confaccesslists.html)
