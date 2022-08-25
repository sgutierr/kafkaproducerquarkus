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
