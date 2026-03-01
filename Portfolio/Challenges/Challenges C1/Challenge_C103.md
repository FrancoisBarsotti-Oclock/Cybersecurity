# 🏆 Challenge C103: Registre de risques
### François BARSOTTI
## 🎯 Pitch et Contexte du challenge

> Continuation (Partie 3) du projet "fil rouge", à être suivi tout au long de la saison C1.
## Énoncé E3 - Consignes
> Jusqu'hier vous avez créé la note de cadrage et le WBS du projet.
> 
> Aujourd'hui on s'attaque à l'analyse des risques !
> 
> 1. Listez au moins 10 risques du projet (technique, humain, organisationnel)
> 2. Évaluez chaque risque :
>       * Probabilité : faible/moyenne/forte
>       * Impact : faible/moyen/critique
>       * Criticité = probabilité × impact
> 3. Classez les risques par criticité
> **Livrable attendu** : registre des risques avec évaluation.

Voir 👉 [Cours C103]()

# :orange_book: Registre des risques - Modernisation infrastructure IT
|**ID** | **Risque** | **Type** | **Probabilité** | **Impact** | **Criticité** |
| -------- | :-------: | :-------: | :-------: | :-------: | :-------: |
|R1  | Sous-cimensionnement des équipéments | Technique | Moyenne | Critique | :red_circle: **Élevée** |
|R2 | Mauvaise configuration du firewall | Technique | Moyenne | Critique | :red_circle: **Élevée** |
|R3 | Interruption prolongée du réseau lors du déploiement | Technique | Moyenne | Critique | :red_circle: **Élevée** |
|R4 | Dépassement de budget | Organisationnel | Moyenne | Moyen | :orange_circle: **Moyenne** |
|R5 | Retard fournisseur matériel | Organisationnel | Moyenne | Moyen | :orange_circle: **Moyenne** |
|R6 | Résistance au changement des utilisateurs | Humain | Moyenne | Moyen | :orange_circle: **Moyenne** |
|R7 | Manque de compétences internes (alternant débutant) | Humain | Moyenne | Moyen | :orange_circle: **Moyenne** |
|R8 | Mauvaise gestion des droits sur serveur/NAS | Technique | Moyenne | Critique | :red_circle: **Élevée** |
|R9 | Absence ou échec des sauvegardes | Technique | Faible | Critique | :orange_circle: **Moyenne** |
|R10 | Documentation insuffisante | Organisationnel | Moyenne | Moyen | :orange_circle: **Moyenne** |
|R11 |Fail­le de sécurité Wi-Fi | Technique | Faible | Critique | :orange_circle: **Moyenne** |
|R12 |Mauvaise segmentation VLAN | Technique | Moyenne | Critique | :red_circle: **Élevée** |
#
## Classement par criticité (du plus critique au moins critique)

### :red_circle: Criticité Élevée
* R1 – Sous-dimensionnement
* R2 – Mauvaise configuration firewall
* R3 – Interruption réseau
* R8 – Mauvaise gestion des droits
* R12 – Mauvaise segmentation VLAN

### :orange_circle: Criticité Moyenne
* R4 – Dépassement budget
* R5 – Retard fournisseur
* R6 – Résistance au changement
* R7 – Manque compétences internes
* R9 – Sauvegardes défaillantes
* R10 – Documentation insuffisante
* R11 – Sécurité Wi-Fi insuffisante
#
### :brain: A tenir en compte
Les risques **les plus critiques** sont liés à :
* la sécurité réseau (firewall, VLAN, droits)
* la continuité de service
