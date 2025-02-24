## **API Development Specification**

This document outlines the structure, practices, and principles to guide the development of a modular, maintainable, and scalable API. The goal is to keep the design simple, promote modularity, and adhere to clean architecture principles, while allowing flexibility for future growth.

### **1. Project Structure**

The API should be organized around **services** (domains) to encapsulate functionality and promote separation of concerns. Each service will have its own folder containing all relevant components.

#### **Folder Structure**
```bash
app
├── services
│   ├── <domain_name>
│   │   ├── controllers
│   │   │   └── <files>
│   │   ├── domain
│   │   │   └── <files>
│   │   ├── helpers
│   │   │   └── <files>
│   │   ├── repositories
│   │   │   └── <files>
│   │   ├── serializers
│   │   │   └── <files>
├── models
│   ├── application_record.rb
│   └── <files>
├── <others, rails defaults>
└── lib
    └── <others, rails defaults>
```

-   **`services/`**: Contains all domain-specific components to isolate concerns and maintain modularity.
-   **`models/`**: Contains Active Record models (or equivalent ORM).
-   **`lib/`**: Utility functions, shared code, or custom libraries not tied to a specific domain.

### **2. Principles**

#### **Modularity**

-   Each domain should be self-contained, exposing only the interfaces necessary for interaction with other parts of the system.
-   Avoid direct cross-domain dependencies; communication should occur through public APIs or service interfaces.

#### **Separation of Concerns**

-   **Controllers** handle HTTP requests and delegate logic to domain or repository layers.
-   **Repositories** encapsulate data access and business validations.
-   **Domain** contains core business rules, logic, and computations.
-   **Serializers** format responses for the client.
-   **Helpers** provide reusable utilities specific to a domain.

#### **Flexibility**

-   Keep entities ORM-independent if possible. If using an ORM (e.g., Active Record), use models for database validations and repositories for business logic.
-   The system should allow for easy refactoring, scaling, or swapping out components (e.g., switching from one database to another).

### **3. Components**

#### **Controllers**

-   Handle HTTP requests and responses.
-   Contain minimal logic; delegate to domain services or repositories.
-   Follow RESTful or GraphQL conventions, depending on the API style.

#### **Domain**

-   Encapsulates business rules, computations, and logic for a specific domain.
-   Includes entities, value objects, and domain services (if needed).
-   Focuses on the **what** (behavior) rather than the **how** (implementation).

#### **Repositories**

-   Handle all interactions with the data source (e.g., database, external API).
-   Encapsulate queries and data transformation logic.
-   Implement business validations not covered by the database.

#### **Serializers**

-   Prepare and format data for client responses.
-   Abstract away internal structures to ensure consistent and clean API responses.

#### **Helpers**

-   Provide domain-specific reusable functions (e.g., formatting, calculations).
-   Avoid adding business logic here.


### **4. General Development Guidelines**

#### **API Design**

-   Follow RESTful principles or GraphQL, depending on the project needs.
-   Maintain consistency in endpoint naming and behavior.
    -   Example: `GET /<resource>` for retrieving data, `POST /<resource>` for creating, etc.
-   Use proper HTTP status codes for responses (e.g., `200 OK`, `404 Not Found`, `422 Unprocessable Entity`).

#### **Error Handling**

-   Centralize error handling for better maintainability.
-   Return standardized error responses:
    
{
  "error": "Validation failed",
  "details": ["Field A is required", "Field B must be unique"]
}

#### **Validation**

-   Use model/database validations for structural integrity.
-   Perform business validations in repositories or domain services.

#### **Authentication & Authorization**

-   Implement authentication via tokens (e.g., JWT, OAuth) or sessions.
-   Handle authorization using domain rules or permission layers (e.g., role-based access).

#### **Testing**

-   Write unit tests for:
    -   Repositories (business logic validation).
    -   Domain services (isolated business rules).
-   Write integration tests for:
    -   Controllers and endpoints.
    -   Interactions between services.

#### **Documentation**

-   Use tools like Swagger or GraphQL schema introspection for API documentation.
-   Provide clear examples for each endpoint, including request and response samples.

----------

### **5. Example Workflow**

1.  **Request Flow**:
    
    -   A client sends a request to an endpoint (e.g., `POST /resources`).
    -   The controller processes the request and validates the input.
    -   The controller calls the appropriate service/repository in the domain layer.
    -   The repository interacts with the database or another data source.
    -   The response is formatted by the serializer and returned to the client.
2.  **Inter-Service Communication**:
    
    -   Services communicate through defined interfaces (e.g., `ServiceA::Interface.call(params)`).
    -   Avoid direct cross-domain data access to maintain encapsulation.

### **6. Example Directory for a Domain**

#### Directory: `services/<domain_name>`
```bash
services
├── user
│   ├── controllers
│   │   └── user_controller.rb
│   ├── domain
│   │   ├── user_entity.rb
│   │   └──user_logic.rb
│   ├── helpers
│   │   └── user_helper.rb
│   ├── repositories
│   │   └── user_repository.rb
│   ├── serializers
│   │   └── user_serializer.rb
```
      
#### File Responsibilities

-   **`controllers/user_controller.rb`**: Handles requests such as `POST /users` or `GET /users`.
-   **`domain/user_entity.rb`**: Represents the user and its business rules.
-   **`domain/user_logic.rb`**: Contains reusable business logic (e.g., password reset, validation rules).
-   **`repositories/user_repository.rb`**: Manages data operations like `find_user_by_email` or `create_user`.
-   **`serializers/user_serializer.rb`**: Formats user data for the API response.
-   **`helpers/user_helper.rb`**: Provides utility functions like formatting names.

### **7. Benefits**

-   **Modular Design**: Domains are isolated, making them easy to scale and maintain.
-   **Testability**: Clear separation of concerns makes writing unit and integration tests simpler.
-   **Flexibility**: Services can evolve independently without affecting other parts of the application.
-   **Simplicity**: The design avoids overengineering while adhering to DDD principles.



