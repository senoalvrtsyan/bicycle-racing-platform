# Bicycle Racing Platform

## Challenges

**1. Real-time Tracking:** \
Ensuring accurate and real-time tracking of racers during live events, considering up to 50 participants per race.

**2. Scalability:** \
Designing the system to handle an infinite number of users and scale efficiently as the load increases.

**3. Security:** \
Implementing robust user authentication and authorization mechanisms to protect sensitive data and ensure secure interactions.

**4. Concurrency:** \
Managing multiple real-time races simultaneously without compromising performance.

## High Level Architecture Design
![High Level Architecture Diagram](/assets/diagram-placeholder.jpg)

### **__The high-level architecture comprises following components__**

* **Web Based Client Application**: Using this app people can log in and check the stats from past races and has the ability 
to track live races in progress.
* **Web-based backend consisting of following microservices**
  * **User management service**: This service is responsible for user login/signup and role management.
  * **Race management service**: This service is responsible for creation of new races and showing statistics from old races.
  * **Real time race data provider**: This service responsible for providing real time location of race participants.
* **Mobile Application**: This app will be responsible for sending racer's real time location to server.
* **Real Time Race Tracker Server**: This service will accept the real time location of racer and push this data to message queue._
* **Real Time Race Data Processor**: This service will listen for message queue's messages save them into permanent data store(MongoDB) and also save the latest
location into in memory cache(Redis) to be used later by "real time race data provider".


## Technology Stack

| Component               | Technology                                           |
|-------------------------|------------------------------------------------------|
| Web based backend       | Node.js                                              |
| Web based client        | React.js                                             |
| Mobile app              | React Native                                         |
| Database                | MongoDB                                              |
| Mobile app              | React Native                                         |
| In memory cache         | Redis                                                |
| Message Queue           | Kafka                                                |
| Communication protocols | HTTPS(Web client and backend), WS(Mobile and Server) |

## Deployment plan
The entire system mostly will be orchestrated and supported by various Google Cloud Platform services.
  * Kubernetes engine for orchestrating Dockerized containers 
  * Utilize a content delivery network (CDN) for hosting the client-side application. 
  * Mongo Atlas will be employed for efficient MongoDB operations 
  * In Memory Store from GCP will be used as a cache.

## Runtime Cost Analysis
*[Todo: Do the runtime cost analysis in this section]

--------------------------------------
![Scopic Software](/assets/footer.png)