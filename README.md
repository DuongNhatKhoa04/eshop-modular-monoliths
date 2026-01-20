## Architecture Philosophy
This project adopts a **Modular Monolith** approach as a balanced solution between traditional monolithic architectures and microservices.

The primary goal is to achieve **strong modular boundaries**, **high maintainability**, and **clear separation of concerns**, while keeping **deployment and operational complexity low**. This approach enables faster development and simpler operations in the early stages, while still allowing a gradual evolution toward microservices when required.

The system implements an **EShop domain** composed of the following core modules: **Catalog**, **Basket**, **Identity**, and **Ordering**.
It leverages **cloud-native backing services** such as **Redis**, **RabbitMQ**, and **Keycloak**, along with a **relational PostgreSQL database** using **isolated database schemas per module**.

Modules communicate through **event-driven asynchronous messaging via RabbitMQ** and follow well-established architectural patterns, including **Vertical Slice Architecture (VSA)**, **Domain-Driven Design (DDD)**, **CQRS**, and the **Outbox Pattern**.

## What is included in this repository
This repository demonstrates the implementation of the following architectural patterns:
* **Modular Monoliths Architecture**
* **Vertical Slice Architecture (VSA)**
* **Domain-Driven Design (DDD)**
* **Command Query Responsibility Segregation (CQRS)**
* **Outbox Pattern for Reliable Messaging**

#### Catalog Module
The Catalog module includes:
* ASP.NET Core **Minimal APIs** and latest features of **.NET8** and **C# 12**
* **VSA** with feature-based folder organization
* **CQRS** implemented using the **MediatR library**
* **CQRS validation pipeline behaviors** using **MediatR** and **FluentValidation**
* **Entity Framework Core** approach and migrations on **PostgreSQL**
* **Carter** for Minimal API endpoint definitions
* Cross-cutting concerns such as **Logging**, **Global Exception Handling** and **Health Checks**

#### Basket Module
The Basket module includes:
* **Redis** used as a distributed cache layered over PostgreSQL
* Implementation of **Proxy**, **Decorator** and **Cache-aside** patterns
* Publishing BasketCheckoutEvent messages to **RabbitMQ** via **MassTransit library**
* Implementation of the **Outbox Pattern** to ensure reliable messaging for the checkout use case

#### Module Communications;
* **Synchronous communication** between Catalog and Basket modules using in-process method calls via public application interfaces
* **Asynchronous communication** using **RabbitMQ** and **MassTransit**, for example to propagate price updates between Catalog and Basket modules

#### Identity Module
The Identity module includes:
* **OAuth 2.0** and **OpenID Connect** authentication flows implemented with **Keycloak**
* Keycloak configured as an identity provider via **Docker Compose**
* **JWT Bearer authentication** integrated with ASP.NET Core using OpenID Connect

#### Ordering Module
The Ordering module includes:
* Implementation of **DDD**, **CQRS**, and **Clean Architecture** best practices
* Use of the **Outbox Pattern** for reliable processing of basket checkout events

#### Migrate to Microservices; 
The architecture is designed to support gradual migration to microservices using the Strangler Fig Pattern, allowing each module to be extracted into an independent service when scaling or organizational needs require it.


## Run The Project
You will need the following tools:

* [Visual Studio 2022](https://visualstudio.microsoft.com/downloads/)
* [.Net 8 SDK or later](https://dotnet.microsoft.com/download/dotnet-core/8)
* [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Installing
Follow these steps to set up the development environment (ensure Docker Desktop is running):
1. Clone the repository
2. Configure Docker resources:
* **Memory**: 4 GB
* **CPU**: 2 cores
3. At the solution root, set **docker-compose** as the startup project and run it without debugging or execute the following command:

```csharp
docker-compose -f docker-compose.yml -f docker-compose.override.yml up -d
```

4. Wait until all services are up and running (some services may take additional time on first startup)
5. Access the Shopping Web API at `https://localhost:6060` using Postman


## Authors
* **Voxwaj** - [DuongNhatKhoa04](/https://github.com/DuongNhatKhoa04)