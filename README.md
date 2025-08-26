# Extension de Théorie des Groupes pour Mathematica (Wolfram)

Oscar Eatwell, Jules Charlier, Jawad Ben Brahim et Rafael Salavarria Lorenzoni

## Composants : 

- Fonctions (Affichage des tables de Cayley pour des groupes non abéliens et des monoïdes, des attributs du groupe ou du monoïde (inverse de chaque élément, élément neutre, opération élémentaire, ordre), cosets, isomorphismes, représentation classique, ordre des éléments, visualisation du groupe, groupes quotient, etc.), champs
- Exemples (Collisions de particules, RSA, Rubik's Cube, jeu de taquin, Modèle standard, Insolvabilité des polynômes degré 5, etc.)
- Documentation (Théorie des groupes, tutoriels, références, etc.)


## Liste des fonctions 

- `IdentityElement[group]` : renvoie l'élément neutre du groupe `group`, ou du monoïde `group`
- `GroupInverse[group, g]` : renvoie l'inverse de `g`, `g` étant un élément du groupe `group`
- `CayleyMultiplicationTable[group]` : renvoie la table de Cayley du groupe `group`, ex `Z4 = {"AddMod", 4}`. (venant compléter `GroupMultiplicationTable[group]` en permettant d'afficher des tables de groupes non abéliens et de monoïdes), ajout d'une case en haut à gauche avec le symbole de l'opération
- Affichage groupes dihédraux.

## Réalisations

| Fonctionnalité | Statut |
|-----------------|--------|
| `IdentityElement[group]` | ✅ Complété |
|`IsIdentityByGroup[group, elem]`|✅ Complété|
|`IsIdentity[list, elem, operation]`|✅ Complété|
| `GroupInverse[group, g]` | ✖️ Non complété |
| `CayleyMultiplicationTable[group]` | ✅ Complété |

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

(*Exemples d'utilisation*)
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
- `CayleyMultiplicationTable[group]` : 
```wl
CayleyMultiplicationTable[group_] := 
 Module[{elements, Operation, n, i, table, 
   opSymbol},(*Détection du type de groupe et définition de l'opératio\
n*) Which[(*Groupe abélien*)Head[group] === AbelianGroup, n = group[[1]];
   elements = Tuples[Table[Range[0, n[[i]] - 1], {i, Length[n]}]];
   Operation[x_, y_] := Mod[x + y, n];
   opSymbol = "+ (mod " <> ToString[n] <> ")",(*Groupe cyclique*)
   Head[group] === CyclicGroup, elements = Range[0, group[[1]] - 1];
   Operation[x_, y_] := Mod[x + y, group[[1]]];
   opSymbol = 
    "+ (mod " <> ToString[group[[1]]] <> 
     ")",(*Groupes modulaires personnalisés*)
   MatchQ[group, {_String, _Integer}], 
   elements = Range[0, group[[2]] - 1];
   Operation[x_, y_] := 
    Which[group[[1]] == "AddMod", Mod[x + y, group[[2]]], 
     group[[1]] == "MultMod", Mod[x*y, group[[2]]]];
   opSymbol = 
    Which[group[[1]] == "AddMod", 
     "+ (mod " <> ToString[group[[2]]] <> ")", group[[1]] == "MultMod",
      "\[Times] (mod " <> ToString[group[[2]]] <> 
      ")"],(*Domaine Booléen*)MatchQ[group, {_String, _String}], 
   elements = {True, False};
   Operation[x_, y_] := 
    Which[group[[1]] == "And", And[x, y], group[[1]] == "Or", Or[x, y],
      group[[1]] == "Xor", Xor[x, y]];
   opSymbol = group[[1]],(*Groupes de permutations*)
   MemberQ[{DihedralGroup, SymmetricGroup, AlternatingGroup, 
     PermutationGroup}, Head[group]], elements = GroupElements[group];
   Operation[x_, y_] := PermutationProduct[x, y];
   opSymbol = "\[SmallCircle]"];
  (*Construction de la table avec en-têtes*)
  table = Prepend[
    MapIndexed[Prepend[#, elements[[First[#2]]]] &, 
     Table[Operation[elements[[i]], elements[[j]]], {i, 
       Length[elements]}, {j, Length[elements]}]], 
    Prepend[elements, 
     Style[opSymbol, Bold]]  (*Case supérieure gauche avec couleur*)];
  Grid[table, Frame -> All, Spacings -> {2, 2}]]

(*Exemples d'utilisation*)
CayleyMultiplicationTable[AbelianGroup[{2, 4}]]
CayleyMultiplicationTable[CyclicGroup[5]]
CayleyMultiplicationTable[{"AddMod", 7}]
CayleyMultiplicationTable[{"MultMod", 8}]
CayleyMultiplicationTable[DihedralGroup[3]]
CayleyMultiplicationTable[{"And", "BooleanDomain"}]
```
- `IsIdentityByGroup[group, elem]` :
```wl
IsIdentityByGroup[group_, elem_] := (elem === IdentityElement[group])

(*Exemples d'utilisation*)
IsIdentityByGroup[CyclicGroup[5], 0]          (*True*)
IsIdentityByGroup[DihedralGroup[5], Cycles[{}]]  (*True*)
IsIdentityByGroup[SymmetricGroup[3], Cycles[{}]]       (*True*)
IsIdentityByGroup[AlternatingGroup[3], Cycles[{}]]       (*True*)
```

- `IsIdentity[list, elem, operation]` :
```wl
IsIdentity[list_List, elem_, operation_ : Plus] /; 
  MemberQ[list, elem] := xAllTrue[list, ((operation[elem, #]) == # &)]

(*Exemple d'utilisation*)
IsIdentity[Range[0, 11], 0, Plus] (*True*)
IsIdentity[{2,4,7,10,24}, 4, Plus] (*False*)
```