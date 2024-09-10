- il s'agit de reconnaître les "types" des "mots" lus
# Rôle
parcourir le programme source de la gauche vers la droite et le décomposer en mots, chaque mot ayant une signification particulière 
- phase la plus simple conceptuellement mais la plus coûteuse en temps (lecture du fichier sources par caractère ou blocs de caractères)
# Definitions
## Une unité lexicale
une suite de caractères qui a une signification collective.
**Exemples :**
• OPREL: opérateurs relationnels (=,<,<=, > etc…) 
• IDENT: toto, X_1, proc2, tab. 
• MC_IF: if • MC_WHILE: while 
• PAR_OUV: ( 
• NB: nombre entier.

## Un modèle
- une règle associée à une unité lexicale qui décrit l'ensemble des chaînes du programme qui peuvent correspondre à cette unité lexicale
**Spécification**: expressions régulières

## Un lexème
- toute suite de caractère du programme source qui concorde avec le modèle d'une unité lexicale.
**– Exemples :**
- ==IDENT en langage C== 
	- a pour modèle : <ins>toute suite non vide de caractères composée de chiffres, lettres ou du symbole "_" et qui commencent par une lettre.</ins> 
	- Des lexèmes correspondants à ce modèle: X_1, proc_2, etc.
	- ident= (a|b|c|d|e|f|g|h|i|j|k|l|m|n|o|p|q|r|s|t|u|v |w|x|y|z|A|B|C|D|E|F|G|H|I|J|K|L|M|N|O|P|Q |R|S|T|U |V|W|X|Y|Z) (a|b|c|d|e|f|g|h|i|j|k|l|m|n|o|p|q|r|s|t|u|v|w|x|y|z|A|B |C|D|E|F|G|H|I|J|K|L|M |N|O|P|Q|R|S|T|U|V|W|X|Y|Z|0|1|2|3|4|5|6|7|8|9|_)*

- ==NOMBRE (entier signé)== a pour modèle : <ins> toute suite non vide de chiffres précédée éventuellement d'un seul caractère parmi {+,-}</ins>. Lexèmes possibles : -12832, 15
![[Analyse lexicale-1.png]]
## Attributs d’une unité lexicale
- toute information supplémentaire, inutile pour **l'analyseur syntaxique** mais utile pour les autres phases du compilateur
 **Exemples:**
 - ==OPREL:== l'analyseur syntaxique a juste besoin de savoir que cela correspond à l'unité lexicale OPREL (opérateur relationnel). C'est seulement lors de la génération de code que l'on aura besoin de distinguer < de >= (par exemple).
 - ==IDENT==: **l'analyseur syntaxique** a juste besoin de savoir que c'est l'unité lexicale IDENT. Mais le générateur de code aura besoin de l'adresse de la variable correspondant à cet identificateur. L'analyseur sémantique aura aussi besoin du type de la variable pour vérifier que les expressions sont sémantiquement correctes.
 -  ==NB==: l'analyseur syntaxique a juste besoin de savoir que c'est l'unité lexicale NB. Mais le générateur de code aura besoin de la valeur du nombre
# Mise en œuvre d’un analyseur lexical
- Une unité lexicale peut (la plupart du temps) être exprimée sous forme de définitions régulières
- Un langage régulier peut être reconnu par un automate à états fini.
- il suffit d'écrire un programme simulant l'automate reconnaissant les unités lexicales.
- une unité lexicale est reconnue -> envoyée à l'analyseur syntaxique -> la traite -> repasse la main a l'analyseur lexical jusqu'à :
	- tomber sur une erreur 
	-  le programme source soit traité en entier


# Fragment d‘un analyseur lexical pour le langage Pascal (en C)
```c
c = getchar();
switch (c) {
case ':' : c=getchar();
if (c== '=') { 
unite_lex = AFFECTATION; c= getchar(); 
} else unite_lex = DEUX_POINTS; 
	break; case '<' : unite_lex := OPREL;
	 c= getchar();
	  if (c=='=') { 
	attribut = INFEG; c = getchar();
	} 
	else attribut := INF ; break;
	 case .....

```

# Autre solution: copier le travail de l’automate
```flex
void Etat1() { char c; c= getchar(); swicth(c) { case ':' : Etat2(); break; case '<' : Etat5(); break; case 'a' : Etat57(); break; ... default : ERREUR(); } } void Etat26() ...
```


