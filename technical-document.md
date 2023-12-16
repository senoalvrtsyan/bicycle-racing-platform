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

### **__The high-level architecture comprises following components:__**

* **Web Based Client Application**: _Using this app people can log in and check the stats from past races and has the ability 
to track live races in progress._
* **Web-based backend consisting of following microservices**
  * **User management service**: This service is responsible for user login/signup and role management.
  * **Race management service**: This service is responsible for creation of new races and showing statistics from old races.
  * **Real time race data provider**: This service responsible for providing real time location of race participants.
* **Mobile Application**: This app will be responsible for sending racer's real time location to server.
* **Real Time Race Tracker Server**: This service will accept the real time location of racer and push this data to message queue._
* **Real Time Race Data Processor**: This service will listen for message queue's messages save them into permanent data store(MongoDB) and also save the latest
location into in memory cache(Redis) to be used later by "real time race data provider".

### Design Description:

During the system design phase, special attention was given to ensuring scalability to accommodate the anticipated user load.
Recognizing the diversity of user roles within the platform, a dedicated service was implemented to handle user management tasks,
such as authentication, sign-up, and related functionalities.

To address the specific needs of race-related operations, a separate service was devised for race management. 
This design choice allows for independent scalability, ensuring efficient handling of past races, statistical data, 
and the creation of new races.

The core of the system involves real-time race data processing, responding to dynamic updates and user interaction. 
The mobile app smoothly transmits real-time location data to a designated service, which, in turn, used a message queue to streamline processing.
To enable easy scalability and keep things simple, we've designed the real-time race tracker service to start with just one instance for each race.

To handle the processed data, a dedicated service retrieves messages from the queue, stores them in a database for future statistical analysis,
and maintains the latest locations in an in-memory cache (Redis) to enable real-time data presentation. 
The race data processor service is designed for independent scaling, ensuring rapid and efficient message processing.

Another service, responsible for delivering real-time race data to clients, is then created. 
This service is designed for independent scalability and uses long polling or web sockets to communicate with the client-side web app. 
This approach ensures timely and synchronized delivery of real-time updates to users.


We chose MongoDB, a NoSQL database, because it scales well horizontally, handles different loads easily, 
and suits our system's needs for smooth scalability and efficient data management.

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