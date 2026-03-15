# 🏆 Challenge B303: Agent Zabbix.
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

>**Objectif** :
>
>Mettre en place une alarme dans Zabbix qui doit se déclencher uniquement si le fichier test.txt n’existe pas sur les **machines Windows** surveillées.
>
>**Consignes** :
>
>Le fichier test.txt doit être recherché à la racine du disque C: (ou à un autre emplacement défini).
>
>Tant que le fichier est présent, l’alarme ne doit pas se déclencher (pas d’erreur, pas de problème).
>
>Si l’utilisateur supprime le fichier :
>
>* Zabbix doit remonter une erreur indiquant que le fichier a été supprimé.
>* L’alarme doit être visible afin de prévenir l’administrateur.
>
>**Points attendus** :
>
>* Création ou utilisation d’un item Zabbix permettant de vérifier l’existence du fichier.
>* Mise en place d’un trigger qui s’active uniquement en cas d’absence du fichier.
>* Vérification que l’alarme ne génère pas de faux positifs (elle reste silencieuse tant que test.txt existe).
>
>_**Astuce**_ : pensez à tester votre configuration en créant puis en supprimant le fichier pour vérifier que le comportement correspond bien à la consigne.
>

Voir 👉 [Cours B303](https://github.com/FrancoisBarsotti-Oclock/Cybersecurity/blob/main/Portfolio/CoursAIS/Bloc%20B%20(Technicien%20Infrastructure)/Saison_B3%20(Supervision)/B303_Agent%20Zabbix.md)

### 🚧 En construction 🚧

---

