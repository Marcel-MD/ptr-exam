Describe a messaging-based system for the given scenario. Keep in mind such concerns as security, fault-tolerance, and performance (don't forget to mention what performance means for your system). Also depending on your scenario, there might also be domain-dependent requirements. Please describe the assumptions you've made and all the design decisions you took. Important: you can take inspiration from existing solutions but don't try to mimic them.

V1: __A system to collect and analyze sensor data for a smart home (heating, meteo, motion sensors, electricity).__

We can assume that a sensor sends data every one second, which means we'll have N messages/second where N is number of sensors. This si not much and won't require any special optimizations. We can also assume that the data is not very big, so we can use JSON as a message format and HTTPS as a transport protocol. Because all the network communication will be happens inside one home, the server will accept requests only from local network, which should simply our security concerns.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v1.png)

If we don't want to lose any messages we can add a message broker between the gateway and analyzers.

V2: __A system to collect sensor data for a car (engine, breaks, media, climate control, cameras).__

In case of a car the messages will be more frequent, let's assume each sensor will send one message every 10ms. This means we'll have 100 messages/second for each sensor and we'll also have a lot for redundant sensors, so it's gonna be a lot of messages to process. All communication will run in car's local network so we don't need to worry about security. We'll need a lot of redundancy and high availability. Also asynchronous communication like using a message broker or batching the messages is not an option because we need to know the state of the car in real time. Communication through a gateway could also add extra latency we can't afford. We'll have to eventually store these messages, because there are a lot of them we will batch them before saving to the database.

![Diagram](https://github.com/Marcel-MD/ptr-exam/blob/main/v2.png)

V3: __An online gaming backend (in-game chat, game state changes/multiplayer, leaderboard).__

V4: __A system to collect sensor data for a national electric grid (cable health/status, voltage, current, temperature, consumption).__

V5: __A credit offerings search/aggregator service (insert your data, get a list of possible credit options from different banks).__

V6: __A machine learning prediction service (High-resolution images, streaming/real time data).__

V7: __A batching data processing pipeline (extraction, transformations, loading, monitoring).__

V8: __A CCTV (recognition) + video analysis (search+reports) system for city’s police department.__

V9: __Elastic computing/data processing platform Hadoop/Spark-style.__