**Building a datapipeline with kafka and cassandra**
This demo demonstrate the functionality of a  data pipeline that is building upon three components. 

1. Weathercondition as Data Producer
2. Kafka as the scalable pub/sub system 
3. Data consumer




![datapipeline](https://user-images.githubusercontent.com/45789394/173239210-1d930858-dec2-475d-b9f9-565b886e0444.jpg)





The weathercondition data will be generated and then it is written to Kafka in a topic that is called "weather". Then the data is written to a CSV file and can be transformed. Each component run in related containers. Containers are connected through a user defined docker bridge. Here is the list of required containers for running each component.

It is based on this toturial: https://medium.com/sfu-cspmp/building-data-pipeline-kafka-docker-4d2a6cfc92ca



To setup the pipeline, we first need to define the docker bridge:

`docker network create kafka-network  `

At the second step we have to run the kafka docker compose file:

`docker-compose -f kafka/docker-compose.yml up`

The next step is to run data generator. This process writes the data stream to kafka under topic "weather".

`docker-compose -f owm-producer/docker-compose.yml up`

Then we can run the data consumer. This process reads the information from Kafka and writes it to a CSV file.

`docker-compose -f consumer/docker-compose.yml up -d `


Note that the data that can also be transferred from Kafka to cassandra by using the kafka-connect. In kafka-connect a sink is generated to take the data from Kafka, transform it to and save in a table in cassandra. We did not implement this part yet. However, the required files are in the cassandra folder. 

Notes:

1. In consumer, as the data is being written to a csv file the required permission should be set for the file that is mounted to the container.

3. As some of the containers do not need to be connected to each other, we can seperate them by creating different docker bridges and allocating each container to one of these bridges. In this way only the continaers that their connection is required are connected to each other.

5. At the next step, the data that is saved in database can be analyzed and at the end some results can be demonstrated visually.

Some documents to read:
1. https://www.cloudkarafka.com/blog/part1-kafka-for-beginners-what-is-apache-kafka.html Introduction to Kafka

3.  https://medium.com/walmartglobaltech/getting-started-with-the-kafka-connect-cassandra-source-e6e06ec72e97
An explanation about how the connection between Cassandra and Kafak work. Kafka-connect, REST API and Cassandra tables and keyspace.

2. https://medium.com/walmartglobaltech/getting-started-with-the-kafka-connect-cassandra-source-e6e06ec72e97
Running a data pipeline with Kafka,spark and docker containers. It uses a notebook environment for running the pipeline (Writing to Kafka, Reading data from Kafka by Spark and analyzing the data)









 

      






