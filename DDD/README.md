# Domain Driven Design (DDD) - Introduction

**What is Domain-Driven Design (DDD)?**

- Domain-Driven Design (DDD) is a software development methodology and set of principles that focuses on building software systems that closely align with the real-world problem domains they address.
- DDD emphasizes the importance of understanding and modeling the core domain of the software, which is the specific problem space or area of expertise that the software is designed to solve.
- It was first introduced by Eric Evans in his book "Domain-Driven Design: Tackling Complexity in the Heart of Software" in 2003.

**Why is DDD important?**

- DDD helps developers create software that accurately reflects the complexities and nuances of the business or problem domain it serves. This alignment between the software and the domain leads to more effective and maintainable solutions.
- By providing a clear and shared language for developers and domain experts (often called the "Ubiquitous Language"), DDD facilitates better communication and collaboration between technical and non-technical stakeholders.
- DDD encourages a focus on the core business logic and rules, which can lead to more flexible and adaptable software that can evolve as the domain evolves.

## Ubiquitous Language and Bounded Contexts

**The Importance of a Shared Language (Ubiquitous Language) between Developers and Domain Experts:**

- In Domain-Driven Design (DDD), a shared language known as the "Ubiquitous Language" is a cornerstone concept. It refers to a common vocabulary and set of terms that both developers and domain experts use when discussing the software's problem domain.
- The Ubiquitous Language bridges the communication gap between technical and non-technical team members. It ensures that everyone involved in the project speaks the same language, reducing misunderstandings and misinterpretations.

**Defining Bounded Contexts to Set Clear Boundaries within Your Software:**

- In DDD, a Bounded Context is a crucial organizational concept used to define clear boundaries within a software system. Each Bounded Context encapsulates a specific part of the problem domain and maintains its own distinct Ubiquitous Language.
- Bounded Contexts are essential because they recognize that complex domains can have different interpretations and meanings for various parts of the system. By isolating these interpretations, developers can avoid conflicts and ensure clarity within each context.
- Bounded Contexts help prevent "ubiquitous language collision," where a term or concept means different things in different parts of the system. They allow developers to focus on a specific subset of the domain without being overwhelmed by the entire domain's complexity.

## Entities and Value Objects

- **Entities:** Entities are objects that have a distinct identity and are defined by a unique identifier (ID). They represent things in the domain that have a lifecycle and can change over time. For example, a "Customer" in an e-commerce system is typically an entity because it can be identified by a customer ID, and its attributes like name and address can change.
- **Value Objects:** Value Objects, on the other hand, are objects that don't have a distinct identity and are immutable. They represent characteristics, measurements, or descriptors of entities. Value Objects are defined solely by their attributes and should not change once created. For example, a "Date of Birth" or a "Money" value (consisting of a currency and an amount) are typically modeled as Value Objects.

**How to Identify and Model Them in Your Domain:**

Identifying whether a concept in your domain should be modeled as an Entity or a Value Object is a crucial design decision:

- **Use Entities** when you need to track the lifecycle of an object, and it has a distinct identity that matters. Think about whether you would need to compare two instances for equality based on their identity.
- **Use Value Objects** when the object is immutable and defined solely by its attributes, and you only care about the values themselves, not their identity.
  To model Entities and Value Objects in your domain:
- **Entities:** Create classes that encapsulate the entity's state and behavior. Include an identifier (ID) as a key property.
- **Value Objects:** Create classes with attributes that define the value. Ensure immutability by not providing methods to change their state.
  Implement equality and hash code methods appropriately for both Entities and Value Objects to ensure correct behavior when comparing or using them in collections.

## Aggregates and Aggregate Roots

Martin Fowler explains:

> Aggregates are the basic element of transfer of data storage – you request to load or save whole aggregates. Transactions should not cross aggregate boundaries.

In Domain-Driven Design (DDD), an "Aggregate" and an "Aggregate Root" are closely related concepts, but they serve different roles within the domain model.

**Aggregate:**
An Aggregate is a group of one or more related domain objects that are treated as a single unit for data changes and consistency.It can consist of entities and value objects that are logically and semantically related, aggregates define transactional boundaries, meaning that all changes to the objects within an aggregate are made together in a single transaction or operation.Aggregates are used to ensure data consistency and to encapsulate complex business logic related to the objects they contain.

