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

