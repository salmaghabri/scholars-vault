---
resource: http://www.fsr.ac.ma/DOC/cours/informatique/rahmani/Compilation%204.pdf
---
# Principe:
- construire l'arbre de dérivation du haut (la racine, c'est à dire l'axiome de départ) vers le bas (les feuilles, c'est à dire les unités lexicales).
# Fonction Premier
## But
trouver **tous** les symboles terminaux pouvant apparaitre en premier dans une chaine dérivable à partir de alpha (chaîne quelconque de terminaux et/ou de non-terminaux)
Premier(α)={a | α ֜=>∗ aβ}
## Algo
- si a est un terminal, alors: PREMIER ( a) = { a}
- de même: PREMIER ( ε) = { ε }
-  si A ->b α  , alors b est un élément PREMIER(A)
- si A-> Bα , alors PREMIER(B) С PREMIER(A)
- si B peut produire la chaîne vide, alors ((PREMIER(B)\{ ε}) U PREMIER ( α)) С PREMIER(A)
**D’une autre manière,**
1. PREMIER( α) inclut toujours l’ensemble PREMIER du premier symbole de α.
2. Si celui-ci peut produire ε, il inclut aussi l’ensemble PREMIER du second symbole de α,
3. Si tous les symboles de α peuvent produire ϵ alors PREMIER( α) contient ε

## Exemples
![[Analyse descendante-1.png]]
![[Analyse descendante-2.png]]
# Fonction Suivant
## But
trouver **tous** les symboles terminaux pouvant apparaître à côté d’un symbole non-terminal donné.
![[Analyse descendante-3.png]]
## Algo
- Nous appliquons les règles suivantes jusqu’à ce qu’aucun terminal ne puisse être ajouté aux ensembles SUIVANT.
-  mettre $ dans SUIVANT de l’axiome.
- s’il y a une production : A -> α B β, le contenu de PREMIER (β) \{ ε} est ajouté à SUIVANT(B)
-  s’il y a une production : A ->α B ou A ->α B β tq β ->* ε les éléments de SUIVANT(A) sont ajoutés à SUIVANT(B)
 
## Exemples
![[Analyse descendante-4.png]]
![[Analyse descendante-5.png]]

# Construction de la table d'analyse
## Définition 
-  un tableau M à deux dimensions qui indique pour chaque symbole non-terminal A et chaque symbole terminal a ou symbole $ la règle de production à appliquer.
## Algorithme
Pour chaque règle A→α faire
- pour tout a ϵ premier(α ), ajouter la règle A→α dans la case M[A,a]
-  si ε ϵ premier(α ), alors pour chaque bϵ suivant(A) ajouter la règle A→α dans M[A, b]


données : mot m, table d'analyse M 
initialisation de la pile : $S 
Ps: pointeur source pointant sur la première lettre du mot m.
repeter 
	Soit X le symbole en sommet de pile
	Soit a la lettre pointée par ps
	**Si** X est un non terminal
	alors Si M[X,A]=X → Y1 Y2 … Yn 
		alors
		 enleve
r X de la pile 
		 mettre Yn puis Yn-1 puis.. Y1 dans la pile
		 sinon ERREUR
		 finsi
	**sinon** si Si X=$
		alors Si a=$
			alors ACCEPTER
			Sinon ERREUR
			finsi
		Sinon Si X=a
			alors enlever X de la pile
				avancer ps
			sinon ERREUR
			finsi
		finsi
	finsi
jusqu'a ERREUR ou ACCEPTER



# Algorithme de l’analyseur syntaxique descendant

# Grammaire LL(1)
## définition
- une grammaire pour laquelle la table d'analyse décrite précédemment n'a aucune case définie de façon multiple
- Le terme "LL(1)" signifie :
	- on parcourt l'entrée de gauche à droite (L pour Left to right scanning), 
	- on utilise les dérivations les plus à gauche (L pour Leftmost derivation), 
	- et qu'un seul symbole de prévision est nécessaire à chaque étape nécessitant la prise d'une décision d'action d'analyse.
## Théorème
>Une grammaire ==ambigüe== ou ==récursive à gauche== ou ==non factorisée à gauche== n'est pas LL(1)
## Récursivité à gauche immédiate
### définition 
- Une grammaire est immédiatement récursive à gauche si elle contient un non-terminal A tel qu'il existe une règle A→A α où α est une chaîne quelconque
### Exemple
A→A α |β
### Elimination
A→βA’ 
A’→αA’ |ε
![[Analyse descendante-6.png]]
## Récursivité à gauche non immédiate
### définition 
>Une grammaire est récursive à gauche si elle contient un non terminal A tel qu'il existe une dérivation A =>∗ 𝐴α où α est une chaîne quelconque.
### Exemple
𝑆 →𝐴𝑎|𝑏 
𝐴 →𝐴𝑐|𝑆𝑑|𝑑
![[Analyse descendante-8.png]]
### Elimination
![[Analyse descendante-7.png]]
L’algorithme précédent ne marche que pour les grammaires propres
### Grammaire propre
ne contient aucune production A-> epsilon
### Elimination des règles A -> epsilon
On rajoute une règle dans laquelle le A est remplacé par epsilon , ceci pour chaque A apparaissant en partie droite d'une production, et pour chaque A d'un A -> epsilon
### exemple
S -> a Tb|aU
T -> bTaTA| epsilon 
U -> a U | b 
devient 
S -> a Tb|ab|aU
T -> bTaTA|baTA|bTaA|baA
U -> a U | b
## Factorisation à gauche
### définition 
La grammaire suivante n’est pas factorisée:
![[Analyse descendante-9.png]]
### Exemple
![[Analyse descendante-11.png]]
### Elimination
![[Analyse descendante-10.png]]
