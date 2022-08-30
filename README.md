# Kafka Producer/Consumer example using Quarkus
This is an example of a Java producer that sends a random integer to a topic named `total-connected-devices`.
# Prerequisites
This is the software required for running this example:
 - Git 
 - Java Development Kit (JDK) OpenJDK 11
 - Red Hat OpenShift Container Platform CLI 
 - Kafka installlation on Red Hat OpenShift 
 - Create a topic called `total-connected-devices`
	> oc process \\
	> -f resources/create-topic.yaml \\
	> -p TOPIC_NAME='device-temperatures' \ | oc apply -f -

# Demo steps:
## 0. Preparing certificate 
  1. Extracting the cluster certificate from OCP, example from Kafka namespace:
 **`oc extract secret/my-cluster-cluster-ca-cert --keys=ca.crt `**
 
 2. Generate a `truststore.jks` file with `password` as the store password. Store the KeyStore files in your workspace:
 **`keytool -import -trustcacerts -alias root -file kafka-cluster.crt -keystore truststore.jks -storepass password -noprompt`**

This application listens for records sent to the `device-temperatures` topic.
## 1. Setting up connection
By using your editor of choice, open the src/main/resources/application.properties file, and configure the following Kafka settings:
Replace these variables by your correspondent environment variables: 
    "YOUR_KAFKA_BOOTSTRAP_HOST"
    "YOUR_KAFKA_BOOTSTRAP_PORT"
    "ABSOLUTE_PATH_TO_YOUR_WORKSPACE_FOLDER"
## 2. Compile application
In your command-line terminal, navigate to the `kafka-producers/quarkus` directory. Compile the application.
**`./mvnw clean package quarkus:dev`**

## 3. Testing
Running the oc and the kafka-console-consumer command inside of the broker you can retrieve topic records as follow:
 **`oc exec -it my-cluster-kafka-0 -- bin/kafka-console-consumer.sh  --bootstrap-server my-cluster-kafka-bootstrap:9092  --topic vehicle-positions  --from-beginning`**
 
 To count topic records:
 **`oc exec -it my-cluster-kafka-0 -- bin/kafka-run-class.sh kafka.tools.GetOffsetShell  --broker-list my-cluster-kafka-bootstrap:9092 --topic vehicle-positions`**
 
 Example of Kafka Consumer for testing which pass key and value deserializer:
 
  `oc exec \
 -it my-cluster-kafka-0 \
  -- bin/kafka-console-consumer.sh \
 --bootstrap-server my-cluster-kafka-brokers:9092 \
 --topic vehicle-feet-elevations \
 --key-deserializer org.apache.kafka.common.serialization.IntegerDeserializer \
 --value-deserializer org.apache.kafka.common.serialization.DoubleDeserializer \
 --property print.key=true --property key.separator=" - "`
