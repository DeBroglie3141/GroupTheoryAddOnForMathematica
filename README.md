# Extension de Théorie des Groupes pour Mathematica (Wolfram)

Oscar Eatwell, Jules Charlier et Rafael Salavarria Lorenzoni

## Composants : 

- Fonctions (Affichage des tables de Cayley, Attributs du groupe (inverse de chaque élément, élément neutre, opération élémentaire, ordre), sous-groupes, cosets, isomorphismes, représentation classique, ordre des éléments, visualisation du groupe, groupes quotient, etc.), champs
- Exemples (Collisions de particules, RSA, Rubik's Cube, jeu de taquin, Modèle standard, Insolvabilité des plynômes degré 5, etc.)
- Documentation (Théorie des groupes, tutoriels, références, etc.)


## Liste des fonctions 

- `GroupIdentity[group]` : renvoie l'élément neutre du groupe `group`
- `GroupInverse[group, g]` : renvoie l'inverse de `g`, `g` étant un élément du groupe `group`
- `CayleyMultiplicationTable[group]` : renvoie la table de Cayley du groupe `group`, ex `Z4 = {4, Plus}`. (venant compléter `GroupMultiplicationTable[group]` en permettant d'afficher des tables de groupes non abéliens)

## Réalisations

| Fonctionnalité | Statut |
|-----------------|--------|
| `GroupIdentity[group]` | ✖️ Non complété |
| `GroupInverse[group, g]` | ✖️ Non complété |
| `CayleyMultiplicationTable[group]` | ✖️ Non complété |

## Installation

(rien ici pour l'instant)