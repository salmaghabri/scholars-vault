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
#### Présentation de Kafka[¶](https://insatunisia.github.io/TP-BigData/tp3/#presentation-de-kafka "Permanent link")
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