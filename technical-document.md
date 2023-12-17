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
![High Level Architecture Diagram](/assets/diagram-placeholder.png)

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

| Component               | Technology                                             |
|-------------------------|--------------------------------------------------------|
| Web based backend       | Node.js                                                |
| Web based client        | React.js                                               |
| Mobile app              | React Native                                           |
| Database                | MongoDB                                                |
| Mobile app              | React Native                                           |
| In memory cache         | Redis                                                  |
| Message Queue           | Google PubSub                                          |
| Communication protocols | HTTPS (Web client and backend), WS (Mobile and Server) |

## Deployment plan
The entire system mostly will be orchestrated and supported by various Google Cloud Platform services.
  * Kubernetes engine for orchestrating Dockerized containers 
  * Utilize a content delivery network (CDN) for hosting the client-side application. 
  * Mongo Atlas will be employed for efficient MongoDB operations 
  * In Memory Store from GCP will be used as a cache.

## Capacity Estimation
### User Data
| Data Field              | Size (bytes) |
|-------------------------|--------------|
| User ID                 | 4            |
| Username                | 20           |
| Email                   | 30           |
| Password (hashed)       | 64           |
| Profile Information     | 100          |
| **Total Size per User** | **218**      |

### User Data Estimates (monthly):
| Parameter                   | Value       |
|-----------------------------|-------------|
| Number of Users (monthly)   | 100,000     |
| Estimated User Data Size    | 21.8 MB     |

### Race Data:
| Data Field                               | Size (bytes)        |
|------------------------------------------|---------------------|
| Race ID                                  | 4                   |
| Race Name                                | 50                  |
| Race Date                                | 8                   |
| Participants List (avg. 50 participants) | 218 per participant |
| Race Statistics                          | 50                  |
| **Total Size per Race**                  | **12,252**          |

### Race Data Estimates (monthly):
| Parameter                   | Value       |
|-----------------------------|-------------|
| Number of Races (monthly)   | 500         |
| Estimated Race Data Size    | 6.126 MB    |

### Real-Time Tracking Data:
| Data Field                            | Size (bytes) |
|---------------------------------------|--------------|
| Race ID                               | 4            |
| Participant ID                        | 4            |
| Timestamp                             | 8            |
| GPS Coordinates (Latitude, Longitude) | 16           |
| **Total Size per Update**             | **32**       |

### Tracking Data Estimates (monthly):
| Parameter                                 | Value       |
|-------------------------------------------|-------------|
| Number of Races (monthly)                 | 500         |
| Number of Participants per Race (avg.)    | 50          |
| Number of Updates per Participant (avg.)  | 100         |
| Estimated Tracking Update Data Size       | 800 MB      |

### Monthly Summary
| Metric                                | Value     |
|---------------------------------------|-----------|
| Total Storage Requirement (monthly)   | 28.726 MB |
| Total Bandwidth Requirement (monthly) | 10.8 GB   |


## Runtime Cost Analysis (Monthly Cost Estimate)

### Assumptions

- **Cloud Provider:** Google Cloud Platform (GCP)
- **Regions:** Standard region (e.g., us-central1)
- **Instance Types:** Standard instance types (e.g., n1-standard-2)
- **Managed Services:** MongoDB Atlas for MongoDB, Cloud Memorystore for Redis

### Compute Costs

#### Web-Based Backend Microservices:
- **User Management Service:** 2 instances
- **Race Management Service:** 2 instances
- **Real-Time Race Data Provider:** 2 instances

#### Mobile Application:
- Minimal server-side components

#### Real-Time Race Tracker Server:
- 1 instance per race

#### Real-Time Race Data Processor:
- 2 instances

#### Real-Time Data Delivery Service:

- 2 instances for delivery

#### Database and Cache Costs

- MongoDB: Utilize MongoDB Atlas with a basic configuration
- Redis: Utilize Cloud Memorystore

#### Data Transfer Costs

- Assume average data transfer based on the number of users and real-time updates
- $0.12 per GB for data transfer

### Monthly Cost Estimates

1. **Compute Costs:**
  - GCP Compute Engine (instances): $2000 - $3000
  - GCP Kubernetes Engine (for orchestration): $500 - $1000

2. **Managed Services:**
  - MongoDB Atlas: $100 - $200
  - Cloud Memorystore (Redis): $50 - $100

3. **Data Transfer Costs:**
  - Assume $0.12 per GB for data transfer: $200 - $300

4. **Total Estimated Monthly Cost:**
  - Sum of the above estimates: $2850 - $4400

--------------------------------------
![Scopic Software](/assets/footer.png)