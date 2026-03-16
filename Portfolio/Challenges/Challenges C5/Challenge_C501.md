# 🏆 Challenge C501: Introduction au Pentesting & XSS
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>* Créer votre compte sur la [plateforme de CTF Root-Me](https://www.root-me.org/?lang=fr),
>* Réaliser les 3 premiers challenges [XSS Portswigger](https://portswigger.net/web-security/all-labs#cross-site-scripting),
>* Réaliser le challenge [XSS-DOM-Based-Introduction](https://www.root-me.org/fr/Challenges/Web-Client/XSS-DOM-Based-Introduction).
>* Réaliser les 3 exploitations de XSS Sur DVWA


Voir 👉 [Cours C501](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20C%20(Administrateur%20Cybers%C3%A9curit%C3%A9)/Saison_C5%20(Pentesting)/C501_Intro%20au%20Pentesting%20%26%20XSS.md) 👈

---

### 🔗 Liens utiles :

* [CWE-79: XSS](https://cwe.mitre.org/data/definitions/79.html)
* [OWASP XSS](https://owasp.org/www-community/attacks/xss/) : une page de l’OWASP dédiée aux attaques XSS
* [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) : une liste de bonnes pratiques pour prévenir les attaques XSS
* [OWASP DOM-based XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html) : une liste de bonnes pratiques pour prévenir les attaques XSS basées sur le DOM


### 📚 Ressources:

* [OWASP – Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
* [PortSwigger – XSS Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
* [HackTheBox – Starting Point](https://www.hackthebox.com/starting-point)
* [TryHackMe – XSS Room](https://tryhackme.com/room/xss)

---

## Partie 1: création de compte sur Root Me

✅ Compte créé et enregistré sur le gestionnaire de mots de passe.

![00-RootMe](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C5/images%20C5/images%20C501/Challenge%20C501_00-RootMe.png)

## Partie 2: 

### Lab 1: Reflected XSS into HTML context with nothing encoded

Le site du lab, nous présente une "search box" où l'on peut mettre n'importe quel mot ou phrase, pour avoir ce que l'on écrit en retour. Dans ce cas, j'ai écrit "Hi, Francois!":

![01-HiFrancois](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C5/images%20C5/images%20C501/Challenge%20C501_01-HiFrancois.png)

Pour lancer l'attaque Reflected XSS (pour que la victime doive cliquer sur le lien piégé), j'ai mit `C<script>alert(1)</script>` dans la "search box"

![C501_02-Lab1Solved](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/Challenges/Challenges%20C5/images%20C5/images%20C501/Challenge%20C501_02-Lab1Solved.png)

### Lab 2: Stored XSS into HTML context with nothing encoded

L'objectif de ce lab est de faire que le script soit exécuté automatiquement à chaque fois que la victime charge le blog. Le message de pop-up sera le même que pour le reflected XSS.

### Lab 3: DOM XSS in `document.write` sink using source `location.search`

