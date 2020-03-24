# Starter Vert.x Kafka app

A starter app for testing out your [Apache Kafka](https://kafka.apache.org) deployment and trying out the [Vert.x Kafka client](https://vertx.io/docs/vertx-kafka-client/java/).

The app allows you to send records to a topic in Kafka called `test` every two seconds and consume back those records.

## Running the app

Test out the app by connecting to the websocket endpoints (UI will be added soon!):

 - Make sure Kafka is running locally: `docker run --rm -p 2181:2181 -p 3030:3030 -p 8081-8083:8081-8083        -p 9581-9585:9581-9585 -p 9092:9092 -e ADV_HOST=localhost        lensesio/fast-data-dev:latest`
 - Build the app: `mvn install`
 - Start the app: `java -jar target/demo-0.0.1-SNAPSHOT-fat.jar`
 - Go to the browser console
 - Connect the producer socket:
   ```
     ws = new WebSocket("ws://localhost:8080/demoproduce");
    ```
 - Start sending records to Kafka by sending the following message to the websocket:
    ```
    ws.addEventListener('message', function (event) {
       console.log('Producer socket received from server ', event.data);
   });
       
    ws.send("{\"action\": \"start\"}");
    ```
    The websocket will receive notifications every time the app sends a new record to Kafka.
 - Stop sending records by sending the following message to the websocket:
    ```
   ws.send("{\"action\": \"stop\"}");
   ```
 - Disconnect from the produce websocket
 - Connect the consumer socket:
    ```
       wsC = new WebSocket("ws://localhost:8080/democonsume");
   ```
 - Start consuming from Kafka by sending the following message to the websocket:
    ```
    wsC.addEventListener('message', function (event) {
          console.log('Consumer socket received from server ', event.data);
    });
   
   wsC.send("{\"action\": \"start\"}");
    ```
   The websocket will receive information about all the consumed records.
 - Stop consuming records by sending the following message to the websocket:
    ```
   wsC.send("{\"action\": \"stop\"}");
   ```
 - Disconnect from the consume websocket


## Configuration
To configure the application to connect to your Kafka edit the properties file in `src/main/resources`.
If your Kafka is secured you will need to enable the security configuration options and add your certificate to `src/main/resources`.
