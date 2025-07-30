# How to Break a Monolith into Microservices

Consider a e-commerce application which has features
- User Authentication
- Product Calatog
- Order Management
- Payment Handling
- Notifications

## Step1 - Understand the current application
Use **Data Driven Design** to break down the application into indiviual services, ask questions such as **What services are tightly coupled?**, **Which Services can run independently?**
, 
## Step2 - Break down into domains
- Auth Service
- Product Service
- Payment serviceIf any amount gets deducted, it is automated by Keka, and no manual requests will be accepted.

- Order Service
- Notifcation service
These are the microservices identified.

## Step3 - Pick up one module to extract first
Don't convert every service at once, instead pick a service at once.
Example **Auth Service**
- Setup a separate reposiory and related infrastructure
- Design and Develop communication channel for other services to communicate (REST, GRPC)
- Existing Monolith will call the AUTH service whenever required

## Step4 - Setup Communication between services
Different services have to communicate with eachother

**Synchronous Communications** : REST, GRPC
**Asynchronous Communication** : Message Queue

## Step5 - Handle Shared Resource
Monoliths often use shared database, to convert it into service
- Establish a separate Database
- Migrate existing schema to the new one as per requirement
- Establish Data Consistency

## Step6 - Step 6: Add API Gateway and Authentication
You don’t want all services exposed directly.
- Use an API Gateway to route requests.
- Add authentication & rate limiting.

## Step7 - Deploy and Monitor
Use tools like:
- Docker + Kubernetes for containerization and orchestration.
- Prometheus + Grafana or Datadog for monitoring.
- ELK Stack or OpenTelemetry for logging and tracing.

# How Netflix moved from Monolith to Microservice
In the Early days Netflix was a DVD rental service, its web application was built as a monolith in Java.
which means entire application in a single code base, services tightly coupled and deployed in a data center.
Eventually, as Netflix grows in the number of users, streaming - problem occured In 2008 Netflix was down for few days because of a database corruption which is a single point of failure.

Hence, The realization current monolith architecture is fragile and too risky to add new features or change the existing one and slow risky deployment.

Step-by-Step Breakdown
🧩 Step 1: Modularize the Monolith

They began by breaking the monolith into modules/domains, like:

    User Management

    Movie Catalog

    Streaming Engine

    Recommendations

    Billing

    Reviews
    Each module would eventually become a microservice.

☁️ Step 2: Move to the Cloud (AWS)

They started moving services from their data center to AWS.

    No more hardware management.

    Easy to scale up/down.

    More reliable.

This allowed them to build and deploy services independently.

🧱 Step 3: Build Individual Microservices

They built hundreds of microservices, each doing one thing:
Service	Responsibility
Auth Service	Login, Tokens, Session
Catalog Service	Movie metadata
Recommendation Service	Personalized suggestions
Billing Service	Subscription management
Streaming Service	Video delivery logic

Each service had:

    Its own team

    Its own codebase

    Its own deployment pipeline

🔗 Step 4: Communication via APIs

    Services communicated via REST APIs (and later gRPC).

    Used asynchronous messaging (via queues) where needed.

🧠 Step 5: Smart Tools for Resilience

Netflix built internal tools to manage microservices:
Tool	Purpose
Eureka	Service Discovery – which services are alive
Hystrix	Circuit Breaker – prevents cascading failures
Zuul	API Gateway – routes requests to correct service
Ribbon	Client-side load balancing
Archaius	Dynamic configuration management
📊 Step 6: Observability & Monitoring

They implemented advanced logging, monitoring, and alerting.

    Used tools like Graphite, Atlas, Spinnaker.

    Collected metrics from every service.

    Built dashboards to monitor health and performance.

⚙️ Step 7: CI/CD Automation

They created Spinnaker, their own Continuous Delivery tool to:

    Deploy changes to individual services.

    Roll back instantly if something fails.

    Run tests automatically.



## Distributed Systems
Distributed systems are set of independent machine (nodes) which commnunicate with eachother to solve a given problem or complete a given task.
**Examples**
| System        | How It's Distributed                                            |
| ------------- | --------------------------------------------------------------- |
| Google Search | Thousands of servers across data centers answer your query.     |
| Netflix       | Separate services for streaming, billing, recommendations, etc. |
| WhatsApp      | Messaging runs on different servers around the world.           |
| Microservices | Each service can run on different machines.                     |

**Core Features**
- Multiple Nodes
- Communication
- Corodination
- Transparency
- Fault Tolreance
- Scalability

**Advantages**
- Handle Millions of users
- no single point of failure
- modular development

**Components**
| Component         | Role                                                                |
| ----------------- | ------------------------------------------------------------------- |
| **Nodes/Servers** | Machines that run services or store data                            |
| **Network**       | Enables communication between nodes                                 |
| **Middleware**    | Software that helps coordinate (e.g., message queues, service mesh) |
| **Databases**     | Often distributed themselves (e.g., Cassandra, MongoDB Cluster)     |


# CAP Theorem
In an Distributed system - you can gurantee any two of the following three properties:
1. **Consistency**
2. **Availabity**
3. **Partition Tolerance**

**Consistency**
It refers to all the nodes in the system should have **same data at the same time**, in other words every read operation should have the latest write operation or error.