**Aggregate Root:**
An Aggregate Root is one specific entity within the aggregate that serves as the entry point or the "root" for accessing and interacting with the entire aggregate.

The Aggregate Root is responsible for maintaining the integrity and enforcing business rules for the objects within the aggregate.

It is the only object that can be directly referenced from outside the aggregate. External entities, such as application services or other aggregates, interact with the aggregate only through its root.

The Aggregate Root is typically an entity with a unique identifier (ID) that distinguishes it from other instances of the same type.

> The main difference between an Aggregate and an Aggregate Root is that an Aggregate is a collection of related domain objects, while the Aggregate Root is a specific object within the aggregate that acts as the gateway for interactions with the entire aggregate. The Aggregate Root is crucial for maintaining the consistency and enforcing the rules of the aggregate, and it ensures that external entities interact with the aggregate in a controlled and consistent manner.

**Designing Aggregate Roots to Ensure Consistency within an Aggregate:**

Every Aggregate has an Aggregate Root, which is an entity that serves as the entry point to the aggregate. The Aggregate Root is responsible for enforcing business rules and maintaining the integrity of the aggregate.

The Aggregate Root is the only point through which external entities can access and modify the aggregate's internal state. This ensures that all changes to the aggregate follow the prescribed rules.

When designing an Aggregate Root:

- Identify the root entity that best represents the aggregate and its business rules.
- Define methods and behaviors on the Aggregate Root that allow controlled manipulation of the aggregate's internal objects.
- Enforce invariants (business rules) within the Aggregate Root to maintain a consistent and valid state.

It's important to keep Aggregate Roots as small as possible while still encapsulating the necessary behavior. This helps with scalability and reduces contention in multi-user scenarios.

## Repositories and Factories

**Repositories Managing Aggregates:**
A Repository is a pattern used to manage and handle Aggregates. Repositories provide a way to access and persist Aggregates, shielding the rest of the application from the details of how the data is stored.They are responsible for retrieving Aggregates from a data store, such as a database, and saving them back when changes are made.

Responsibilities of repositories:

- Providing methods to find and retrieve Aggregates by their unique identifiers.
- Abstracting away the details of data storage and retrieval, allowing the domain layer to work with Aggregates as if they were in memory.
- Ensuring that Aggregates are consistent and valid when they are retrieved and persisted.

**Factories Creating Complex Objects:**
Factories are responsible for creating complex domain objects, including Aggregates, Entities, and Value Objects. They abstract the object creation process and encapsulate any complex logic or rules required for creating these objects. They are useful in scenarios where object creation is not straightforward or involves multiple steps, validation, or decisions.

Responsibilities of Factories:

- Encapsulating the knowledge of how to create objects with the correct initial state.
- Enforcing business rules related to object creation.
- Promoting the reuse of complex object creation logic across the application.

## Domain Events and Event Sourcing

**Using Domain Events to Capture Changes in Your Domain:**
Domain Events are a powerful concept that enable the capture of significant changes and state transitions within your domain. These events represent meaningful occurrences or facts in the domain and serve as a means of communicating changes to interested parties. When something important happens within an Aggregate, such as an order being placed, a payment being processed, or a user profile being updated, a Domain Event is raised to record that event. These events can be used for various purposes, including notifying other parts of the system, maintaining an audit trail, or even triggering asynchronous processes.

**Introduction to Event Sourcing as a Storage Mechanism:**
Event Sourcing is a storage mechanism that complements Domain Events by recording all state changes in your system as a sequence of immutable events. Instead of storing the current state of an entity, Event Sourcing keeps track of how that entity arrived at its current state by storing a history of events. These events, which encapsulate changes in the domain, can be replayed to reconstruct the current state of an entity at any point in time.
Event Sourcing offers benefits like: Auditability, Traceability, and The ability to rewind and analyze historical data.

¿Cuál de las siguientes afirmaciones describe mejor el concepto barrido de ping?
(Seleccione una única respuesta)
a. Es un método de prevención para el análisis de puertos que evita que una red sea detectada
b. Es un mecanismo para escanear los puertos de una red completa usando NMAP y así identificar los puertos TCP y UDP abiertos
c. Es un proceso que identifica los dispositivos presentes en una red que responden a paquetes del tipo ICMP
d. Es una prueba de conectividad extremo a extremo entre 2 o más dispositivos de red
