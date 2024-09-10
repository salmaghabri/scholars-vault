#### un système de messaging
 - responsable du **transfert de données** d'une application à une autre, de manière à ce que les applications puissent **se concentrer sur les données** sans s'inquiéter de la manière de les partager ou de les collecter. 
 - Le messaging distribué est basé sur le principe de file de message fiable. 
 - Les messages sont stockés de manière asynchrone dans des files d'attente entre les applications clientes et le système de messaging.
 - Deux types de patrons de messaging existent:
	 1. Les systèmes "_point à point_" 
	 2. les systèmes "_publish-subscribe_".
#### 1. SYSTÈMES DE MESSAGING POINT À POINT
les messages sont stockés dans une file. un ou plusieurs consommateurs peuvent consommer les message dans la file, mais un message ne peut être consommé que par un seul consommateur à la fois. Une fois le consommateur lit le message, ce dernier disparaît de la file.

![Messaging Point à Point](https://insatunisia.github.io/TP-BigData/img/tp3/point-to-point.gif)
#### 2. SYSTÈMES DE MESSAGING PUBLISH/SUBSCRIBE
 les messages sont stockés dans un "_topic_". Contrairement à un système point à point, les consommateurs peuvent souscrire à un ou plusieurs topics et consommer tous les messages de ce topic.
![Messaging Point à Point](https://insatunisia.github.io/TP-BigData/img/tp3/pub-sub.gif)
#### Présentation de Kafka
Apache [Kafka](https://kafka.apache.org/) est une plateforme de streaming qui bénéficie de trois fonctionnalités:
1. Elle vous permet de publier et souscrire à un flux d'enregistrements. Elle ressemble ainsi à une file demessage ou un système de messaging d'entreprise.
2. Elle permet de stocker des flux d'enregistrements d'une façon tolérante aux pannes.
3. Elle vous permet de traiter (au besoin) les enregistrements au fur et à mesure qu'ils arrivent.
Les principaux avantages de Kafka sont:
1. **_La fiablitié_**: Kafka est distribué, partitionné, répliqué et tolérent aux fautes.
2. **_La scalabilité_**: Kafka se met à l'échelle facilement et sans temps d'arrêt.
3. **_La durabilité_**: Kafka utilise un _commit log_ distribué, ce qui permet de stocker les messages sur le disque le plus vite possible.
4. **_La performance_**: Kafka a un débit élevé pour la publication et l'abonnement.
#### Architecture de Kafka
1. **Topic**: Un flux de messages appartenant à une catégorie particulière. Les données sont stockées dans des topics.
2. **Partitions**: Chaque topic est divisé en partitions. Pour chaque topic, Kafka conserve un minimum d'une partition. Chaque partition contient des messages dans une séquence ordonnée immuable. Une partition est implémentée comme un ensemble de sègments de tailles égales.
3. **Offset**: Les enregistrements d'une partition ont chacun un identifiant séquentiel appelé _offset_, qui permet de l'identifier de manière unique dans la partition.
4. **Répliques**: Les répliques sont des _backups_ d'une partition. Elles ne sont jamais lues ni modifiées par les acteurs externes, elles servent uniquement à prévenir la perte de données.
5. **Brokers**: Les _brokers_ (ou courtiers) sont de simples systèmes responsables de maintenir les données publiées. Chaque courtier peut avoir zéro ou plusieurs partitions par topic. Si un topic admet N partitions et N courtiers, chaque courtier va avoir une seule partition. Si le nombre de courtiers est plus grand que celui des partitions, certains n'auront aucune partition de ce topic.
6. **Cluster**: Un système Kafka ayant plus qu'un seul Broker est appelé _cluster Kafka_. L'ajout de nouveau brokers est fait de manière transparente sans temps d'arrêt.
7. **Producers**: Les producteurs sont les éditeurs de messages à un ou plusieurs topics Kafka. Ils envoient des données aux courtiers Kafka. Chaque fois qu'un producteur publie un message à un courtier, ce dernier rattache le message au dernier sègment, ajouté ainsi à une partition. Un producteur peut également envoyer un message à une partition particulière.
8. **Consumers**: Les consommateurs lisent les données à partir des brokers. Ils souscrivent à un ou plusieurs topics, et consomment les messages publiés en extrayant les données à partir des brokers.
9. **Leaders**: Le leader est le noeud responsable de toutes les lectures et écritures d'une partition donnée. Chaque partition a un serveur jouant le rôle de leader.
10. **Follower**: C'est un noeud qui suit les instructions du leader. Si le leader tombe en panne, l'un des followers deviendra automatiquement le nouveau leader.
La figure suivante montre un exemple de flux entre les différentes parties d'un système Kafka:

![Architecture Kafka](https://insatunisia.github.io/TP-BigData/img/tp3/archi.jpg)

Dans cet exemple, un topic est configuré en trois partitions.
En supposant que, si le facteur de réplication du topic est de 3, alors Kafka va créer trois répliques identiques de chaque partition et les placer dans le cluster pour les rendre disponibles pour toutes les opérations. L'identifiant de la réplique est le même que l'identifiant du serveur qui l'héberge. Pour équilibrer la charge dans le cluster, chaque broker stocke une ou plusieurs de ces partitions. Plusieurs producteurs et consommateurs peuvent publier et extraire les messages au même moment.
#### Kafka et Zookeeper[¶](https://insatunisia.github.io/TP-BigData/tp3/#kafka-et-zookeeper "Permanent link")
[Zookeeper](https://zookeeper.apache.org/) est un service centralisé permettant de maintenir l'information de configuration, de nommage, de synchronisation et de services de groupe. Ces services sont utilisés par les applications distribuées en général, et par Kafka en particulier. Pour éviter la complexité et difficulté de leur implémentation manuelle, Zookeeper est utilisé.

![Utilisation de Zookeeper avec Kafka](https://insatunisia.github.io/TP-BigData/img/tp3/zookeeper-kafka.png)

Un cluster Kafka consiste typiquement en plusieurs courtiers (Brokers) pour maintenir la répartition de charge. Ces courtiers sont stateless, c'est pour cela qu'ils utilisent Zookeeper pour maintenir l'état du cluster. Un courtier peut gérer des centaines de milliers de lectures et écritures par seconde, et chaque courtier peut gérer des téra-octets de messages sans impact sur la performance.
Zookeeper est utilisé pour gérer et coordonner les courtiers Kafka. Il permet de notifier les producteurs et consommateurs de messages de la présence de tout nouveau courtier, ou de l'échec d'un courtier dans le cluster.
Il est à noter que les nouvelles versions de Kafka abandonnent petit à petit Zookeeper pour une gestion interne des métadonnées, grâce au protocole de consensus appelé KRaft (Kafka Raft).
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

