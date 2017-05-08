# A retenir !


## Définitions et méthodologie

### Généralité

On notera $n$ le nombre de sommet(s) du graphe, et $m$ le nombre d'arc(s).

### Les ensembles d'arcs

**Ensemble des arcs incidents** : $U(A)$  
Degré : $d(A)$  
**Ensemble des predecesseurs** : $U^-(A)$  
Degré : $d^-(A)$  
**Ensemble des successeurs** : $U^+(A)$  
Degré : $d^+(A)$

### Chaine
Une chaine est un chemin dont l'extremité initiale se confond avec l'extremité terminale.

### Chemin simple mais pas élémentaire
Un chemin est **simple** lorsqu'il ne passe pas deux fois par le **même arc**.  
Un chemin est **élémentaire** lorsqu'il ne passe pas deux fois par le **même sommet**.  
Un chemin simple mais pas élémentaire est donc un chemin passant une seule fois par les arcs, mais qui revient au moins une fois sur un sommet déjà visité.

*Remarque : cette notion est généralisable pour une chaine ou un cycle.*

### Lemme de König
> On peut toujours extraire d'un chemin, une chaine ou un cycle allant de i à j, un chemin, chaine ou cycle élémentaire allant de i à j.
--- [König](https://fr.wikipedia.org/wiki/D%C3%A9nes_K%C5%91nig)

### Chemin Hamiltonien
Une chemin est **Hamiltonien** s'il passe *une et une seule fois* par *tous* les sommets du graphe.

### Couplage

Un couplage est un ensemble d'arcs tels que deux arcs de cet ensemble n'ont pas d'extremité commune.

### Connexité

#### Types de connexité
* Simple : Il existe une chaine entre les sommets i et j  
* Forte : Il existe un circuit passant par i et j

Une composante fortement connexe *(cfc)* est un ensemble des sommets à la fois descendants et ascendants les un des autres.

*Remarque méthodologique : pendant la recherche des cfc, si un sommet fait parti d'une composante, alors il n'est plus à traiter ensuite.*

#### Connexité et graphe réduit
Une fois que l'on a calculé toutes les *cfc* d'un graphe, on peut construire son **graphe réduit**. Ce dernier va relier toutes les cfc d'un groupe.


##### Graphe réduit et graphe planaire
A partir de ce dernier, on peut essayer de déduire si le graphe initial était **planaire** *(voir plus bas)*.

##### Nombre de composantes connexes
On note $C(G_i)$ le nombre de composantes connexes de $G_i$.
On a la propriété suivante :  

$C(G_i)=\begin{cases}C(G_{i-1})\\C(G_{i-1})-1\end{cases}$

Dans le premier cas, on enleve un cycle à une composante connexe. Dans le second, on détache deux composantes connexes

### Graphe planaire
Graphe dont aucun arc ne s'entrecoupe.

### Graphe biparti, graphe bicolorable
Un graphe biparti vérifie que l'ensemble X de ses sommets peut être partitionné en 2 parties Y et Y'. Y et Y' sont tels que tout arc de X a une extremité dans Y et l'autre dans Y'.

Un graphe est bicolorable si tous ses sommets peuvent être colorés selon deux couleurs, chaque somme relié étant d'une couleur différente. (Les deux couleurs étant le rouge et le vert, le sommet 1 étant colorié par défaut en rouge)

Graphe biparti $\Leftrightarrow$ Graphe bicolorable.

**Propriétés :**
* Ne contient pas de cycle de longueur impaire *(démonstration dans le TD4)*
* Pour chaque sommet rouge *(resp. vert)*, il en existe un chemin de longueur paire *(resp. impaires)* depuis la racine.
* la connexité assure la coloration de tous les sommets lors de l'application de l'algorithme BIPAR.

**Généralisation** :
* Pour tester si un graphe quelconque (non necessairement connexe) est biparti, on appliquera l'algo BIPAR sur chaque composante connexe et on fera un $\land$ sur tous les résultats de l'algo obtenus.

#### Le problème de coloration minimale d'un graphe quelconque
 C'est un problème dit **NP-complet** : nous n'avons pas encore trouvé d'algorithme polynomial pour le résoudre.

### Arbre
#### Définition
Un arbre est un graphe connexe sans cycle.  

Arbre $\Leftrightarrow$ $\exists$ *une et une seule* chaîne élémentaire entre chaque paire de sommets.

#### Démonstration rapide
* Si le graphe est connexe, alors $\forall$ sommet $\exists$ une chaine élémentaire.  
* Si le graphe est sans cycle, alors cette chaine est unique.  

Il existe donc une chaine, unique, entre chaque paire de sommets.

### Stable d'un graphe
Un stable d'un graphe est un sous ensemble s'il n'existe pas d'arc reliant deux sommets de S.


### Noyau

K, sous ensemble de X est dit noyau si :
* tout sommet de K a ses successeurs en dehors de K.
* tout sommet en dehors de K a au moins un de ses successeurs dans K.

#### Propriétés
* Un graphe sans circuit a exactement un noyau.

#### Tips
* Pour trouver un noyau, regarder les sommets qui ont le plus de predecesseurs (pour respecter la deuxième condition).

### Clique

Une clique est un graphe complet sans boucles. Deux types de cliques :
* Graphe orienté : $n(n-1)$ arcs
* Graphe non orienté : $\frac{n(n-1)}{2}$ arrêtes.

*Remarque : seules les cliques de cardinalité 1,2,3 et 4 sont planaires.*

## Coder un graphe en machine

### Matrice d'adjacence

On fait un tableau de taille n, associant 1 à la case (i,j) lorsque le sommet i admet comme successeur le sommet j, 0 sinon.
2 types d'adjacence :
* Indirecte : chaine (non orienté)
* Directe : chemin (orienté)

### Exemple

|      | 1     | 2     | 3     | 4     | 5     |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | 1     | 1     | 1     | 1     | 0     |
| **2** | 1     | 0     | 1     | 0     | 0     |
| **3** | 0     | 0     | 0     | 0     | 0     |
| **4** | 0     | 0     | 0     | 0     | 1     |
| **5** | 0     | 0     | 1     | 1     | 0     |

### File des successeurs

####  Les tableaux $\alpha$ et $\beta$
Méthode très utilisé car elle permet d'accéder en $O(1)$ au successeurs d'un sommet.
On part de la matrice d'adjacence. Pour chaque ligne on met à la suite dans le tableau
$\beta$ ses successeurs. $\beta$ est de taille $m$.  

* $\beta$ :

| i=1     | i=2     | i=3     | i=4     | i=5     |  i=6     |  i=7     | i=8     |  i=9     |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| / 1     | 2     | 3     | 4 \     | / 1     | 3 \     | / 5 \     | / 3     | 4 \     |
*Remarque : les numéros entre "/ \\" correspondent aux successeurs d'un même sommet.
Le sommet 3 n'en ayant aucun, il n'est alors pas représenté.*

Pour pouvoir reconnaitre les groupes à l'intérieur du tableau $\beta$, on va se servir des tableaux $\alpha_1$ (indice i de début de groupe) et $\alpha_2$ (indice i de fin de groupe). Les $\alpha_i$ sont de taille $n$.

* $\alpha_1$ :

|  1  |  5  |  0  |  7  |  8  |
| :-: | :-: | :-: | :-: | :-: |

* $\alpha_2$ :

|  4  |  6  |  0  |  7  |  9  |
| :-: | :-: | :-: | :-: | :-: |

ou encore de manière compacte, avec $\alpha$ de taille $n+1$:

* $\alpha$ :

|  1  |  5  |  5  |  7  |  8  |  10  |
| :-: | :-: | :-: | :-: | :-: | :-: |

*Remarque : pour $\alpha$ un numéro de noeud est associé au premier successeur de ce noeud*


## Quelques formules à savoir

* Nombre d'arrêtes d'un graphe non orienté complet sans boucle de $n$ sommets : $\frac{n(n-1)}{2}$
* Nombre de graphes orientés à $n$ sommets : $2^{n^n}$
* Un graphe connexe a au moins $n-1$ arrêtes *(démonstration dans le TD4)*
* Un graphe sans cycle a au plus $n-1$ arrêtes *(démonstration dans le TD4)*

## Some other tips

Si on a un long exercice, souvent les dernières questions sont simplement des conséquences des autres. Par exemple on peut nous demander de prouver une condition nécessaire, puis la suffisante et en fin d'exo l'équivalence *(cf la deuxième partie du TD5)*.

------------------------

## Algorithmes

### Recherche des descendants d'un sommet

#### Principe général

Le sommet pour lequel on veut chercher les descendants est le sommet $i_0$.
A chaque sommet $i$, on va associer à chaque itération un $n(i)$ et un $p(i)$. $n(i)$ représentera le nombre de sommets qui n'ont pas encore été traités, et $p(i)$ le nombre de d'arcs entre $i_0$ et $i$.  
En fin d'algorithme, les $n(i)$ doivent être à 0 (tous les sommets auront été traités) et les $p(i)$ avec le nombre d'arc qu'il faut pour rejoindre le sommet $i$ à partir du sommet $i_0$ (donc de 0 à n, 0 indiquant que $i$ n'est pas accessible à partir de $i_0$).

