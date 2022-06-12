**Building a datapipeline with kafka and cassandra**
This demo demonstrate the functionality of a  data pipeline that is building upon three components. 


1. Weathercondition as Data Producer
2. Kafka as the scalable pub/sub system 
3. Data consumer

The weathercondition data will be generated and then it is written to Kafka in a topic that is called "weather". Then the data is written to a CSV file and can be transformed. Each component run in related containers. Containers are connected through a user defined docker bridge. Here is the list of required containers for running each component.





To setup the pipeline, we first need to define the docker bridge:

`docker network create kafka-network  `

At the second step we have to run the kafka docker compose file:

`docker-compose -f kafka/docker-compose.yml up`

The next step is to run data generator. This process writes the data stream to kafka under topic "weather".

`docker-compose -f owm-producer/docker-compose.yml up`

Then we can run the data consumer. This process reads the information from Kafka and writes it to a CSV file.

`docker-compose -f consumer/docker-compose.yml up -d `


Note that the data that can also be transferred from Kafka to cassandra by using the kafka-connect. In kafka-connect a sink is generated to take the data from Kafka, transform it to and save in a table in cassandra. We did not implement this part yet. However, the required files are in the cassandra folder. 







 

      






