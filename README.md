# Building a datapipeline with kafka and cassandra


This demo demonstrate the functionality of a  data pipeline that is building upon three components. 

1. Weathercondition as Data Producer
2. Kafka as the scalable pub/sub system 
3. Data consumer




![datapipeline](https://user-images.githubusercontent.com/45789394/173239210-1d930858-dec2-475d-b9f9-565b886e0444.jpg)





The weathercondition is written to Kafka in a topic that is called "weather". Then the data is written to a CSV file and can be transformed. Each component runs in a containers. Containers are connected through a user defined docker bridge. Here is the list of required containers for running each component. 





To setup the pipeline, we first need to define the docker bridge:

`docker network create kafka-network  `

At the second step we have to run the kafka docker compose file:

`docker-compose -f kafka/docker-compose.yml up`

The next step is to run data generator. This process writes the data stream to kafka under topic "weather".

`docker-compose -f owm-producer/docker-compose.yml up`

Then we can run the data consumer. This process reads the information from Kafka and writes it to a CSV file.

`docker-compose -f consumer/docker-compose.yml up -d `


Please note that the data can also be transferred from Kafka to cassandra by using the kafka-connect. In kafka-connect, kafka-conssandra connector is installed to poll the data from Kafka and save it into cassandra. We need to add Cassandra Source connector to the Kafka Connect. We did not implement this part yet. 

Notes:
1. This document is based on this toturial: ttps://medium.com/sfu-cspmp/building-data-pipeline-kafka-docker-4d2a6cfc92ca

2. In consumer, as the data is being saved to a csv file the required permissions should be set for the file that is mounted to the container.

3. It is not ncessary to connect the all containers to gether. We can creat different docker bridges and allocate the containers that they need to be connected to one bridge. In this way only the necessary connection is allowed.

4. At the next step, the data that is saved in database can be analyzed and at the end some results can be demonstrated visually.

Some documents to read:
1. https://www.cloudkarafka.com/blog/part1-kafka-for-beginners-what-is-apache-kafka.html Introduction to Kafka

2.  https://medium.com/walmartglobaltech/getting-started-with-the-kafka-connect-cassandra-source-e6e06ec72e97
An explanation about how the connection between Cassandra and Kafak work. Kafka-connect, REST API and Cassandra tables and keyspace.

3. https://medium.com/walmartglobaltech/getting-started-with-the-kafka-connect-cassandra-source-e6e06ec72e97
Running a data pipeline with Kafka,spark and docker containers. It uses a notebook environment for running the pipeline (Writing to Kafka, Reading data from Kafka by Spark and analyzing the data)









 

      






