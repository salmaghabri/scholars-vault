### Apache HBase
#### Présentation
HBase est sgbd distribué, non-relationnel et orienté colonnes, développé au-dessus du système de fichier HDFS. Il permet un accès aléatoire en écriture/lecture en temps réel à un très grand ensemble de données.
#### Modèle de données
Le modèle se base sur six concepts, qui sont :
- **Table** : dans HBase les données sont organisées dans des tables. Les noms des tables sont des chaînes de caractères.
- **Row** : dans chaque table, les données sont organisées dans des lignes. Une ligne est identifiée par une clé unique (_RowKey_). La Rowkey n’a pas de type, elle est traitée comme un tableau d’octets.
- **Column Family** : Les données au sein d’une ligne sont regroupées par _column family_. Chaque ligne de la table a les mêmes column families, qui peuvent être peuplées ou pas. Les column families sont définies à la création de la table dans HBase. Les noms des column families sont des chaînes de caractères.
- **Column qualifier** : L’accès aux données au sein d’une column family se fait via le _column qualifier_ ou _column_. Ce dernier n’est pas spécifié à la création de la table mais plutôt à l’insertion de la donnée. Comme les rowkeys, le column qualifier n’est pas typé, il est traité comme un tableau d’octets.
- **Cell** : La combinaison du RowKey, de la Column Family ainsi que la Column qualifier identifie d’une manière unique une cellule. Les données stockées dans une cellule sont appelées les _valeurs_ de cette cellule. Les valeurs n’ont pas de type, ils sont toujours considérés comme tableaux d’octets.
- **Version** : Les valeurs au sein d’une cellule sont versionnés. Les versions sont identifiés par leur _timestamp_ (de type long). Le nombre de versions est configuré via la Column Family. Par défaut, ce nombre est égal à trois.

![Modèle de données de HBase](https://insatunisia.github.io/TP-BigData/img/tp4/hbase-model.jpg)
Les données dans HBase sont stockées sous forme de _HFiles_, par colonnes, dans HDFS. Chaque _HFile_ se charge de stocker des données correspondantes à une _column family_ particulière.

![HFile](https://insatunisia.github.io/TP-BigData/img/tp4/hbase-hfile.png)

Autres caractéristiques de HBase:

- HBase n'a pas de schéma prédéfini, sauf qu'il faut définir les familles de colonnes à la création des tables, car elles représentent l'organisation physique des données
- HBase est décrite comme étant un magasin de données clef/valeur, où la clef est la combinaison (_row_-_column family_-_column_-_timestamp_) représente la clef, et la _cell_ représente la valeur.

#### Architecture[¶](https://insatunisia.github.io/TP-BigData/tp4/#architecture "Permanent link")

Physiquement, HBase est composé de trois types de serveurs de type Master/Worker.

- **Region Servers**: permettent de fournir les données pour lectures et écritures. Pour accéder aux données, les clients communiquent avec les RegionServers directement.
- **HBase HMaster** : gère l'affectation des régions, les opérations de création et suppression de tables.
- **Zookeeper**: permet de maintenir le cluster en état.

Le DataNode de Hadoop permet de stocker les données que le Region Server gère. Toutes les données de HBase sont stockées dans des fichiers HDFS. Les RegionServers sont colocalisés avec les DataNodes.

Le NameNode permet de maintenir les métadonnées sur tous les blocs physiques qui forment les fichiers.

![Architecture de HBase](https://insatunisia.github.io/TP-BigData/img/tp4/hbase-archi.png)

Les tables HBase sont divisées horizontalement, par _row_ en plusieurs **Regions**. Une region contient toutes les lignes de la table comprises entre deux clefs données. Les regions sont affectées à des noeuds dans le cluster, appelés _Region Servers_, qui permettent de servir les données pour la lecture et l'écriture. Un _region server_ peut servir jusqu'à 1000 régions.

Le HBase Master est responsable de coordonner les _region servers_ en assignant les régions au démarrage, les réassignant en cas de récupération ou d'équilibrage de charge, et en faisant le monitoring des instances des _region servers_ dans le cluster. Il permet également de fournir une interface pour la création, la suppression et la modification des tables.

HBase utilise Zookeeper comme service de coordination pour maintenir l'état du serveur dans le cluster. Zookeeper sait quels serveurs sont actifs et disponibles, et fournit une notification en cas d'échec d'un serveur.

### Traitement de données avec Spark[¶](https://insatunisia.github.io/TP-BigData/tp4/#traitement-de-donnees-avec-spark "Permanent link")

Installé sur le même cluster que HBase, Spark peut être utilisé pour réaliser des traitements complexes sur les données de HBase. Pour cela, les différents Executors de Spark seront co-localisés avec les region servers, et pourront réaliser des traitements parallèles directement là où les données sont stockées.

![HBase et Spark](https://insatunisia.github.io/TP-BigData/img/tp4/hbase-spark.png)

