# 🚩🪟 Session A407. DNS & IIS.

Ce cours explore deux rôles fondamentaux de Windows Server : le serveur DNS, pilier de la résolution de noms dans le réseau, et le serveur Web IIS, plateforme d'hébergement d'applications et de sites internet. Leur configuration conjointe est essentielle pour rendre les services accessibles de manière conviviale.

- **DNS (Domain Name System)** :
     - **Rôle** : C'est l'annuaire d'Internet et des réseaux locaux. Il convertit des noms de domaine lisibles par l'humain (ex: `www.exemple.com`) en adresses IP utilisables par les machines.
    - **Gestionnaire DNS** : L'outil d'administration sur Windows Server permet de configurer deux types de zones principales :
        - **Zone de recherche directe** : Associe un nom à une IP (le cas le plus courant). Elle contient des enregistrements de type **A** (IPv4), **AAAA** (IPv6), **CNAME** (Alias), **MX** (Messagerie) ou **TXT** (Infos diverses).
        - **Zone de recherche inversée** : Associe une IP à un nom. Elle utilise des enregistrements de type **PTR** (Pointeur). Utile pour le diagnostic réseau et certaines vérifications de sécurité.

- **IIS (Internet Information Services)** :
     - **Définition** : C'est le serveur Web modulaire et extensible de Microsoft. Il permet d'héberger des sites web (HTML, ASP.NET), des services FTP et des API.
    - **Test rapide** : Après l'installation du rôle, accéder à `http://127.0.0.1` ou `http://localhost` depuis le serveur affiche la page d'accueil par défaut d'IIS, confirmant son bon fonctionnement.
    - **Fonctionnalités clés** :
        - **Sites multiples** : Hébergement de plusieurs sites sur un même serveur grâce aux **bindings** (liaisons).
        - **Sécurité** : Gestion des certificats SSL/TLS pour le HTTPS.
        - **Pools d'applications** : Isolation des processus pour qu'un crash sur un site n'affecte pas les autres.
        - **Exploration de répertoire** : Option (souvent désactivée par sécurité) qui permet d'afficher la liste des fichiers d'un dossier ("Index of...").

