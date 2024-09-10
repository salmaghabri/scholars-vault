# definition compilation
- un logiciel particulier qui traduit un programme écrit dans un langage de haut niveau (par le programmeur) en instructions exécutables (par un ordinateur).
# Chaîne de développement d'un programme
![[Intro-1.png]]
# Compilateurs vs interpréteurs
- un interprèteur exécute lui même **au fur et à mesure** les opérations spécifiées par le programme source, sans produire de code cible:
	-  I lanalyse une instruction après l'autre puis l'exécute immédiatement.
	-  A l'inverse d'un compilateur, il travaille simultanément sur le programme et sur les données.
- L'interpréteur **doit être présent** sur le système à chaque fois que le programme est exécuté, ce qui n'est pas le cas avec un compilateur.
- les interpréteurs sont assez petits,  ce qui n'est pas le cas avec un compilateur
- Avec les interpréteurs, on ne peut pas cacher le code (et donc garder des secrets de fabrication): toute personne ayant accès au programme peut le consulter et le modifier
- Les langages interprétés sont souvent plus simples à utiliser et tolèrent plus les erreurs de codage que les langages compilés
# Langages P-codes
- sont à mi-chemin de l'interprétation et de la compilation.
- code source traduit (compilé) dans une forme binaire compacte (du pseudo-code ou p-code) qui n'est pas encore du code machine.
- Lorsque l'on exécute le programme, ce P-code est interprété.
- relativement petits et rapides, si bien que le p-code peut s'exécuter presque aussi rapidement que du binaire compilé.
# Structure d'un compilateur
![[Intro-2.png]]
## une phase d'analyse (Partie frontale)
– [[Analyse lexicale]]: reconnaissance des variables, instructions, opérateurs, etc.
– [[Analyse syntaxique]]: élaboration de la structure syntaxique du programme
– [[Analyse sémantique]]: vérification de certaines propriétés sémantiques.
## une phase de synthèse et de production(Partie finale)
- production du code cible