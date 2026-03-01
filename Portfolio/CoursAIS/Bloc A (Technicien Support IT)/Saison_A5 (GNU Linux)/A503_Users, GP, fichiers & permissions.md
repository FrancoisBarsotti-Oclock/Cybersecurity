# 🐧 Session A503. Utilisateurs, groupes, fichiers et permissions.

### Notions du jour
- la gestion des utilisateurs (useradd, usermod, userdel)
- la gestion des groupes (groupadd, usermod -aG, gpasswd -- delete)
- les fichiers /etc/passwd, /etc/shadow, /etc/group
- les permissions Unix, chown et chmod

----------------------------------

Un **jeu** en **lignes** de **commandes** : [Terminus](https://luffah.xyz/bidules/Terminus/)

**Jeu** pour apprendre **VIN** [Learn VIM while playing a game - VIM Adventures](https://vim-adventures.com/)

-----------------------------------
# Comptes utilisateurs

Un utilisateur identifie une personne ou un service sur la machine. II possède :
- un identifiant numérique (UID)
- un groupe primaire (GID)
- un dossier personnel et un shell par défaut
- des permissions distinctes de celles des autres utilisateurs
## /etc/passwd
Le fichier /etc/passwd recense les comptes. Chaque ligne ressemble à :
![[Pasted image 20251212152945.png]]
- baptiste : nom de compte
- x : mot de passe chiffre stocké dans /etc/shadow
- 1000 : UID (ID de l’utilisateur)
- 1000 : GID primaire (ID du groupe)
- Baptiste : commentaire (gecos)
- /home/baptiste : dossier personnel
- /bin/bash : shell par défaut

Avec : **cat /etc/passwd | echo** et le nom du compte, puis **cat /etc/passwd**
![[Pasted image 20251212153114.png]]
## /etc/shadow
Le fichier /etc/shadow contient les mots de passe chiffrés et les règles d'expiration.
![[Pasted image 20251212153224.png]]
User : mdp : date de création du mdp : nombre de jours minimal pour changer le mdp : max change (nb des jours à partir duquel le mdp doit changer) : warning de changement (nb de jours avant la fin de validité du mdp) : date d’expiration du compte

- seul root (ou un superuser via sudo ) peut le lire
- on y trouve les hachages, dates de changement, délais d'expiration

--------------------------------------------

Revoir vidéo matin 1

Hash = il y a un mdp

!hash = il est verrouillé

!! = le mdp n’a jamais été configuré

:: = pas de mdp

--------------------------------------------

Pour l’avoir : **sudo cat /etc/shadow** | grep $ user
![[Pasted image 20251212153304.png]]
![[Pasted image 20251212153318.png]]
Avec **ls /etc** on peut voir tous les dossiers « etc »
![[Pasted image 20251212153339.png]]
**Astuce** : avec **Ctrl + l** on va tout en bas sans faire « clear »

## Créer, modifier, supprimer un utilisateur

Avec la commande **man useradd** nous pourrons voir le manuel complet

Ajouter un utilisateur : **useradd** (standard, bas niveau : ça ne fait que créer l’utilisateur) ou **adduser** (non native, haut niveau, elle va demander d’autres choses. Il n’est pas partout)
![[Pasted image 20251212153411.png]]
#créer un utilisateur avec home et shell

sudo useradd -m -s /bin/bash -c "Jean Dupont" jean

#choisir un UID ou un GID primaire précis

sudo useradd -m -u 1050 -g 100 jean-tech
- -m : crée le dossier personnel (dans Home) > Si je ne veux pas
- -s : shell par défaut
- -c : commentaire (gecos)
- -u / -g : UID / GID imposés (on lui rentre manuellement le GID)
![[Pasted image 20251212153606.png]]
Pensez à définir un mdp ensuite : **sudo passwd jean** (je lui crée le mdp 0011223344)
![[Pasted image 20251212153626.png]]
On voit qu’ils ont créés
![[Pasted image 20251212153646.png]]
Si je crée un autre user avec -M, je ne le crée pas dans Home
![[Pasted image 20251212153707.png]]
Pour pouvoir voir tous mes utilisateurs (même ceux qui ne sont pas dans Home), il faudra regarder avec **cat /etc/passwd**

Si je veux voir directement un utilisateur visé (dans ce cas « Alice ») : **cat /etc/passwd | grep Alice**
![[Pasted image 20251212153731.png]]
## Modifier un utilisateur : usermod
![[Pasted image 20251212153757.png]]
- -d + -m : change le home et déplace le contenu
- -l : renomme le login
- -L / -U : verrouille/déverrouille le mot de passe dans /etc/shadow

#changer le shell

sudo usermod -s /bin/sh jean

#changer le dossier personnel (et déplacer les fichiers)

sudo usermod -d /home/jdupont -m jean

#changer le nom du compte

sudo usermod -l jdupont jean

#verrouiller/déverrouiller le compte

sudo usermod -L jean
sudo usermod -U jean
![[Pasted image 20251212153856.png]]
avec **cat /etc/passwd** je regarde les changements
![[Pasted image 20251212153921.png]]
## Supprimer un utilisateur : userdel
![[Pasted image 20251212153943.png]]
#supprimer le compte uniquement
**sudo userdel jean-tech**
![[Pasted image 20251212154034.png]]
#supprimer le compte et son dossier personnel
**sudo userdel -r jean**

Si l’on a déjà supprimé l’user mais pas le dossier : **rmdir**
- **-r** : supprime le home et la boîte mail locale (/var/mail/user)
- vérifiez avant de supprimer : aucun processus en cours, pas de fichiers critiques
avec **cat /etc/passwd** je regarde les changements
![[Pasted image 20251212154127.png]]
## Groupes
Un groupe rassemble plusieurs utilisateurs et simplifie l'attribution des droits.
- chaque utilisateur a un groupe primaire (GID)
- il peut appartenir à des groupes secondaires (supplémentaires)
- root possède des droits qui dépassent la logique de groupe

Ils sont utilisés pour gérer les permissions.
### /etc/group
Le fichier **/etc/group** liste les groupes :
![[Pasted image 20251212154210.png]]
- sudo : nom du groupe
- x : mot de passe du groupe (rarement utilisé)
- 27 : GID
- baptiste, jean : membres secondaires du groupe

Pour les voir tous : **cat /etc/group**
![[Pasted image 20251212154305.png]]
Pour voir un résumé : **cat /etc/group | grep fb**
![[Pasted image 20251212154337.png]]
### Créer / supprimer un groupe
![[Pasted image 20251212154414.png]]
#créer un groupe
**sudo groupadd reseau**

#imposer un GID
**sudo groupadd -g 1052 groupaupif**
![[Pasted image 20251212154446.png]]
![[Pasted image 20251212154502.png]]
![[Pasted image 20251212154512.png]]
#supprimer un groupe
**sudo groupdel reseau**
### Ajouter / retirer un utilisateur d'un groupe
Ajouter un utilisateur à des groupes secondaires :
![[Pasted image 20251212154623.png]]
sudo usermod -aG sudo, reseau jean
- -G : liste des groupes secondaires
- -a : ajoute sans écraser les groupes secondaires existants
### Retirer un utilisateur d'un groupe :
![[Pasted image 20251212154654.png]]
sudo gpasswd -- delete jean reseau
### Changer le groupe primaire d'un utilisateur :
![[Pasted image 20251212154718.png]]
sudo usermod -g devops jean

---------------------------------

**Astuce** : Utilisez un max "grep" ça aide a capter > sudo usermod -aG grouptest freed (j'ajoute freed à mon grouptest)  > cat /etc/group | grep freed >> je cherche freed dans mes groupes, je vois son groupe primaire (freed:x:1000: ) et le secondaire (grouptest:x:1055:bob,freed)
![[Pasted image 20251212154821.png]]
Résumé : **cat /etc/passwd** pour lister les utilisateurs.
**cat /etc/group** pour lister les groupes.
**cat /etc/shadow** = les pass

-----------------------------------
Commandes à coller (pour la pratique de groupes et permissions)

sudo groupadd travailleurs  
sudo groupadd chefs  
  
for user in ouvrier employe technicien leader patron; do  
sudo useradd -M -N -r "$user"  
done  
  
mkdir -p srv/permissions/vestiaires  
mkdir -p srv/permissions/open_space  
mkdir -p srv/permissions/bureaux  
  
echo "contenu du projet a" > /srv/permissions/open_space/projet_a.txt  
echo "contenu du projet b" > /srv/permissions/open_space/projet_b.txt  
echo "contenu du projet c" > /srv/permissions/open_space/projet_c.txt  
  
echo "compte-rendu de la dernière AG" > /srv/permissions/bureaux/compte_rendu.txt  
echo "réunion top-secrète" > /srv/permissions/bureaux/reunion_strategique.txt  
  
for user in ouvrier employe technicien leader patron; do  
echo "contenu du casier de ${user}" > "/srv/permissions/vestiaires/${user}.txt"  
done  
  
find /srv/permissions -type d -exec chmod 777 {} \;  
find /srv/permissions -type f -exec chmod 666 {} \;  
  
echo "C'est en place"

![[Pasted image 20251212155024.png]]
![[Pasted image 20251212155038.png]]
![[Pasted image 20251212155059.png]]
## Permissions Unix
Chaque fichier ou dossier possède 3 blocs de droits :
- u (user) : propriétaire
- g (group) : groupe propriétaire
- o (others) : tous les autres
### Lire les droits
**ls -l** affiche les permissions :
![[Pasted image 20251212155133.png]]
-rw-r -- r -- 1 jean devops 4200 Jan 5 10:15 rapport.txt
drwxr-x --- 2 root reseau 4096 Jan 5 09:80 projets

- 1er caractère : type ( - fichier, d dossier, l lien ... )
- bloc rwx pour l'utilisateur, le groupe, puis les autres
### Signification des bits
- r (read) : lire un fichier / lister un dossier
- w (write) : modifier un fichier / créer-supprimer dans un dossier
- x (execute) : exécuter un binaire / traverser un dossier

Sur un dossier, x est indispensable pour parcourir son contenu, même si **r** est présent.
![[Pasted image 20251212155222.png]]
![[Pasted image 20251212155232.png]]
### Changer propriétaire : chown
![[Pasted image 20251212155710.png]]
#changer propriétaire et groupe
**sudo chown jean:devops rapport.txt**

#ne changer que le groupe
**sudo chown :reseau projets**

#appliquer récursivement
**sudo chown -R www-data:www-data /var/www/html**
- user : group permet de modifier les deux en une fois
- -R descend dans l'arborescence
![[Pasted image 20251212155813.png]]
**Pour voir l’arborescence des permissions** : `/srv/permissions# tree -pug`
![[Pasted image 20251212155920.png]]
### Changer les droits : chmod
Deux syntaxes possibles : symbolique ou octale.
### Syntaxe symbolique
![[Pasted image 20251212155949.png]]
#**donner l'exécution au propriétaire**
`chmod u+x script.sh`

#**retirer l'écriture aux autres**
`chmod o-w rapport.txt`

#**aligner les autres sur le groupe**
`chmod o=g partage/`

- `u` **propriétaire**, `g` **groupe**, `o` **autres**, `a` **tous**
- `+` **ajoute**, `-` **retire**, = impose exactement
### Syntaxe octale

Chaque bloc **rwx** correspond à un chiffre (**r = 4, w = 2, x = 1**) :
![[Pasted image 20251212160445.png]]
#**fichier lisible par tous, modifiable par le propriétaire**
`chmod 644 rapport. txt

#**exécutable pour tous, écriture pour proprio et groupe**
`chmod 775 script.sh

#**droits stricts : propriétaire seul**
`chmod 700 secrets/

Sur un dossier partage, pensez au x pour permettre la traversée.
## Sudo
Monter en privilèges sans rester root.
* pourquoi utiliser sudo plutôt que se connecter en root
* le fichier `/etc/sudoers` et ses règles
* `visudo`, l'outil sûr pour modifier la configuration
* quelques démos à reproduire sur une VM Linux

---------------------------------------
Résumé :
**Utilisateurs**
   * useradd, usermod, userdel => commandes utilisateurs
   * `/etc/passwd` => liste les utilisateurs
   * `/etc/shadow` => liste les mots de passe
**Groupes**
   * `groupadd, groupdel
   * `/etc/group` => liste les groupes
   * pour ajouter un utilisateur à un groupe, on modifie l'utilisateur, pas le groupe
   * à la création, chaque user a un GID
   * Les groupes servent à faire des ensembles d’utilisateurs
   * Les groupes permettent de gérer les droits

**Permissions**
   * Une ressource ça a : un propriétaire, un groupe, et les autres
   * Les droits s’appliquent au niveau des ressources
   * Les droits s’appliquent pour chacun de ces 3 acteurs
   * `Chown` => changer le propriétaire et/ou le groupe d’une ressource
   * `Chmod` => permet de changer les droits d’une ressource
	Pourquoi 3 niveaux de droits :
   * Les droits du propriétaire
   * Les droits du groupe
   * Les droits des autres
	! Pour pouvoir parcourir un dossier, il faut avoir le droit d’exécution

**Syntaxe symbolique**
               r = droit de lecture
               w = droit d’écriture
               x = droit d’exécution
**Syntaxe octale**
	r = 4 ; w = 2 ; x = 1
	0 `---
	1 `--x
	2 `-w-
	3 `-wx
	4 `r--
	5 `r-x
	6 `rw-
	7 `rwx
	Alors le droit **644** correspond à - `rw-r- -r--

--------------------------------------------

### Pourquoi sudo ?
* applique le principe du moindre privilège : on reste utilisateur normal, on élève ponctuellement
* journalise les commandes et les erreurs (audit)
* contrôle fin : qui peut faire quoi, sur quelle machine, avec quel mot de passe
* évite les sessions root permanentes et les erreurs irréversibles
### Comment ça marche ?
- sudo vérifie l'utilisateur et son mot de passe
- lit **/etc/sudoers** et les fichiers du dossier **/etc/sudoers . d/**
- valide la commande demandée selon les règles
- passe l'environnement et le contexte précisés dans la config
![[Pasted image 20251212161710.png]]
![[Pasted image 20251212161745.png]]
### /etc/sudoers

Fichier principal de configuration. Ne l'éditez JAMAIS avec un éditeur classique : utilisez visudo.

Structure minimaliste :
![[Pasted image 20251215103831.png]]
User_Alias ADMINS = baptiste, jean
Defaults env_reset
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/b
Root           ALL=(ALL:ALL) ALL
%sudo      ALL=(ALL:ALL) ALL
ADMINS   ALL=(root) /usr/bin/systemctl restart nginx
### Clés de lecture
![[Pasted image 20251215103928.png]]
 %sudo : tous les membres du groupe sudo

• ALL : depuis n'importe quelle machine (utile surtout en multi-hôte)
• (ALL:ALL) : exécuter en tant que n'importe quel utilisateur et groupe
• ALL : toute commande
### Règle ciblée
![[Pasted image 20251215104042.png]]
baptiste ALL=(www-data) /usr/bin/systemctl restart nginx
- baptiste peut relancer nginx en se faisant passer pour www-data
- reste bloqué pour le reste
NOPASSWD : peut supprimer la demande de mot de passe, à utiliser avec parcimonie.
### visudo
L'outil recommande pour modifier la configuration.
![[Pasted image 20251215104128.png]]
sudo visudo
# ou éditer un fichier dédié :

**sudo visudo -f /etc/sudoers.d/web**
L'outil recommande pour modifier la configuration.
- alias : regrouper utilisateurs/commandes si besoin
- Defaults : options globales (environnement, logs ... )
- règles : qui / depuis où / en tant que qui / quelles commandes
- verrouille le fichier pendant l'édition
- vérifie la syntaxe avant d'enregistrer
- empêche de casser l'accès sudo par une erreur

Préférez créer des fichiers dans /etc/sudoers. d/ plutôt que d'éditer directement /etc/sudoers.
### Démos sur VM (1/2)
1. Vérifier ses droits :
![[Pasted image 20251215104208.png]]
id
groups
**sudo - l**
2. Tester des commandes simples :
![[Pasted image 20251215104402.png]]
**sudo whoami**

**sudo tail -n 5 /var/log/syslog**     # ou /var/log/messages

3. Ajouter un utilisateur au groupe sudo :
![[Pasted image 20251215104523.png]]
**sudo usermod -aG sudo jean**

Déconnexion/reconnexion pour appliquer.

### Démos sur VM (2/2)

4. Créer une règle ciblée :
![[Pasted image 20251215104654.png]]
**sudo visudo -f /etc/sudoers.d/web**
#y ajouter :
#jean ALL=(www-data) /usr/bin/systemctl restart n

5. Tester l'accès :
**sudo systemctl restart nginx**      # en tant que jean
**sudo -l**      # vérifier les droi
### Journalisation

- Debian/Ubuntu : tail -f /var/log/auth.log | grep sudo
- RHEL/CentOS : journalctl -f -t sudo
- les logs montrent l'utilisateur, la commande, le résultat (success/denied)



Règle standard
### Bonnes pratiques
- éviter sudo su - ou sudo - s prolongés, préférer des commandes ciblées
- limiter les règles ALL et les NOPASSWD
- utiliser sudoedit pour éditer des fichiers de config (ouvre dans un éditeur sans exécuter d'interpréteur)
- ranger vos règles dans /etc/sudoers. d/ avec un nom explicite, un par périmètre
- tester avec sudo -l et surveiller les logs après modification

### Points clés à retenir
- sudo permet une élévation contrôlée et tracée des privilèges
- /etc/sudoers définit qui peut faire quoi et comment
- visudo protège la configuration en vérifiant la syntaxe
- privilégiez des règles minimales, testées, et loggez vos usages
