# Course Registration System — API & Security Standards

## 1. Purpose

This document defines shared development, API, security, and error-handling standards for all microservices in the Course Registration System.

All team members should follow these standards to ensure consistency, maintainability, and successful integration.

**Status:** Proposed team standards — pending team approval.

## 2. Service Names and Ports

| Service | Application Name | Port |
|---|---|---:|
| Discovery Server | discovery-server | 8761 |
| API Gateway | api-gateway | 8090 |
| Authentication Service | auth-service | 8081 |
| Course Service | course-service | 8082 |
| Enrollment Service | enrollment-service | 8083 |
| Grade Service | grade-service | 8084 |
| PostgreSQL | postgresql | 5432 |
| React Frontend | frontend | 5173 |

All business microservices must register with Eureka using their assigned application names.

## 3. API Gateway

The API Gateway is the main entry point for frontend applications and external API clients.

Base URL:

`http://localhost:8090`

### Routing

| Path | Destination |
|---|---|
| /api/auth/** | lb://auth-service |
| /api/students/** | lb://auth-service |
| /api/courses/** | lb://course-service |
| /api/enrollments/** | lb://enrollment-service |
| /api/grades/** | lb://grade-service |

Service discovery and client-side load balancing are handled through Eureka and Spring Cloud LoadBalancer.

## 4. Authentication and Authorization

- Authentication: Spring Security and JWT.
- Authorization: Role-Based Access Control (RBAC).
- Password hashing: BCrypt.
- Token type: Bearer Token.
- Proposed access token lifetime: 60 minutes.
- Token validation: API Gateway and protected business microservices.
- Signing keys and database credentials must be provided through environment variables.
- Passwords and tokens must never appear in application logs.
- Password hashes must never be returned in API responses.

### 4.1 User Roles

**STUDENT**

- View and update permitted personal profile information.
- Browse available courses.
- Register for eligible courses.
- Withdraw from courses according to business rules.
- View personal enrollments and grades.

**ADMIN**

- Manage courses.
- Manage authorized student information.
- Manage enrollments.
- Add and update grades.

### 4.2 Public Endpoints

- POST /api/auth/register
- POST /api/auth/login

Public registration must not allow users to assign themselves the ADMIN role.

All other business endpoints require authentication unless explicitly approved.

### 4.3 Authorization Header

`Authorization: Bearer <access-token>`

The backend must verify token validity, expiration, roles, and resource ownership where applicable.

Frontend authorization checks are not a replacement for backend security.

## 5. Backend Architecture

Each business microservice should follow a layered architecture:

Controller → Service → Repository → Database

### Package Structure

- controller
- service
- repository
- entity
- dto
- exception
- config
- security
- client — when inter-service communication is required.
- aspect — when AOP is required.

### Responsibilities

- Controller: Handle HTTP requests and responses.
- Service: Implement business logic and business rules.
- Repository: Handle database operations.
- Entity: Represent persistent database models.
- DTO: Represent API requests and responses.
- Exception: Handle application exceptions.
- Config: Manage application configuration.
- Security: Handle authentication and authorization.
- Client: Communicate with other microservices.
- Aspect: Implement cross-cutting concerns.

Entities must not be exposed directly through public REST APIs.

## 6. REST API Conventions

- Use plural resource names.
- Use lowercase endpoint paths.
- Use kebab-case for multi-word paths.
- Use JSON for request and response bodies where applicable.
- Use camelCase for JSON property names.
- Use ISO 8601 timestamps in UTC.
- Use appropriate HTTP methods and status codes.

| Method | Purpose |
|---|---|
| GET | Retrieve resources |
| POST | Create resources or perform defined actions |
| PUT | Replace or fully update resources |
| PATCH | Partially update resources |
| DELETE | Delete resources |

## 7. HTTP Status Codes

| Status | Meaning |
|---|---|
| 200 | Successful request |
| 201 | Resource created |
| 204 | Successful request without response body |
| 400 | Invalid request or validation failure |
| 401 | Missing or invalid authentication |
| 403 | Insufficient permissions |
| 404 | Resource not found |
| 409 | Resource or business-state conflict |
| 500 | Unexpected internal server error |
| 503 | Service unavailable |

## 8. Standard API Responses

Successful requests should return the appropriate response DTO or collection.

A successful creation should return HTTP 201.

A successful operation using HTTP 204 must not include a response body.

### Standard Error Response Fields

| Field | Description |
|---|---|
| timestamp | Error timestamp in UTC |
| status | HTTP status code |
| error | HTTP error description |
| message | Clear error message |
| path | Requested API path |
| fieldErrors | Field-level validation errors, when applicable |

Example validation error:

    {
      "timestamp": "2026-11-26T20:00:00Z",
      "status": 400,
      "error": "Bad Request",
      "message": "Validation failed",
      "path": "/api/courses",
      "fieldErrors": {
        "capacity": "Capacity must be greater than zero"
      }
    }

All business microservices must use consistent exception handling through @RestControllerAdvice.

Gateway and Spring Security errors should follow the same response contract using their appropriate error handlers.

Internal stack traces and sensitive implementation details must not be exposed to clients.

## 9. Validation and Business Rules

- Use Jakarta Bean Validation.
- Validate request DTOs using @Valid.
- Use appropriate validation annotations.
- Enforce business rules inside the service layer.
- Enforce database constraints where applicable.
- Implement all approved OCL constraints in the application.
- Prevent duplicate enrollments.
- Prevent enrollment beyond course capacity.
- Validate grade ranges.
- Verify resource ownership and user permissions.
- Handle concurrent enrollment requests safely.

## 10. Inter-Service Communication

- Use Spring Cloud OpenFeign for synchronous service communication.
- Resolve services through Eureka application names.
- Avoid hardcoded service hostnames and ports.
- Authenticate and authorize internal service requests.
- Propagate user tokens when appropriate and permitted.
- Configure connection and response timeouts.
- Handle unavailable downstream services gracefully.
- Do not directly access another microservice's database.

The internal authentication strategy must be coordinated with the authentication service owner.

## 11. Database Standards

- Database: PostgreSQL.
- ORM: Spring Data JPA and Hibernate.
- Each business microservice owns its database or schema.
- Use snake_case for database table and column names.
- Use primary keys, unique constraints, and appropriate indexes.
- Use transactions for operations requiring atomicity.
- Use Flyway for database migrations, subject to team approval.
- Database credentials must not be committed to GitHub.

## 12. Logging and AOP

The project must include at least two AOP aspects:

- Method execution-time monitoring using @Around.
- Method logging using @Before or another approved advice type.

Logs should include relevant service and operation information without exposing passwords, JWT tokens, or sensitive personal data.

## 13. API Documentation and Testing

Each business microservice must provide:

- Swagger/OpenAPI documentation.
- Endpoint descriptions.
- Request and response examples.
- Expected HTTP status codes.
- Postman requests.
- Successful and unsuccessful test cases.
- Authentication and authorization tests.
- Business-rule validation tests.

External Postman and frontend requests must go through the API Gateway.

## 14. Git and Development Standards

- Main branch: main.
- Active development branch: development.
- Pull the latest changes before starting work.
- Use meaningful commit messages.
- Each member must commit their own contributions.
- Do not commit generated build files, credentials, or secrets.
- Coordinate shared API contract changes with the team.

## 15. Current Infrastructure Status

| Component | Status |
|---|---|
| GitHub Repository | Completed |
| Eureka Discovery Server | Completed |
| API Gateway Initialization | Completed |
| Gateway Health Check | Passed |
| Gateway Eureka Registration | Passed |
| Routing Configuration | Completed |
| Initial Unavailable-Service Tests | Passed |
| Successful Business-Service Routing | Pending |
| Load Balancing Testing | Pending |
| Security Implementation | Pending |
| Docker Integration | Pending |
| Final System Testing | Pending |

Load balancing must be tested using multiple running instances of the same business microservice.

## 16. Team Agreement

All team members should review these standards before implementing their assigned microservices.

Changes to service names, ports, API contracts, security configuration, or error response formats must be coordinated with the infrastructure and integration owner.