**Availabilty**
It refers that for every available node - read/write operation should respond in timely manner even if some nodes are down.

**partition Tolerance**
It refers to the system continue to work fine even if there's an network partion has occured

In a system, **Network Partion** is a part-and-parcel hence **Partion Tolerance (P)** is must. Now the system can have either of consistency or availabilty

**CAP Tradeoff's**
- **AP**: Availabilty & Partion Tolerance
Here the data is availabe at every given time, but not consistent used when the data should be avaialabe every time no matter of the consistency (Social Media Platform) user experience matters the most.

- **CP**: Consistency & Partion Tolerance
Here the data is consistency which means the data is upto the mark as per the latest write operation but not available immediately. (Used in Banking Systems)

- **CA**: Consistency & Availabilty
Here the data is consistent and available every time -  without any partion tolerance
This is not an ideal world scenarion, Here it assumes in Distributed System no network failure has occured.

**Real-Life Analogy: Online Shopping**

Let’s say you’re buying the last unit of a phone online.

    Two users try to buy it at the same time.

    Servers are in two regions (India and US).

    Network between them breaks (partition).

Now:
✅ If the system is CP:

    One user’s request will succeed.

    The other may get an error or delay.

    Data stays correct.

✅ If the system is AP:

    Both requests might go through 😬

    But later, the system realizes only one item was in stock.

    Now it needs to reconcile (e.g., cancel one order).

# ACID Properties
ACID properties is conceputal properties which makes sure transactions occurs successfully, reliable and with safety.
- **Atomicity**: It refers either all or none -  a single transactions can have multiple operations, if one of the operation fails it cannot be resumed again either start again.
- **Consistency**: It refers to transactions should always follow the set of rules and constraints defined, If one of the rules was not followed then the tranasction failed.
- **Isolation**: If two transactions are executed simultaneously, there should be no intervention between these two transactions.
- **Duarabilty**: Once a transactions is complete and commited to the database, the data cannot be lost.

**When ACID is important**
- Banking (no lost money)
- Online Shopping (orders don't disappear)
- Healthcare (Patients records must be accurate)

# BASE Properties
BASE properties is alternative to ACID properties specaially used with NoSql database. 
- Basically Available: The system always responds, even if data isn't perfectly up-to-date.
- Soft State: Data can change over time even without new updates beacause of syncing delays
- Eventually Consistent: The data will be synced eventaully in a distributed environment, but not immediately.

**When to Use BASE?**
- Social media (a delayed "like" is okay).
- IoT/sensor data (speed matters more than precision).

# ACID vs BASE Consistency

| **Key Feature**  | **ACID**                           | **BASE**                                     |
| ---------------- | ---------------------------------- | -------------------------------------------- |
| **Consistency**  | Instant (strictly correct)         | Eventual (correct later)                     |
| **Availability** | May fail to enforce rules          | Always responds, even with delays            |
| **Speed**        | Slower (due to consistency checks) | Faster (no need to wait for synchronization) |
| **Use Cases**    | Banking, Flights, Healthcare       | Social Media, Ads, Analytics                 |

# Eventually Consistency
Eventually consistent is the phenomena where the data is updated eventually across all the serves but not immediately, Sometimes data may be inconsistency for some time, eventaully this would be available for all the users.

**How It Works**
- You update data (e.g., post a tweet).
- The change syncs across servers—but not instantly.
- For a short time, some users might see the old tweet (or no tweet).
- Eventually, all servers agree, and everyone sees the update.


# Sagas and Distributed Transactions in Microservices
Transactions in monolithic arhcitecture followed the **ACID** properties, as the services were tightly coupled all the services worked together to perform a transaction - finally would make a commit to the database.
Here if any of the operations gets failed the complete transactions will be failed and would start again.

**Transactions in distributed systems**
Transactions in distributed systems spread across various services & databases, but managing this was a tedious task beacuse the transaction logic was completey is single unit. As a alternative to this **sagas patterns** were introduced.

**Sagas Pattern**
The saga pattern is an alternative approach to manage distributed transactions in microservices. Instead of trying to execute all operations atomically, it breaks the transaction into a sequence of local transactions, each in a single service.

How Sagas Work:

1. Sequence of Local Transactions: Each step in a saga is a local transaction that updates the database and publishes an event/message.
2. Compensating Transactions: For each transaction, there's a compensating transaction that can undo its effects if needed.
3. Orchestration or Choreography: The saga can be coordinated either through:
    - Orchestration: A central coordinator tells participants what to do
    - Choreography: Participants subscribe to each other's events and react accordingly

# Idempotence
Idempotence means performing a operation multiple times has the same effect as performing it once.

**You could same request twice, but its result should not be changed.**

Idempotence is important because, In an Distributed network 
- Operation could be retried due to network failures
- Asynchronous message duplicates
- User may click same button multiple times

Due to which:
- Order placed can be doubled
- You might double charge a customer
- You might update data inconsistently.

**Idempotence can be used in**

- Payment Systems: A user should be able to make payments only once, avoid duplicate transactions by checking transaction_id
- Message Queue: A message should only be processed once, avoid processing same messages more than once.
- Retry Mechanism: 	Safe to retry requests without unexpected side-effects.
- HTTP Request (GET, POST): GET and PUT methods should be idemptent using ID's.
