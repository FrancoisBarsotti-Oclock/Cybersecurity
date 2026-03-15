# Session B401 : Bases de l’algorithmie

### Notions du jour
* scripting : intérêt ?
* langages de script vs. langages compilés
* algorithmique, bases de la programmation :
    * variables
    * conditions
    * boucles
    * fonctions
* logigramme
* scratch

_Découvrons les bases de la programmation et apprenons à automatiser des tâches avec des scripts !_
## C'est quoi, un script ?
En Informatique, un script désigne un programme chargé d'exécuter une ou plusieurs actions prédéfinies.

C'est souvent une suite de commandes plus ou moins simples, qui permettent d'automatiser des tâches dans un ordre donné. Ces commandes sont écrites dans un fichier texte, qu'on appelle script.
### Langage de script
Les scripts sont rédigés dans un langage de programmation interprété (ils n'ont pas besoin d'être compilés, contrairement à un programme écrit en C par exemple).

Historiquement, on parlait même de langage de commande ou d'enchainement des travaux, ils permettaient simplement d'automatiser une suite de commandes simples, a la manière d'un
script de théâtre.

De nos jours, les langages de script offrent des fonctionnalités similaires aux autres langages de programmation.

Selon langage
Batch = 
```batch
set nom=François
```
PowerShell = 
```powershell
$nom = "François"
```
Bash = 
```bash
nom="François"
```
Python = 
```python
nom = "François"
```

### Quel langage choisir ?

Le langage de script utilisé dépend du système d'exploitation sur lequel le script devra être lancé :

* Sur Windows :
    * script "batch", extension de fichier .bat
    * script PowerShell, extension .ps1
* Sur GNU/Linux et MacOS :
    * script Shell/Bash, extension .sh

_Mince, on ne peut pas faire **un script qui fonctionne sur Windows ET sur GNU/Linux** ?_

Si, avec un langage de script compatible avec les deux systèmes, comme Python par exemple ! (extension .py)

Attention, Python tourne sur Windows, Linux et MacOS, mais les commandes à lancer/automatiser restent potentiellement différentes d'un système à l'autre.

### Pourquoi faire ?

Imaginons qu'on ait besoin de créer 5 dossiers sur le disque dur de plusieurs machines dans notre parc.

Si le parc est composé de 10 machines, ça va être fastidieux mais on peut le faire a la main. Si le parc est composé de 1000 machines, il va falloir automatiser cette tâche.

Et en plus, ça fait partie des compétences que l'on doit maitriser pour obtenir le TP: Voir la fiche professionnelle #6 (Automatiser des tâches à l'aide de scritps).





