# PTR Exam

Describe a messaging-based system for the given scenario. Keep in mind such concerns as security, fault-tolerance, and performance (don't forget to mention what performance means for your system). Also depending on your scenario, there might also be domain-dependent requirements. Please describe the assumptions you've made and all the design decisions you took. Important: you can take inspiration from existing solutions but don't try to mimic them.

## V1: A system to collect and analyze sensor data for a smart home (heating, meteo, motion sensors, electricity).

We can assume that a sensor sends data every one second, which means we'll have N messages/second where N is number of sensors. This si not much and won't require any special optimizations. We can also assume that the data is not very big, so we can use JSON as a message format and HTTPS as a transport protocol. Because all the network communication will be happens inside one home, the server will accept requests only from local network, which should simply our security concerns.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v1.png)

## V2: A system to collect sensor data for a car (engine, breaks, media, climate control, cameras).

In case of a car the messages will be more frequent, let's assume each sensor will send one message every 10ms. This means we'll have 100 messages/second for each sensor and we'll also have a lot for redundant sensors, so it's gonna be a lot of messages to process. All communication will run in car's local network so we don't need to worry about security. We'll need a lot of redundancy and high availability. Also asynchronous communication like using a message broker or batching the messages is not an option because we need to know the state of the car in real time. Communication through a gateway could also add extra latency we can't afford. We'll have to eventually store these messages, because there are a lot of them we will batch them before saving to the database.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v2.png)

## V3: An online gaming backend (in-game chat, game state changes/multiplayer, leaderboard).

In this case we'll have a lot of small messages. If 60 fps is a playable frame rate for a game, we could say that we'll need 60 messages/second for each player. If we have 1000 players online we'll have 60 000 messages/second. This is a lot of messages and we'll need to optimize our system. Games also require low latency, so we'll use UDP as transport protocol. To make the game look smother we'll have to make small batches of messages and send them at once. For each game we'll have a separate server where players can connect. For login, leaderboard, and other non-realtime features we'll have a separate server for all players. We also might need to queue the players before they can join a game if there are too many of them.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v3.png)

## V4: A system to collect sensor data for a national electric grid (cable health/status, voltage, current, temperature, consumption).

The system will be deployed across various locations within the national electric grid. There will be a large number of sensors distributed throughout the grid, generating data continuously. If each sensor will generate a new data every 10 seconds, and we could have thousands of sensors, we'll have a lot of messages to process. We'll have to think about security because the system will be deployed across various locations, so we'll need to authenticate all requests before we process and store them. Let's use a Gateway to authenticate all requests. A perfect storage for this type of sensor data would be Kafka because it's optimized for high throughput and low latency, and will allow for real time processing of the data. To store the data long term we could have a daily job that will move the data from Kafka to a database.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v4.png)

## V5: A credit offerings search/aggregator service (insert your data, get a list of possible credit options from different banks).

The tricky part in this system is gathering up to date information from all the banks. We'll need to have a separate service that will periodically fetch the data from all the banks and store it in a database (assuming the banks have an API). The search service will be a simple REST API that will query the database and return the results. Let's assume the biggest scale, for example we need data from all banks in the world, even in such a extreme case we could fetch the data from banks in different time zones at different times, so the load will be distributed. We also could have separate databases for each region, because the users will be interested only in the banks from their region.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v5.png)

## V6: A machine learning prediction service (High-resolution images, streaming/real time data).

Because the images will be high-resolution we shouldn't pass them as messages. Instead we'll store them in a blob storage and pass the link to the image as message. Because this prediction algorithms could take time, we need a queue to store the requests and process them one by one. Prediction results could be stored in a database.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v6.png)

## V7: A batching data processing pipeline (extraction, transformations, loading, monitoring).

## V8: A CCTV (recognition) + video analysis (search+reports) system for city’s police department.

## V9: Elastic computing/data processing platform Hadoop/Spark-style.