# Zepto Order Service

A Spring Boot backend application for managing customer orders. This project is part of a hands-on backend development journey focused on REST APIs, persistence with Spring Data JPA, and order-service design.

> **Project status:** Under development. Check the source code for the latest implemented endpoints and integrations.

## Tech Stack

- **Java:** 21
- **Spring Boot:** 4.1.0
- **Spring Web MVC:** REST API development
- **Spring Data JPA / Hibernate:** ORM and database access
- **MySQL:** Relational database
- **Apache Kafka:** Messaging dependency for event-driven integration
- **Maven:** Build and dependency management
- **Postman:** API testing

## Key Learning Areas

- Building REST endpoints with Spring Boot
- Separating controller, service, repository, entity, and request/response DTO responsibilities
- Persisting order data using Spring Data JPA and MySQL
- Exploring Hibernate query behavior, including the N+1 query problem
- Comparing fetching approaches such as `JOIN FETCH` and `@EntityGraph` where implemented
- Preparing an order service for event-driven communication using Kafka

*Only features implemented in the source code are available at runtime; some learning areas may still be in progress.*

## Architecture

The application follows a layered Spring Boot structure:

```text
Client (Postman / REST client)
          |
          v
     Controller
          |
          v
      Service
          |
          v
     Repository
          |
          v
 MySQL Database
```

- **Controller:** Receives HTTP requests and returns HTTP responses.
- **Service:** Holds order-related business logic.
- **Repository:** Performs persistence operations through Spring Data JPA.
- **Entity:** Represents persisted domain data.
- **DTOs:** Define request and response payloads when used by the API.

## Prerequisites

Install the following before running the project:

- JDK 21
- MySQL Server
- Git
- Maven (or use the included Maven Wrapper)
- An API client such as Postman

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/ALOKKUMARSAW/zepto-order-service.git
cd zepto-order-service
```

### 2. Create a MySQL database

Create a database for the application. For example:

```sql
CREATE DATABASE zepto_order_db;
```

If your local configuration uses a different database name, use that name instead.

### 3. Configure the database

Open `src/main/resources/application.properties` and configure your local MySQL connection. Add or update the following properties to match your environment:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/zepto_order_db
spring.datasource.username=${DB_USERNAME:root}
spring.datasource.password=${DB_PASSWORD:your_password}

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

Replace `your_password` with your local MySQL password. For a shared or production environment, use environment variables or a secret manager instead of committing credentials.

If the project already has database settings, preserve the existing values and update only what is needed for your machine.

### 4. Run the application

**Windows:**

```bat
mvnw.cmd spring-boot:run
```

**macOS / Linux:**

```bash
./mvnw spring-boot:run
```

Alternatively, run the main Spring Boot application class from your IDE.

Spring Boot uses port `8080` by default unless the application configuration overrides it. To run on another port, add `--server.port=8082` to the command:

```bash
mvnw.cmd spring-boot:run -Dspring-boot.run.arguments="--server.port=8082"
```

### 5. Test the API

Open Postman and send requests to the endpoints defined in the project's controller classes. Confirm the application port and endpoint mappings before testing.

## API Documentation

The exact routes and payloads are defined by the controller and request/response classes in the source code. Document them here as the API evolves.

| Operation | HTTP method | Endpoint |
|---|---|---|
| Create an order | `POST` | Use the create-order mapping in the controller |
| Get order(s) | `GET` | Use the read-order mapping in the controller |
| Update an order | `PUT` / `PATCH` | If implemented |
| Delete an order | `DELETE` | If implemented |

Example JSON shape for an order request (adjust field names and required values to match your `OrderRequest` class):

```json
{
  "customerId": 101,
  "customerName": "Amit Kumar",
  "productId": 501,
  "productName": "Wireless Earbuds",
  "quantity": 2,
  "amount": 1999.00,
  "deliveryAddress": "Bengaluru, Karnataka"
}
```

## Database and Query Performance

For learning and testing database access:

- Review the entity relationships and repository queries.
- Enable SQL logging locally to inspect Hibernate-generated queries.
- Check for repeated queries that may indicate an N+1 problem.
- Where supported by the implementation, compare regular relationship loading with `JOIN FETCH` or `@EntityGraph`.
- Avoid exposing sensitive customer or order data in logs.

## Kafka

The project includes the Spring Boot Kafka dependency. Kafka producer/consumer behavior requires the corresponding code and broker configuration to be implemented and enabled.

If Kafka is used locally, configure the bootstrap server in `application.properties`, for example:

```properties
spring.kafka.bootstrap-servers=localhost:9092
```

Start Kafka using your chosen local setup before testing any Kafka-based flow. The configuration above alone does not create topics or implement message publishing/consumption.

## Build and Test

Build the project:

```bash
mvnw.cmd clean package
```

Run tests:

```bash
mvnw.cmd test
```

On macOS or Linux, use `./mvnw` instead of `mvnw.cmd`.

## Troubleshooting

- **Database connection error:** Confirm MySQL is running and the database name, username, password, and port are correct.
- **Unknown database:** Create the database configured in `spring.datasource.url`.
- **Port already in use:** Start the app with a different `--server.port` value.
- **404 Not Found:** Verify the controller's `@RequestMapping` and method-level mappings.
- **400 Bad Request:** Check the JSON field names, data types, validation rules, and required fields.
- **Kafka connection error:** Verify the broker is running and `spring.kafka.bootstrap-servers` points to the correct address.

## Future Improvements

Potential next steps, depending on project goals:

- Add request validation and centralized exception handling
- Document endpoints with OpenAPI / Swagger
- Add unit and integration tests
- Add pagination and sorting for order queries
- Add Kafka event publishing and consuming for order lifecycle events
- Add Docker configuration for repeatable local setup

## Author

**Alok Kumar Saw**

- GitHub: [ALOKKUMARSAW](https://github.com/ALOKKUMARSAW)
- LinkedIn: [Alok Kumar Saw](https://www.linkedin.com/in/alok-kumar-saw/)
