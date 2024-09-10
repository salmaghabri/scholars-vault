# Analyse SLR
## Principe 
construire un arbre de dérivation du bas (les feuilles, i.e les unités lexicales) vers le haut (la racine, i.e l'axiome de départ).
## Le modèle général utilisé 
décalages- réductions
### decalage
décaler d'une lettre le pointeur sur le mot en entrée
### reduction
réduire une chaîne (suite consécutive de terminaux et non terminaux à gauche du pointeur sur le mot en entrée et finissant sur ce pointeur) par un non-terminal en utilisant une des règles de production
## Remarque:
Méthodes d’analyses syntaxiques ascendantes: LR (Left scanning Rightmost derivation)
• Etant donné un mot, elles construisent une dérivation la plus à droite mais inversée. Dans l’exemple introductif: aacbaacbcbcbcbacbc ⇐ aaSbaacbcbcbcbacbc ⇐ aaSbaaSbcbcbcbacbc ⇐ ⋯ ⇐ aaSbSbacb𝑐 ⇐ aSbacb𝑐 ⇐ aSbaSb𝑐 ⇐ aSbaSbS ⇐ aSbS ⇐ 𝑆
# Analyse LR
## Table d'analyse LR
- Les lignes: numéros **d’états** de l’automate à pile
- nous dit ce qu'il faut faire quand on lit une lettre a et qu'on est dans un état i: 
	- on décale:  Dans ce cas, on **empile la lettre lue** et on va dans un autre état j. Ce qui sera noté dj
	- on réduit  par la règle de production numéro:  on remplace la chaîne en sommet de pile (qui correspond à la partie droite de la règle numéro p) par le non-terminal de la partie gauche de la règle de production, et on va dans l'état j qui dépend du non-terminal en question. On note cela par rp
-  soit on accepte le mot. Ce qui sera noté ACC 
- soit c'est une erreur. Case vide
## Un 0-item (ou LR(0)-item ou plus simplement item) 
- une production de la grammaire avec un "." quelque part dans la partie droite. Par exemple (sur la gram ETF) : E -> E . + T ou encore T-> F. ou encore F -> . ( E )
## Fermeture d'un ensemble d'items I 
1.  Mettre chaque item de I dans Fermeture(I) 
2. Pour chaque item i de Fermeture(I) de la forme 𝐴 → 𝛼 ∙ 𝐵𝛽 
	 Pour chaque production 𝐵 → 𝛾
	  rajouter l'item 𝐵 →∙ 𝛾 dans Fermeture(I)
3. Recommencer 2 jusqu'à ce qu'on n'ajoute rien de nouveau
---

1.  Mettre chaque item de I dans Fermeture(I)
2. chercher les productions ayant 𝐵 dans le membre droit et ajouter  𝐵 →∙ 𝛾
3. répéter 2 tant qu'il y a des . avant des non-terminaux
## Transition par X d'un ensemble d'items I 
𝛿 (𝐼, 𝑋) = ==𝑓𝑒𝑟𝑚𝑒𝑡𝑢𝑟𝑒== 𝑑𝑒 𝑡𝑜𝑢𝑠 𝑙𝑒𝑠 𝑖𝑡𝑒𝑚𝑠 𝐴 → 𝛼𝑋 ∙ 𝛽 𝑜ù 𝐴 → 𝛼 ∙ 𝑋𝛽 est dans I
𝑋: terminal, non-terminal ou $

## Collection des items d'une grammaire 
- les états de l'automate
Rajouter un nouvel axiome S' avec la production S' → S 
1. Mettre dans l'item I0 la Fermeture({S' → .S}) 
2.  Mettre I0 dans Collection 
3. Pour chaque I dans Collection faire 
		Pour chaque X tel que 𝛿(I, X) est non vide 
			ajouter ce 𝛿(I, X) dans Collection 
		Fin pour
4. Recommencer 3 jusqu'à ce qu'on n'ajoute rien de nouveau

## Construction de la table d'analyse SLR (Simple LR):
1.  Construire la collection d'items {I0, ... In} 
2.  l'état i est construit à partir de Ii :
		a) pour chaque 𝛿 (Ii , a) = Ij : 
				mettre décalage par j dans la case M[i,a] 
		b) pour chaque 𝛿(Ii , A) = Ij :
			 mettre aller à j dans la case M[i,A] 
		c) pour chaque 𝐴 → 𝛼 ∙ contenu dans Ii :
			 pour chaque a de SUIVANT(A) faire 
				 mettre rp (p: numéro de la règle 𝐴 → 𝛼) dans la case M[i,a]
## Conclusion
cette méthode permet d'analyser plus de grammaires que la méthode descendante (car il y a plus de grammaires SLR que LL)
• dans cette méthode d'analyse, ça n'a strictement aucune importance que la grammaire soit récursive à gauche, même au contraire, on préfère.
• Les grammaires ambigües provoquent des conflits:
– conflit décalage/réduction : on ne peut pas décider à la lecture du terminal a s'il faut réduire une production ou décaler le terminal
– conflit réduction/réduction : on ne peut pas décider à la lecture du terminal a s'il faut réduire par une production ou par une autre

# Analyse LR(1)
- La construction LR(1) s’appuie sur des items de la forme:
	𝐴 → 𝛼 ∙ 𝛽, 𝑎 
	voulant dire : « j’ai reconnu un mot dérivé de 𝛼 , il me reste à reconnaître un mot dérivé de 𝛽 et à vérifier que le lexème suivant est a pour pouvoir affirmer avoir reconnu un mot dérivé de A».
	– 𝐴 → 𝛼 ∙ 𝛽: noyau de l’item 
	– a: look-ahead ou suivant contextuel
- L’automate LR(1) aura (beaucoup) plus d’états que l’automate LR(0).
## table d'analyse
Fermeture(I)
![](Analyse%20ascendante-1.png)

# Analyse LALR: Look Ahead LR
- On construit l'automate d'analyse LR et on regroupe les états ayant un ≪ noyau ≫ commun (le noyau d’un état étant l’ensemble des parties gauches des items LR(1) qu’il contient, i.e. sans le lookahead, i.e. des items LR(0))
-  Si aucun conflit ne se produit, la grammaire est LALR(1)
## Construction des tables LALR
- On construit l'automate d'analyse LR et on regroupe les états ayant un ≪cœur≫ commun (les mêmes items indépendamment des terminaux de prévision) 
- Si aucun conflit ne se produit, la grammaire est LALR(1)
## Automate LALR(1) 
-  On peut construire la table LALR sans passer par les tables LR. On construit alors l’automate LALR(1) 
-  Pour construire l’automate LALR(1), on tient compte des informations sur les suivants comme pour la construction de l’automate LR(1). A chaque création d’état, s’il existe un autre état qui a le même noyau que l’état en cours de création alors on fusionne les deux états en considérant l’union des ensembles de suivants.
# Conclusion
-  Le transfert LR(1) vers LALR(1) peut engendrer des conflits, mais en général ce n’est pas le cas → popularité des parseurs LALR(1) (Comme YACC, bison, etc.) 
- Les parseurs LALR(1) sont plus efficaces que les parseurs LR(1) (moins d’états) 
- L’ambiguïté provoque toujours des conflits 
- Une grammaire non ambiguë n’est pas forcément LR(1)
