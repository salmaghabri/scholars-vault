## Présentation
Spark est un système de traitement rapide et parallèle. Il fournit des APIs de haut niveau en Java, Scala, Python et R, et un moteur optimisé qui supporte l'exécution des graphes. Il supporte également un ensemble d'outils de haut niveau tels que Spark SQL pour le support du traitement de données structurées, MLlib pour l'apprentissage des données, GraphX pour le traitement des graphes, et Spark Streaming pour le traitment des données en streaming
Spark et Hadoop Spark
peut s'exécuter sur plusieurs plateformes: Hadoop, Mesos, en standalone ou sur le cloud. Il peut également accéder diverses sources de données, comme HDFS, Cassandra, HBase et S3.
![sparlk-1.png](sparlk-1.png)

## L'API de Spark
A un haut niveau d'abstraction, chaque application Spark consiste en un programme driver qui exécute la fonction main de l'utilisateur et lance plusieurs opérations parallèles sur le cluster. L'abstraction principale fournie par Spark est un RDD (Resilient Distributed Dataset), qui représente une collection d'éléments partitionnés à travers les noeuds du cluster, et sur lesquelles on peut opérer en parallèle. Les RDDs sont créés à partir d'un chier dans HDFS par exemple, puis le transforment. Les utilisateurs peuvent demander à Spark de sauvegarder un RDD en mémoire, lui permettant ainsi d'être réutilisé e cacement à travers plusieurs opérations parallèles.

Les RDDs supportent deux types d'opérations: les transformations, qui permettent de créer un nouveau Dataset à partir d'un Dataset existant les actions, qui retournent une valeur au programme driver après avoir exécuté un calcul sur le Dataset. Par exemple, un map est une transformation qui passe chaque élément du dataset via une fonction, et retourne un nouvel RDD représentant les résultats. Un reduce est une action qui agrège tous les éléments du RDD en utilisant une certaine fonction et retourne le résultat nal au programme. Toutes les transformations dans Spark sont lazy, car elles ne calculent pas le résultat immédiatement. Elles se souviennent des transformations appliquées à un dataset de base (par ex. un chier). Les transformations ne sont calculées que quand une action nécessite qu'un résultat soit retourné au programme principal. Cela permet à Spark de s'exécuter plus e cacement.
![[sparlk-2.png]]
![[sparlk-4.png]]
![[sparlk-5.png]]
![[sparlk-6.png]]
![[sparlk-7.png]]
## Spark Streaming
Spark est connu pour supporter également le traitement des données en streaming. Les données peuvent être lues à partir de plusieurs sources tel que Kafka, Flume, Kinesis ou des sockets TCP, et peuvent être traitées en utilisant des algorithmes complexes. Ensuite, les données traitées peuvent être stockées sur des systèmes de chiers, des bases de données ou des dashboards. Il est même possible de réaliser des algorithmes de machine learning et de traitement de graphes sur les ux de données.
![[sparlk-8.png]]