# Clean Architecture

## Domain Centric Architecture

Domain-centric architecture is an architectural approach and methodology for building complex software systems that prioritize a deep understanding of the problem domain as the foundation for software design. As the name suggests, all of these domain-centered architectures are a set of architectures essentially focused on different aspects of the same set of core ideas.

<img width="1382" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/d5cd99ce-f76b-4f0e-89d8-1760090a51ef">

## Application Layers

The application core layer contains all the business logic along with entities, domain services, and interfaces. It should have minimal dependencies on databases, instead, we expect it should have interface and domain model types including entities, and value objects aggregates. There are domain services that will have logic in them that affects multiple entities or aggregates.

<img width="860" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/de2229c3-ed9a-4b1f-81e0-2f9f1d644a57">

### Layer Dependencies

<img width="574" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/7b4bc468-1d22-48b7-a8b1-6e8a81ad49ab">


## Commands and queries

Commands and queries are two distinct types of interactions with a software system, each serving a specific purpose. 

1. **Commands:**
   - **Purpose:** Commands are used to represent actions that change the state of the system, often referred to as "write" operations. They encapsulate the intent to perform an action or modify data.
   - **Characteristics:**
     - Commands are typically associated with use cases or application-specific business logic.
     - They can include operations like creating, updating, or deleting data records.
     - Commands are usually executed within the application's core use case or interactor layer.
   - **Interaction Flow:** When a command is received, it typically goes through the following layers:
     - **Controller/Presenter:** In the user interface or API layer, a controller or presenter receives the user's request and translates it into a command.
     - **Use Case/Interactor:** The command is passed to the use case or interactor layer, where the application's business rules are applied.
     - **Entities/Domain Models:** In some cases, commands may interact with domain entities to enforce business logic.
     - **Repository/Gateways:** Commands may also involve interactions with data sources (e.g., databases) to persist changes.
   - **Example:** An "Add New User" command could be used to create a new user account, including data validation and database insertion.

2. **Queries:**
   - **Purpose:** Queries are used to retrieve data from the system without changing its state, often referred to as "read" operations. They represent requests for information or data retrieval.
   - **Characteristics:**
     - Queries are typically associated with retrieving data for presentation, reporting, or analysis purposes.
     - They do not alter the system's state; instead, they return information based on specific criteria.
     - Queries are usually executed within the application's read-only or query layer.
   - **Interaction Flow:** When a query is made, it typically follows these steps:
     - **Controller/Presenter:** In the user interface or API layer, a controller or presenter receives a query request.
     - **Use Case/Interactor:** The query request is forwarded to the use case or interactor layer, which may involve querying a data source or repository.
     - **Data Source/Repository:** The query retrieves data from a data source (e.g., database) or repository.
     - **Response:** The queried data is returned to the controller/presenter for presentation.
   - **Example:** A "Get User Profile" query could retrieve a user's profile information for display in a user interface.


<img width="832" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/54a50f53-1359-4427-ae2f-67c030cb45d4">


## Functional organization

Entities are fundamental concepts representing the core business objects or concepts within your application. They encapsulate the most critical and essential business rules and data in your domain. Entities are part of the innermost layer of the architecture and are at the heart of the application's business logic.

On the Left side, you can see a classic MVC folder structure and on the right side you can see a clean architecture folder structure grouped by entities

<img width="1188" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/bcf70ed3-dc00-4fb3-b628-057f7bd8f3fe">


## Testable Architecture

<img width="1024" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/a3df4758-e0a5-4a4e-89aa-dcd474373cda">

The way you can organize your test is based on the layer dependencies diagram by changing concepts:

Layer dependencies diagram:

<img width="421" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/99b48081-419b-49e4-9152-9795c3de16a3">

Test diagram:

<img width="455" alt="image" src="https://github.com/stivendiaz/carrier-path-stiven-diaz/assets/26977162/38e25281-60bc-4cd3-a244-95638dc1452a">

