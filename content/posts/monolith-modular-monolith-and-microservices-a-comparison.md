---
title: "Monolith, Modular Monolith, and Microservices: A Comparison"
date: 2025-01-06
---

Software architecture plays a pivotal role in the success of any application. Among the most frequently discussed architectural models are monoliths, modular monoliths, and microservices. Choosing the right model depends on factors such as system complexity, scalability requirements, and maintenance considerations. In this article, we gonna view a quick introduction of each concept with some examples.

## Monolith

The monolith is the most traditional architectural model, where the entire application is built as a single unit. All components (interface, business logic, and database) are integrated and interact directly within the same project.

A typical example of monolithic architecture is ERP systems used by small businesses or startups. These systems centralize all functionality within a single application, benefiting from simplicity and faster implementation during the initial stages.

### Characteristics

- **Highly coupled**: All components have direct dependencies. For example, the authentication logic, requests, and products are integrated, without clear boundaries between responsibilities.
- **Overlapping responsibilities**: It's common to find duplicated or merged code fragments, like validation of products combined with exhibition logic or authentication. Even with some basic organization, generally there is not a robust logical split between the layers.
- **Example file structure**:

```
- app/
    - controllers/
        - UsersController
        - ProductsController
    - models/
        - User
        - Product
    - services/
        - OrderService
        - EmailService

```

### Common Scenario

In a typical scenario of a monolith, all application components are centralized in a single project:

- **Server**: A virtual machine hosting all application processes.
- **API**: A Rails or Laravel server managing the client interface and business logic.
- **Background processes**: A manager like Sidekiq handles asynchronous tasks, such as sending emails or processing data.
- **Database**: A PostgreSQL server centralized, directly integrated into the application.
- **Development flow**: The API, Sidekiq, and database migration codes are maintained in the same repository, being developed and deployed together.

### Advantages:

- **Simplicity**: Ideal for starting a project, because of the ease of project development and deployment.
- **Performance**: Enables fast communication between components since they operate within the same project.
- **Consolidated tools**: Most frameworks were developed to support monolithic applications.

### Disadvantages:

- **Limited Scalability**: Horizontal scalability is less efficient because it requires the replication of the entire monolith.
- Low flexibility: A minimal modification in a part of the system can affect the entire application, increasing risks.

## Modular Monolith

A common example of this approach is SaaS applications in expansion, like a marketplace that splits product lists, payments, and users into internal modules. This format facilitates a future transition to a microservices architecture by establishing clear boundaries between features.

### Characteristics

- **Responsibilities Separation**: Each module (e.g., “Users”, “Requests”, “Payments”) wraps its logic and data. Interactions between modules happen using defined interfaces or services (UserService, PaymentService…), avoiding direct access to models, helpers, etc. Here, each application feature is wrapped in its own module, promoting organization and scalability.
- **Less Dependencies**: The internal coupling is reduced because modules have clear boundaries and their interface for communication.
- Preparation of Microservices: Internal modularity makes it possible to split modules into independent services in the future if necessary.
- **Example file structure**:

```
- app/
    - modules/
        - users/
            - controllers/
                - UsersController
            - models/
                - User
            - services/
                - UserService
        - products/
            - controllers/
                - ProductsController
            - models/
                - Product
            - services/
                - ProductService

```

### Common Scenario

In a modular monolith, the main project is implemented as a single unit, but organized into different modules:

- **Application Server**: A backend like Spring Boot or Django, where each module manages a functionality (e.g., users, products, or payments).
- **Background Jobs**: A task manager like Celery, integrated through specific configurations of modules.
- **Shared Database**: An example is a MongoDB database with separate collections for each module or functionality.
- **Reusable Modules**: Each module can be developed and tested separately, but the system works as a single application.

### Advantages

- **Better organization**: Modularity makes the code more legible and easier to maintain.
- **Ease of transition**: It can serve as an intermediate step for a future migration to microservices.
- **Preserved performance**: Like the traditional monolith, communication occurs within the same process.

### Disadvantages

- **Internal dependencies**: Coupling issues can still arise, especially if the interfaces aren’t well defined.
- **Monolithic deployment**: Despite modularity, the system is still deployed as a single package.
- **Boundaries management**: Requires development discipline to maintain clearly defined responsibilities between modules.

## Microservices

Microservices represent a distributed architectural approach, where the application is composed of many independent services. Each service is responsible for specific functionalities and can be developed, deployed, and scaled independently.

Large-scale platforms such as Uber utilize this approach to separate critical functionalities, including geolocation, ride management, payments, and real-time communications.

### Microservices Characteristics

- **Independent services**: Each functionality is isolated in a microservice, which has its own life cycle and can be developed or deployed without directly depending on other services.
- **Technological flexibility**: It is possible to use different technologies and programming languages for each microservice, depending on the specific needs of each functionality.
- **Distributed communication**: Services communicate through REST APIs, gRPC, or asynchronous messages (e.g., RabbitMQ, Kafka). This approach allows for uncoupling functionalities but adds the necessity to manage latency and reliability.

### Common Scenario

In a microservice-based application, services work autonomously but are orchestrated to create a coherent system:

- **Independent services**:
    
    - A microservice for authentication developed in Python with Flask.
    - Another microservice for payments, using Node.js and integrated with a gateway like Stripe.
    - A microservice for product recommendations, implemented in Go with support for machine learning models.
    

- **Orchestration**:
    
    - Solutions such as Kubernetes or Docker Compose are used to manage containers and ensure communication between services.
    

- **Distributed Databases**:
    
    - Each microservice manages its own database to meet its needs:
        
        - MySQL for the authentication service.
        - PostgreSQL for payments.
        - MongoDB for recommendations.
        
    

### Advantages

- **Scalability**: Allows for the individual scaling of each service, optimizing resource usage.
- **Resilience**: Problems in one service are isolated, reducing the impact on the entire application.
- **Technological flexibility**: Each service can adopt the most suitable technology for its functionality.

### Disadvantages

- **Complexity**: Managing communication, orchestration, and monitoring between services requires robust tools and a high level of discipline.
- **Infrastructure Overhead**: Maintenance costs increase due to the necessity of separate environments for each service.
- **Latency**: Communication over the network between services introduces delays and potential additional failure points.

## Sources:

- Como criar um sistema monolítico modular de verdade
- Design Monolitos Modulares parte 1
- What is a modular monolith
- What are microservices

Go to Source
