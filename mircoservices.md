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
- 
