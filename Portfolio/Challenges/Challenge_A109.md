# 🏆 Challenge A109: Intro Adressage IP
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

> « Pour les adresses IP et masques de sous-réseau suivants, calculez :
> -	l’adresse de réseau
> -	l’adresse de broadcast
> -	le nombre d’adresses utilisables par des machines
> -	la plage d’adresses disponibles
> Certains utilisent la notation « classique », d’autres la notation CIDR :
>      +	192.168.13.67/24
>      +	172.16.0.1 – 255.255.255.0
>      +	172.16.27.32/23
>      +	10.7.5.1 – 255.255.128.0
>      +	10.42.0.82/12
>        
>Essayez de calculer tout à la main (avec la méthode de votre choix, idéalement essayez d’utiliser les deux !), puis vérifiez vos calculs avec une calculatrice en ligne [exemple](https://www.subnet-calculator.com/cidr.php) ! »
> #

Voir 👉 Cours A109 👈

---

## 🧠🧮 Let's make some math

### - Adresse IP ``192.168.13.67/24``

/24 signifie qu'on a 24 "1" dans le masque réseau, ou 24/8 = 3 octets, traduit en binaire :

1111 1111 . 1111 1111 . 1111 1111 . 0000 0000 qui correspond à **255.255.255.0** en décimal.

- ### [Si nous utilisons la méthode logique](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/images_challenges/Challenge%20A109_01-M%C3%A9thodeLogique.png)

(adresse IP) ET (masque)=adresse réseau

(NOT masque) OU (adresse réseau)=adresse broadcast

- ### **Calcul de l'adresse réseau**

**IP 192.168.13.67** : on commence par traduire cette IP en binaire, on va utiliser la table de calcul à partir des multiples de 2.

| Table | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | ---- | ---- | ---- | ---- | ---- | ---- | ----- |
| 192 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| 168 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 |
| 13 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| 67 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 |

**IP en binaire** : 1100 0000 . 1010 1000 . 0000 1101 . 0100 0011

**Masque binaire** : 1111 1111 . 1111 1111 . 1111 1111 . 0000 0000

**ET Logique** : 1100 0000 . 1010 1000 . 0000 1101 . 0000 0000

résultat : **Adresse réseau : 192.168.13.0**

- ### **Calcul de l'adresse de broadcast**

**NOT Masque binaire** : 0000 0000 . 0000 0000 . 0000 0000 . 1111 1111

**Adresse réseau** : 1100 0000 . 1010 1000 . 0000 1101 . 0000 0000

**Adresse broadcast** : 1100 0000 . 1010 1000 . 0000 1101 . 1111 1111

résultat : **Adresse broadcast : 192.168.13.255**

- ### **Calcul du nombre d'adresses utilisables pour les machines**

On est en /24 soit 3 octets réservés au réseau, il reste donc un seul octet pour les hôtes : 8 bits. Néanmoins 2 adresses sont réservées, l'adresse réseau et l'adresse de diffusion (broadcast) ne sont pas utilisables par les machines.

Formule de calcul des hôtes possible : ``(2^b)-2``, ici (2^8)-2 = **254**

- ### **Calcul de la plage d'adresses disponibles**

On va faire le même calcul mais cette fois on inclu toutes les adresses.

Formule de calcul de la plage d'adresses disponibles : ``(2^b)``, ici (2^8) = **256**

- **Résultats**

``192.168.13.67/24``

**Masque sous-réseau** : 255.255.255.0

**Adresse réseau** : 192.168.13.0

**Adresse broadcast** : 192.168.13.255

**Machines hôtes** : 254

**Plage d'adresses** : 256, 192.168.13.0 à 192.168.13.255

---
### Si l'on utilise la [méthode du "Nombre magique"](https://zestedesavoir.com/tutoriels/321/calculer-une-plage-dadresses-avec-la-methode-magique/#2-10258_quest-ce-que-la-methode-magique) à partir de maintenant car c'est plus rapide et surtout qu'il peut se calculer de tête et sans papier.
### - Adresse IP ``172.16.0.1 – 255.255.255.0``

- ### **Calcul de l'adresse réseau**

Le masque étant 255.255.255.**0**, l'**octet significatif** est donc le **dernier** octet (on peut reconnaître l'octet significatif comme le premier qui n'est pas 255).

Si 256-0 = 256, les multiples de 256 sont 0, 256, 512...

0 est le multiple inférieur/égal à 1 (dernier octet de l'adresse IP)

Alors,

L'adresse réseau est **172.16.0.0**

- ### **Calcul de l'adresse de broadcast**

On prend le multiple suivant, donc 256, -1

L'adresse broadcast est donc **172.16.0.255**

- ### **Calcul du nombre d'adresses utilisables pour les machines**

Le masque étant 255.255.255.0, il y a seulement un octet libre, soit (2^8)-2 = **254** (car une est réservée pour l'AR et l'autre pour l'AB).

- ### **Calcul de la plage d'adresses disponibles**

Idem sans enlever les 2 adresses, soit (2^8) = **256**

- **Résultats**

``172.16.0.1 – 255.255.255.0`` (0 sur le dernier octet a été pris par l'AR et 255 sur le dernier octet a été pris par l'AB).

**Masque sous-réseau** : 255.255.255.0

**Adresse réseau** : 172.16.0.0

**Adresse broadcast** : 172.16.0.255

**Machines hôtes** : 254

**Plage d'adresses** : 256, 172.16.0.0 à 172.16.0.255

On peut aussi l'écrire : 172.16.0.1/24

---

### - Adresse IP ``172.16.27.32/23``

- ### **Calcul de l'adresse réseau**

Le masque en /23 se traduit par 23 "1" puis 9 "0" en binaire :

1111 1111 . 1111 1111 . 1111 1110 . 0000 0000 soit 255.255.**254**.0 et l'**octet significatif** est le **3ème**.
Multiples: 0,2,4,6,...,26,28,30...

256-254 = **2** (comme nombre magique).
Multiples: 0 2 4 6 8 10 ... 24 26 28 30 ...

26 est le multiple inférieur/égal à 27 (3ème octet de l'adresse IP), et le dernier octet sera 0.

L'adresse réseau est donc **172.16.26.0**

- ### **Calcul de l'adresse de broadcast**

On prend le multiple supérieur à 27, donc 28 et on soustrait 1

L'adresse broadcast est donc **172.16.27.255**

- ### **Calcul du nombre d'adresses utilisables pour les machines**

Le masque étant /23 il reste (32-23) = 9 bits libres pour les hôtes, soit (2^9)-2 = **510**

- ### **Calcul de la plage d'adresses disponibles**

Idem sans enlever les 2 adresses, soit (2^9) = **512**

- **Résultats**

``172.16.27.32/23``

**Masque sous-réseau** : 255.255.254.0

**Adresse réseau** : 172.16.26.0

**Adresse broadcast** : 172.16.27.255

**Machines hôtes** : 510

**Plage d'adresses** : 512, 172.16.26.0 à 172.16.27.255

---

### - Adresse IP ``10.7.5.1`` 

### Si on le traite comme une adresse classful (Classe A ou < 126, ancien modèle)

👉 Le masque est 255.0.0.0 (par défaut pour les IP de Classe A, car il y a 24 bits libres pour les hôtes). Ceci fermerait la possibilité à discusion pour un CIDR /8:

- [x] Masque:       255.0.0.0 (par défaut pour les IP de Classe A, car il y a 24 bits libres pour les hôtes)
- [x] Nombre magique: 256-0 = 256
- [x] Multiples: 0,2,4,6,...,256...

Alors,
- [x] Adresse réseau (AR):    10.0.0.0
- [x] Adresse broadcast (AB): 10.255.255.255
- [x] Nombre d'addresses utilisables par des machines: 256-2 = 254 (car une est réservée pour l'AR et l'autre pour l'AB)
- [x] Plage d'adresses disponibles: 10.0.0.1 - 10.255.255.254 (0 sur le dernier octet a été pris par l'AR et 255 sur le dernier octet a été pris par l'AB)

### Mais si l'on suit la méthode magique

- ### **Calcul de l'adresse réseau**

255.255.**128**.0, 3ème **octet significatif**, Nbre magique : 256-128 = 128, ses multiples sont 0, 128, 256.

0 <= 5 donc l'adresse réseau est **10.7.0.0**

- ### **Calcul de l'adresse de broadcast**

(128 > 5) -1  donc l'adresse broadcast est **10.7.127.255**

- ### **Calcul du nombre d'adresses utilisables pour les machines**

255.255.128.0 est 1111 1111 . 1111 1111 . 1000 0000 . 0000 0000 en binaire.

Soit 17 bits utilisés pour le réseau et 15 bits libres pour les hôtes.

Donc (2^17)-2 = **131070**

- ### **Calcul de la plage d'adresses disponibles**

(2^17) = **131072**

- **Résultats**

``10.7.5.1 – 255.255.128.0``

**Masque sous-réseau** : 255.255.128.0

**Adresse réseau** : 10.7.0.0

**Adresse broadcast** : 10.7.127.255

**Machines hôtes** : 131070

**Plage d'adresses** : 131072, 10.7.0.0 à 10.7.127.255

Nous avons à faire à un réseau bien plus vaste.

---

### - IP Adress ``10.42.0.82/12``

👉 Le fait de nous donner le CIDR /12 nous dit qu'il ne s'agit pas d'un adressage classful (Class C).

- ### **Calcul de l'adresse réseau**

Le sous-masque /12 correspond à 12 bits utilisés et 20 bits libres soit 1111 1111 . 1111 0000 . 0000 0000 . 0000 0000 en binaire et 255.**240**.0.0 en décimal.

L'**octet significatif** est donc le **second**. Nbre magique : 256-240 = 16, ses multiples sont : 0, 16, 32, 48.

32 <= 42 donc l'adresse réseau est **10.32.0.0**

- ### **Calcul de l'adresse de broadcast**

(48 > 42) -1  donc l'adresse broadcast est **10.47.255.255**

- ### **Calcul du nombre d'adresses utilisables pour les machines**

Le masque étant /12 il reste (32-12) = 20 bits libres pour les hôtes, soit (2^20)-2 = **1 048 574**

- ### **Calcul de la plage d'adresses disponibles**

(2^20) = **1 048 576**

- ### **Résultats**

``10.42.0.82/12``

**Masque sous-réseau** : 255.240.0.0

**Adresse réseau** : 10.32.0.0

**Adresse broadcast** : 10.47.255.255

**Machines hôtes** : 1 048 574

**Plage d'adresses** : 1 048 576, 10.32.0.0 à 10.47.255.255

---

### 📚 Ressources:
### - CIDR Calculator 🔣

![CIDR Calculator](../images/CIDRcalc.png): Aujourd'hui on peut aller plus vite avec les calculatrices en-line.

[Qu'est-ce que la méthode magique?](https://zestedesavoir.com/tutoriels/321/calculer-une-plage-dadresses-avec-la-methode-magique/#2-10258_quest-ce-que-la-methode-magique)
