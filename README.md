# estore

Welcome to the **estore** project! This is a simple Java application demonstrating the functionality of an e-commerce 
application (API and client) using a service-oriented architecture.

## Project Structure and Architecture

This application uses a service-oriented architecture (SOA). An MVC client calls API endpoints which in turn call "service" methods. The facade pattern is implemented in these service methods which encapsulate logic and CRUD calls using the DAO classes. The DAO then performs CRUD operations on the MySQL database.

**MVC Client —> API Endpoints —> Service Facade —> DAO —> Database**

| Folder  | Description |
| --- | --- |
| db | Contains SQL scripts for creating the MySQL database and tables. |
| com.estore.entity | Contains entity model classes that are mapped to the database tables and fields using the _Java Persistence API_ implemented using the _Hibernate_ ORM. |
| com.estore.dto | These classes extend the _RepresentationModel_ Spring base class which adds a _Links_ collection for HATEOAS functionality. |
| com.estore.dao | Contains DAO classes with methods for performing CRUD operations using the Hibernate _SessionFactory_. A base DAO is implemented using Java generics. This allows for a consistent implementation of standard methods (e.g., get, getAll, create, update, delete) while avoiding code duplication. |
| com.estore.service | Contains the implementation of the facade pattern. While the DAO methods correspond to CRUD operations, the service methods encapsulate the actions and logic required to perform operations, such as placing an order which could call multiple DAO methods. DAO dependencies are injected into these service classes using _javax.inject_ which allows for mocking in unit tests. |
| com.estore.controller.api | Contains web service endpoints using Spring MVC (not Spring Boot). |
| test/java | Contains unit and integration tests using the JUnit framework and Mockito. |
| com.estore.controller.client | Contains Spring MVC controllers for the client app. |
| resources | Contains client-side JavaScript and CSS assets. Also contains Thymeleaf views and fragments. |

## Tech

**Server Side:**

- Spring MVC
- Spring Security
- Thymeleaf (template engine)
- Hibernate (ORM)
- JUnit (testing framework)
- Mockito (mocking framework)

**Client Side:**

- npm (to manage third-party JavaScript packages)
- Webpack (module bundler)
- jQuery
- Axios (promise-based HTTP client)
- Bootstrap (UI framework) + Bootswatch (themes)
- Babel (transpiler for browser compatibility)
- ESLint (linter for JavaScript)

**Hosting:**

- AWS Elastic Beanstalk
- AWS RDS for MySQL

## Testing

**Integration tests:**

Used for testing methods that interact with the database when performing CRUD operations

**Unit tests:**

Used for testing application logic. When testing service methods that interact with the database, the DAO methods are mocked using the Mockito mocking framework, which means we can avoid calling the database while testing different scenarios. DAO dependencies are injected into these service classes using **javax.inject** which allows for mocking in unit tests.

**Manual tests:**

Postman was used to manually test web service endpoints.

## Caching

Server-side caching is implemented using Spring's caching abstraction with the **@Cacheable** annotation. This improves performance by reducing unnecessary database calls. It also allows for advanced caching features, such as a Redis Cache.

## Exception Handling

Custom exception handlers are used for throwing exceptions such as "ProductNotFoundException" which will include helpful details about the error, such as the product ID that was not found. Details can be logged to an error or exception log.

## HATEOAS

HATEOAS is implemented using Spring's API. The link creation occurs in the DTO classes to prevent bloated controllers and service methods.

## Security

- oAuth2 protocol
- Social Identity Providers such as Google
- Auth0 which provides authentication and authorization as a service
- Spring Security framework

## License

[MIT license](LICENSE)
