# 🏆 Challenge A106
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>- En priorité, terminez l’atelier de Vendredi ! (y compris les bonus, si possible)
>- C’est à dire : installez Ubuntu et les logiciels demandés dessus !
>- Activez le copier/coller entre vos VMs et votre système hôte (petit indice : il faudra regarder du coté des « Additions Invité » de Virtual Box 😉 une petite recherche sur Internet (ou avoir écouté votre super formateur) devrait vous permettre de trouver comment faire !)
>- Bonus: Installer une 4ème VM avec le système d’exploitation Debian 13 !

Voir 👉 Cours A106 👈

---

## 📀 Virtual Box Guest Additions
* Activation dans les paramètres du `copier/coller` et `glisser/déposer` sur les VM Windows 10 et 11 après avoir installé les Additions invités (via le Disque "VirtualBox Guest Additions", monté dans les périphériques). Elles permettent en plus d'adapter la fenêtre de la VM en fonction de l'hôte.

![Win10Additions](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_01-Win10Aditions.png)

## 🐧 Ubuntu
* Un message d'erreur rencontré au lancement du disque après installation des Guest Additions:

![ErreurUbuntu](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_02-ErreurUbuntu.png)

* Erreur corrigé avec la commande `sudo apt install --reinstall bzip2 tar`qui force les outils manquants.

* Installation du reste des applications à l'aide de commandes via le terminal.

* Installation d'Okular, un outil PDF qui remplace Adobe Acrobat sur Linux, même si à priori Libre Office Draw permet de faire les basiques. Adobe Acrobat n'étant plus du tout suivit depuis des années, va aussi à l'encontre de la mentalité Linux et l'Open source.

![logiciels](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_03-logiciels.png)

## 🐧 Debian
* Création de la VM à partir de l'image fournie `debian-13.1.0-amd64-netinst.iso` avec 4096Mo de Ram, 30Go d'espace disque et 2 CPU core.

![Debian13](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_04-Debian13.png)

* Ajout d'une partition Home séparée.

![Partition](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_05-Partition.png)

* Installation de Vbox Guest Additions en chargeant l'image, lancement de l'executable, puis reboot et config, sans problème.

* Vérification et des programmes demandés avec la commande `sudo apt list grep 7zip vlc okular libreoffice`.

![UbuntuAdditions](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_06-UbuntuAdditions.png)

* Installation des paquets manquants.

![PaquetsManquant](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_07-PaquetsManquants.png)

* Vérification

![VérificationDebian](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A106_08-V%C3%A9rificationDebian.png)

