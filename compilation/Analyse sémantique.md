# Définition Dirigée par la Syntaxe
 un formalisme permettant d'associer des actions à une production d'une règle de grammaire.
 # attributs
- Chaque symbole de la grammaire (terminal ou non) possède un ensemble d'**attributs** . 
- Chaque règle de production de la grammaire possède un ensemble de règles sémantiques qui permettent de calculer la valeur des attributs associés aux symboles apparaissant dans la production.
- On notera X.a l'attribut a du symbole X. S'il y a plusieurs symboles X dans une production, on les notera 𝑋1, 𝑋2, ⋯ 𝑋𝑛 , et 𝑋0 s'il est en partie gauche.
# Arbre syntaxique décoré
un arbre syntaxique sur les noeuds duquel on rajoute la valeur de chaque attribut.
![](Analyse%20sémantique-1.png)
# Attributs synthétisés
Un attribut est dit synthétisé lorsqu'il est calculé pour le non terminal de la partie gauche en fonction des attributs des non terminaux de la partie droite.
##### Sur l'arbre décoré : 
la valeur d'un attribut en un nœud se calcule en fonction des attributs de ses fils. 
##### le calcul de l'attribut se fait
des feuilles vers la racine
##### peuvent être facilement évalués lors d'une analyse 
 ascendante (donc par exemple avec yacc/bison). <ins>Mais pas du tout lors d'une analyse descendante.</ins>
 ##### exemple
 ![](Analyse%20sémantique-2.png)
# Attributs hérités
- Un attribut est dit hérité lorsqu'il est calculé à partir des attributs du **non terminal de la partie gauche**, et éventuellement des attributs d'autres non terminaux de la partie droite.
##### Sur l'arbre décoré : 
la valeur d'un attribut à un noeud se calcule en fonction des attributs **des frères et du père**. 
##### le calcul de l'attribut se fait 
**de la racine vers les feuilles**. 
#####  peuvent être facilement évalués lors d'une analyse 
descendante Si les attributs d'un nœud donné ne dépendent pas des attributs de ses **frères droits**,  mais pas lors d'une analyse ascendante.
##### exemple
Calcul du niveau d'imbrication des ) dans un système de parenthèses bien formé.
![](Analyse%20sémantique-3.png)
# Graphe de dépendances 
• Une DDS peut utiliser à la fois des attributs synthétisés et des attributs hérités 
→ Il faut établir un ordre d'évaluation des règles sémantiques. 
→ On construira ce qu'on appelle un graphe de dépendances. 
• On appelle **graphe de dépendances** le graphe orienté représentant les interdépendances entre les divers attributs. Le graphe a pour sommet chaque attribut. Il y a un arc de a à b ssi le calcul de b dépend de a.
# Evaluation des attributs 
##  Après l'analyse syntaxique 
-  On peut faire le calcul des attributs indépendamment de l'analyse syntaxique : lors de l'analyse syntaxique, on construit (en dur) l'arbre syntaxique, puis ensuite, lorsque l'analyse syntaxique est terminée, le calcul des attributs s'effectue sur cet arbre par des parcours de cet arbre (avec des aller-retours dans l'arbre suivant l'ordre d'évaluation des attributs). 
- Cette méthode est très couteuse en mémoire (stockage de l'arbre). 
- Mais l'avantage est que l'on n'est pas dépendant de l'ordre de visite des sommets de l'arbre syntaxique imposé par l'analyse syntaxique (une analyse **descendante impose un parcours en profondeur** du haut vers le bas, de la gauche vers la droite, une analyse ascendante un parcours du bas vers le haut, ...). 
-  On peut également construire l'arbre en dur pour une sous-partie seulement du langage. 
##  Pendant l'analyse syntaxique 
– On peut évaluer les attributs en même tant que l'on effectue l'analyse syntaxique. 
– Dans ce cas, on utilisera une pile pour conserver les valeurs des attributs, cette pile pouvant être la même que celle de l'analyseur syntaxique, ou une autre. 
– Cette fois-ci, l'ordre d'évaluation des attributs est tributaire de l'ordre dans lequel les noeuds de l'arbre syntaxique sont "crées" par la méthode d'analyse.
## exemple
### Attributs synthétisés
### Attribut hérités 

# Conclusion
Mais, vu que les attributs synthétisés s'évaluent avec une analyse ascendante et les attributs hérités avec une analyse descendante, comment on fait quand on a à la fois des attributs synthétisés et hérités ?
- Souvent, on peut utiliser des attributs synthétisés qui font la même chose que les hérités que l'on voulait. Exemple : compter le nombre de bit à 1 dans un mot binaire.
-  Parfois, il est nécessaire de modifier la grammaire 
-  il existe des méthodes automatiques qui transforment une DDS avec des attributs hérités et synthétisés en une DDS équivalente n'ayant que des attributs synthétisés 
- Une autre idée peut être de faire plusieurs passes d'analyses, suivant le graphe de dépendances de la DDS.
