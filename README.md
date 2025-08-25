# Extension de Théorie des Groupes pour Mathematica (Wolfram)

Oscar Eatwell, Jules Charlier,, Jawad Ben Brahim et Rafael Salavarria Lorenzoni

## Composants : 

- Fonctions (Affichage des tables de Cayley pour des groupes non abéliens et des monoïdes, des attributs du groupe ou du monoïde (inverse de chaque élément, élément neutre, opération élémentaire, ordre), cosets, isomorphismes, représentation classique, ordre des éléments, visualisation du groupe, groupes quotient, etc.), champs
- Exemples (Collisions de particules, RSA, Rubik's Cube, jeu de taquin, Modèle standard, Insolvabilité des polynômes degré 5, etc.)
- Documentation (Théorie des groupes, tutoriels, références, etc.)


## Liste des fonctions 

- `IdentityElement[group]` : renvoie l'élément neutre du groupe `group`, ou du monoïde `group`
- `GroupInverse[group, g]` : renvoie l'inverse de `g`, `g` étant un élément du groupe `group`
- `CayleyMultiplicationTable[group]` : renvoie la table de Cayley du groupe `group`, ex `Z4 = {4, Plus}`. (venant compléter `GroupMultiplicationTable[group]` en permettant d'afficher des tables de groupes non abéliens et de monoïdes) + options d'affichage (meilleure lisibilité, plus jolis graphismes, etc)
- Affichage groupes dihédraux.

## Réalisations

| Fonctionnalité | Statut |
|-----------------|--------|
| `IdentityElement[group]` | ✅ Complété |
| `GroupInverse[group, g]` | ✖️ Non complété |
| `CayleyMultiplicationTable[group]` | ✖️ Non complété |

## Installation

(rien ici pour l'instant)

## Code source

- `IdentityElement[group]` : 

```wl
IdentityElement[group_] := Module[{elems, n}, Which[
   (*Groupes finis standard*)Head[group] === CyclicGroup, 0,
   Head[group] === DihedralGroup, Cycles[{}],
   Head[group] === SymmetricGroup || Head[group] === AlternatingGroup,
    Cycles[{}],
   (*PermutationGroup*)Head[group] === PermutationGroup, Cycles[{}],
   (*AbelianGroup*)Head[group] === AbelianGroup, 
   Table[0, {i, Length[group[[1]]]}],
   (*(Zn,+):groupe additif modulo n*)MatchQ[group, {"AddMod", n_}], 0,
   (*(Zn,\[Times]) monoïde ou groupe multiplicatif*)
   MatchQ[group, {"MultMod", n_}], 1,
   (*Cas général:prendre le premier élément de GroupElements*)True, 
   First[GroupElements[group]]]]

(*Exemples d'utilisation:*)
IdentityElement[CyclicGroup[5]]          (*0*)
IdentityElement[DihedralGroup[5]]   (*Cycles[{}]*)
IdentityElement[SymmetricGroup[3]]       (*Cycles[{}]*)
IdentityElement[AlternatingGroup[3]]       (*Cycles[{}]*)
IdentityElement[
 PermutationGroup[{Cycles[{{2, 10}, {4, 11}, {5, 7}}], 
   Cycles[{{1, 4, 3}, {2, 5, 6}}]}]]       (*Cycles[{}]*)
IdentityElement[AbelianGroup[{2, 3, 4}]]     (*{0,0,0}*)
IdentityElement[{"AddMod", 7}]           (*0*)
IdentityElement[{"MultMod", 8}]         (*1*)
```
- `GroupInverse[group, g]` : 