- **L'Interaction DNS <-> IIS** :
     - Pour qu'un utilisateur accède à un site hébergé sur IIS via un nom (ex: `intranet.thm.local`), deux configurations sont nécessaires : 1. **Côté DNS** : Créer un enregistrement **A** qui fait pointer le nom `intranet` vers l'adresse IP du serveur IIS. 2. **Côté IIS** : Configurer le **binding** (liaison) du site pour qu'il écoute les requêtes arrivant sur cette IP avec ce nom d'hôte spécifique (ex: port 80, nom d'hôte `intranet.thm.local`).
# Domain Name System (DNS)

On va monter un serveur web pour avoir différents DNS qui puissent créer des intranets
## Zones de recherche inversée
La commande **nslookup** dit « Je cherche le nom de tel adresse » quand on cherche le nom on fait une recherche directe, pour voir l’adresse. Puis, quand on cherche l’adresse ip, on fait une recherche inversée pour voir le nom du serveur.
![[Pasted image 20251208215031.png]]
Comme je n’ai pas encore de zone de recherche inversée dans mon DNS, il ne le trouve pas
![[Pasted image 20251208215057.png]]
La commande « `netstat -an` » nous montre toutes les connexions réseau actives

La commande « `tracert` » permet de voir les routes prises pour arriver vers un serveur.

Le site Whois [https://www.whois.com/whois/](https://www.whois.com/whois/)
### Création d’une Zone de recherche inversée
![[Pasted image 20251208215228.png]]
### Types de zones

L’assistant nous en propose 3, puis « suivant »
![[Pasted image 20251208215251.png]]
On va oublier le troisième choix. Comme nous n’avons pas de réplication, on choisi le deuxième, puis « suivant » :
![[Pasted image 20251208215310.png]]
Nous travaillons avec IPv4 pour le moment, puis « suivant » :
![[Pasted image 20251208215336.png]]
On renseigne l’ID de notre réseau à l’inverse (le « 10 » correspond au dernier octet de mon IP.. Alors, c’est normal qu’il note automatiquement l’adresse inversée en bas), puis « suivant »:
![[Pasted image 20251208215356.png]]
Par défaut, on choisit que les sécurisées, puis « suivant »
![[Pasted image 20251208215422.png]]
On a bien créé notre zone de recherche inversée :
![[Pasted image 20251208215445.png]]
Avec des informations dedans
![[Pasted image 20251208215504.png]]
Maintenant il faut faire le **pointeur** :

Clic droit > Nouveau pointeur (PTR)
![[Pasted image 20251208215530.png]]
Parcourir
![[Pasted image 20251208215551.png]]
Il est créé
![[Pasted image 20251208215612.png]]
Maintenant je peux faire les deux recherches et obtenir même la zone de rechercher inversée
![[Pasted image 20251208215630.png]]
Revoir la vidéo (fin de matinée) pour voir comment bloquer un site web en créant un DNS trompeur.
Pour nettoyer le DNS : `ipconfig /displaydns`
Pour vider les DNS : `ipconfig /flush…`
# Serveur Web (IIS, Internet Information Services)

On installe un rôle de Serveur Web (IIS) : Gérer > Ajouter des rôles et fonctionnalités > Serveur Web (IIS) et suivre
![[Pasted image 20251208215734.png]]
Si l’**IIS** est bien installé, je pourrais tomber sur la configuration suivante de plusieurs façons :
![[Pasted image 20251208215817.png]]

![[Pasted image 20251208215832.png]]
On peut lancer l’utilitaire IIS : Outils > Gestionnaire des services internet (IIS)
![[Pasted image 20251208215901.png]]
D’autres configurations sur le nom du DNS
![[Pasted image 20251208215924.png]]
**Note** : Jamais supprimer le Default web site
![[Pasted image 20251208215955.png]]
Du côté droit
![[Pasted image 20251208220014.png]]
Je rentre dans Explorer, ce qui m’emmène à `wwwroot`
![[Pasted image 20251208220101.png]]
Dans `isstart.pnp` se trouve le désigné du site
![[Pasted image 20251208220209.png]]
Si je l’édite, le site va afficher la modification.

Si je les supprime (surtout le « htm »), mon site web montrera une erreur
![[Pasted image 20251208220234.png]]
Alors, je peux créer un fichier .txt et je le change pour le .html et mon site le montrera en blanc pendant qu’il ne soit pas édité (pour l’éditer on peut l’ouvrir avec bloc de note ou Paint, par exemple)
![[Pasted image 20251208220257.png]]
Si jamais on veut créer un autre document par défaut, il faudra l’ajouter dans le document par défaut
![[Pasted image 20251208220312.png]]
Revoir explication de « index of » et « zone grise »

On édite le html (cette fois on copie le script de cette page [**https://gist.githubusercontent.com/pmaldi/d6f546755af860e28fd536ac5b6d1518/raw/821f6a3ef5bfbb56d6f7e779b9cc839329d0e1a0/Index.html**](https://gist.githubusercontent.com/pmaldi/d6f546755af860e28fd536ac5b6d1518/raw/821f6a3ef5bfbb56d6f7e779b9cc839329d0e1a0/Index.html)

(il faut résoudre le souci de connexion à distance pour pouvoir copier et coller)

On ouvre le html avec bloc de notes et on colle le script

Résultat ci-dessous (une intranet, je suis connecté sur intranet mais pas sur le même DNS, pas contre mon IPv4 fonctionne) :

![[Pasted image 20251208220352.png]]
Si on essaie de rentrer dans « Accueil » il aura une erreur. Pour pouvoir emmener un utilisateur, il faudra pointer ecole.ocklock.lan ou un **CNAME**
![[Pasted image 20251208220435.png]]
On crée un **CNAME** avec un clic droit
![[Pasted image 20251208220458.png]]
On le nomme et on parcoure
![[Pasted image 20251208220521.png]]
Résultat :
![[Pasted image 20251208220546.png]]
Maintenant, l’accueil du site pointe bien sur le site
![[Pasted image 20251208220625.png]]
Pour créer un site web Aldebaran, Dans IIS on fait un nouveau site web :
![[Pasted image 20251208220658.png]]
Il faudra le nommer, et choisir le chemin `C:\inetpub\wwwroot` et on va créer le dossier Aldebaran
![[Pasted image 20251208220735.png]]
Et il va créer le chemin complet
![[Pasted image 20251208220757.png]]
Il faudra gérer le nom de l’hôte pour que le site ne soit pas en arrêt

![[Pasted image 20251208220830.png]]
Popcorn fin d’après midi
![[Pasted image 20251208220849.png]]
Si jamais il ne se met pas à jour, il faudra le démarrer, pour que le lien apparaisse du côté droit des actions
![[Pasted image 20251208220907.png]]
