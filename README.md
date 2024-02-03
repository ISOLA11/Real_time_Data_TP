Etape 1 : Création du topic "velib-projet" :

@ISOLA11 ➜ /workspaces/Real-time_Data_Streaming (main) $ ./kafka_2.12-2.6.0/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic velib-projet
Created topic velib-projet.
@ISOLA11 ➜ /workspaces/Real-time_Data_Streaming (main) $ ./kafka_2.12-2.6.0/bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic velib-projet
Topic: velib-projet     PartitionCount: 1       ReplicationFactor: 1    Configs: segment.bytes=1073741824
        Topic: velib-projet     Partition: 0    Leader: 0       Replicas: 0     Isr: 0


Etape 2 : Création du Topic "velib-projet-final-data" :
@ISOLA11 ➜ /workspaces/Real-time_Data_Streaming (main) $ ./kafka_2.12-2.6.0/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic velib-projet-final-data
Created topic velib-projet-final-data.


Etape 3 : Collecte des Données des Stations Vélib
Remplacer producer.send("velib", message) ====> producer.send("velib-projet", message)


Etape 4  : Filtrage des Données et Publication
Ajout de la ligne de code  "filtered_data = [station for station in stations if station["stationCode"] in ["16107", "32017"]]" au sein du script "Kafka_exercice.py" pour n'obtenir que les infos des stations voulues dans le topic. On peut aussi visualiser les données via un producer pour voir si toutes nos données sont bien acheminées. 
Lancer le code "kafka_exercice.py" pour lancer l'acheminement des données vers le topic "velib-projet".



Etape 5 & 6 : Traitement des Données avec Spark Streaming
Pour cette étape nous allons nous focaliser uniquement sur le script SPARK. 
Pour cette étape, j'ai réalisé une jointure entre les données de vélos provenant de Kafka et des informations sur les stations Vélib. Le code effectue un traitement par lot, extrayant les codes postaux distincts, filtrant les données par code postal et calculant les totaux de vélos disponibles, mécaniques et électriques. Les résultats sont écrits en continu dans le topic "velib-projet-final-data", à chaque batch et le processus est maintenu en attente continue de nouveaux résultats.
Pour effectuer tout cela j'ai grandement modifier le code "spark.Py" pour pouvoir lancer l'acheminement des données vers le nouveau topic. Après de longues heures de travail, j'ai finalement réussi la processus. 
Lancer le code "spark.py".

@ISOLA11 ➜ /workspaces/Real-time_Data_Streaming (main) $ ./kafka_2.12-2.6.0/bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic velib-projet-final-data
Topic: velib-projet-final-data  PartitionCount: 1       ReplicationFactor: 1    Configs: segment.bytes=1073741824
        Topic: velib-projet-final-data  Partition: 0    Leader: 0       Replicas: 0     Isr: 0
    