#### L'initialisation
Pour chacun des $n$ sommets on va :
* Initialiser les $p(i)$ à 0 : aucun sommet n'a encore été traité.
* Initialiser les $n(i)$ à $d^+(i)$, le nombre de successeurs de $i$.

On a donc une complexité de $O(n)$ à l'initialisation (en supposant que $d^+(i)$ soit connu).

#### La boucle principale
On va itérer tant qu'on a pas parcouru tous les successeurs de $i$. Dans le pire des cas, on va faire $n$ fermetures de sommets ($n_i=0$), et $m$ analyses d'un successeur de $i$ ($n_i\ne0$).

On a donc une complexité de $O(n+m)$ dans notre boucle principale.

##### Propriétés

###### Construction d'une arborescence de $i_0$

La fonction $f$ défini une arborescence $\Leftrightarrow$ $G^f$ est connexe.
On peut le prouver par récurrence qu'à chaque itération :
* soit on rajoute un arc à l'arborescence, donc un graphe connexe avec un sommet de plus
* soit on ne change pas le graphe

###### Tout sommet atteint est descendant de $i_0$

Un sommet atteint $i$ est un sommet dans l'arborescence, qui commence à $i_0$. Ce qui signifie bien qu'il existe un chemin dans l'arborescence de i0 à $i$. On dira donc que $i$ est un descendant de i0.

#### Fin de l'algorithme
Prouver la fin de l'algo revient à prouver que la boucle principale se termine. Dans celle ci, pour chaque sommet $i$, on fait $d^+(i)$ analyse d'un successeur. On aura donc :
$\sum_{i=1}^{n}d^+(i)=m$ analyses.
Ainsi, le nombre d'itérations de la boucle principale est majoré par $(n+m)$.
