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

## Bases de la programmation
_Quelques concepts de base, avant d'attaquer !_

### Opérateurs logiques

Plus grand que : > GT (pour Greater than)
Plus grand ou égal que : >= GTE (pour Greater than or equal)
Égal à : == EQ (equal)
Strictement égal à : ===
Différent de : != NEQ (Not Equal)
Strictement différent de : !==
Plus petit que : < LT (Lower Than)
Plus petit ou égal à : <= LTE (Lower than or Equal)

```python
if (1 == 1) { // OK

}

if (1 == '1') { // Ici on compare uniquement la valeur, donc 1 = 1 donc c'est OK

}

if (1 === '1') { // On compare la valeur ET le type (nombre entier Vs. chaine de caractères) donc c'est KO

}
```

* **Pour combiner plusieurs conditions** : condition 1 && condition 2 (and, -and, &, && selon langage) ; donc vérifie si les 2 condition sont respectées.

* **Pour combiner plusieurs conditions et vérifier qu’au moins une est OK** : condition 1 || condition 2 (or, -or, | , || selon le langage) ; cela vérifie si au moins une des 2 conditions est OK.

* Nous pouvons combiner autant de conditions que nécessaires, et même faire un mélange entre ET et OU.

### Variables

Le principe d’une **variable** est de stocker du contenu (Texte, nombre, listes, etc…) dans un espace de stockage lié au programme, au script, que l’on nomme comme on veut et que l’on peut réutiliser partout (on stocke une information).

Par **exemple** :

$prénom = "Robin" (pas de comparaison quand il y a qu'un seul "=" ; Il s'agit d'assigner la valeur Robin a la variable $prénom)
= > Assignation de variable
== > Comparaison simple
=== > Comparaison stricte

En programmation, une variable permet de stocker une donnée (chaîne de caractères, nombre entier ou à virgule, booléen vrai ou faux, une liste/un tableau, etc.) dans la mémoire vive de l'ordinateur.

### Conditions

Les conditions vont nous permettre de modifier le comportement de notre programme (même une boucle) !
Quelques **exemples** :

* Si un dossier est présent, le supprimer. Sinon, ne rien faire.
* Si l'utilisateur répond "non" à une question posée par le script, ne pas faire une certaine action.
* etc.

```python
if ($age >= 21) {
    echo "majeur";
} elseif ($age >= 18) {
    echo "Mineur mais peut tuer";
} else {
    echo "Mineur";
}
```

### Boucles
Les boucles permettent de répéter une (ou plusieurs) action(s) un certain nombre de fois.
On peut également s'en servir pour parcourir une liste d'éléments et effectuer une action pour chaque élément dans la liste.

Mots clés : `for`, `while`

Exemple de tableau : [element1, element2, element3, element4]
Pour chaque élément du tableau ci-dessus, on va faire cette liste d’actions :
-action 1
-action 2
-action 3

**ATTENTION** : Cela varie en fonction du langage 
Je déclare une variable tableau dans lequel je stock une liste d’éléments

$tableau = [element1, element2, element3, element4]

* Pour chaque élément du tableau que je vais récupérer dans ma boucle sous le nom $élément
* Pour chaque élément, ma boucle va faire certaines actions que je vais définir
* Puis, une fois les actions finies, je passe à l’élément suivant et je refais le 
>For (*élément in $tableau) {
>$élément->add($user)
>}
>
>For

```python
$tableau = [$pc1, $pc2, $pc3, $pc4];

// Je récupère chaque élément du tableau ci-dessus
// Chaque élément sera récupéré sous le nom $ordinateur
for ($ordinateur in $tableau) {
    // J'éxecute une liste d'action sur l'ordinateur
    $ordinateur->ajouterUtilisateur();
    $ordinateur->action2();
    $ordinateur->redemarrer();
    // Une fois mes actions finies, je passe à l'élément suivant de mon tableau et je recommence
}

// une fois mon tableau terminé, quand j'arrive à la fin, la boucle se termine
```

### Fonctions
Les fonctions permettent de "regrouper" plusieurs actions dans un "bloc", et ce bloc d'actions pourra être déclenché depuis différents endroits dans notre script. Elles évitent de répéter le même code.

* ✔️ Je crée ma fonction `verifierAge`, et je demande d* prendre un paramètre, l’âge à tester.
* ✔️ Je récupère l’âge dans la variable `$age`
* ✔️ Je fais ma condition pour vérifier l’âge de la personne
* ✔️ Et en fonction du résultat, j’affiche mineur ou majeur.

Maintenant que ma fonction `vérifierAge` est créée, je peux l’utiliser autant de fois que je veux juste en appelant « `VéfrifierAge(ageàVérivier)` », par exemple :

* `VérifierAge(18)`
* `VérifierAge(12)`
* `VérifierAge(39)`

### 🚧 En construction 🚧 (pag. 5)

---