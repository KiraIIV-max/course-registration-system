# System Architecture — University Course Registration System

**Version:** Draft 1.1

**Date:** November 26, 2026

**Status:** Infrastructure verified locally — business architecture pending final approval.

## 1. Architecture Overview

The University Course Registration System follows a Microservices Architecture.

The system consists of a React frontend, an API Gateway, a Eureka Discovery Server, four independent business microservices, and PostgreSQL data storage.

The architecture separates business responsibilities into independent services to improve maintainability, modularity, scalability, and team collaboration.

The Eureka Discovery Server and API Gateway have been implemented and verified locally.

Business microservices, security implementation, database integration, and containerized deployment remain subject to implementation and verification.

## 2. Logical Architecture

The system follows the following communication structure:

- React Frontend communicates with the API Gateway.
- API Gateway routes requests to the four business microservices.
- Eureka Discovery Server provides service registration and discovery.
- Spring Cloud LoadBalancer resolves available service instances.
- OpenFeign supports selected inter-service communication.
- Each business microservice owns its database or schema.

### Architecture Diagram

The final Architecture Diagram must be prepared and reviewed by the team using draw.io.

It must include:

- React Frontend.
- API Gateway.
- Eureka Discovery Server.
- Auth Service.
- Course Service.
- Enrollment Service.
- Grade Service.
- PostgreSQL databases or isolated schemas.
- Service ports.
- Client request flows.
- Service registration and discovery.
- Inter-service communication.
- Docker containers and networking.

The diagram must remain consistent with the actual implementation.

## 3. Component Responsibilities

| Component | Responsibility |
|---|---|
| React Frontend | User interface for students and administrators |
| API Gateway | External entry point, request routing, and load balancing |
| Eureka Discovery Server | Service registration and discovery |
| Auth Service | User identity, authentication, student management, and authorization support |
| Course Service | Course catalog, course management, capacity, and availability |
| Enrollment Service | Student registration, withdrawal, and enrollment rules |
| Grade Service | Grade assignment, validation, and retrieval |
| PostgreSQL | Persistent relational data storage |

Current implementation status is maintained in [PROGRESS.md](../PROGRESS.md).

## 4. Backend Repository Layout

    backend/
    |-- discovery-server/
    |-- api-gateway/
    |-- auth-service/
    |-- course-service/
    |-- enrollment-service/
    `-- grade-service/

Additional project directories:

- frontend/
- docker/
- docs/

Each backend service is maintained as an independent Spring Boot Maven project.

## 5. Service Names and Ports

| Component | Application Name | Port |
|---|---|---:|
| Eureka Discovery Server | discovery-server | 8761 |
| API Gateway | api-gateway | 8090 |
| Auth Service | auth-service | 8081 |
| Course Service | course-service | 8082 |
| Enrollment Service | enrollment-service | 8083 |
| Grade Service | grade-service | 8084 |
| PostgreSQL | postgresql | 5432 |
| React Frontend | frontend | 5173 |

Discovery Server and Gateway ports have been verified locally.

Remaining ports represent the proposed development assignments.

## 6. Communication Design

### 6.1 Client-to-Service Communication

The React frontend communicates with the API Gateway through REST APIs.

External clients must not bypass the Gateway to access business APIs directly.

Gateway base URL:

http://localhost:8090

Authentication and authorization must be enforced at appropriate backend boundaries.

### 6.2 Gateway Routing

The following routes are configured in the API Gateway:

| Route ID | Path | Destination |
|---|---|---|
| auth-route | /api/auth/** | lb://auth-service |
| students-route | /api/students/** | lb://auth-service |
| course-route | /api/courses/** | lb://course-service |
| enrollment-route | /api/enrollments/** | lb://enrollment-service |
| grade-route | /api/grades/** | lb://grade-service |

The lb:// scheme enables service-name-based routing through Spring Cloud LoadBalancer.

The configured routes preserve incoming request paths.

Business service endpoints must match the agreed route prefixes.

Successful request forwarding remains pending verification.

### 6.3 Service Discovery

Netflix Eureka provides service registration and discovery.

Discovery Server configuration:

- Application name: discovery-server.
- Port: 8761.
- Self-registration: disabled.
- Registry fetching: disabled.

Eureka Dashboard:

http://localhost:8761

Eureka Registry:

http://localhost:8761/eureka/

The API Gateway has successfully registered with Eureka.

All business microservices must register using their assigned application names.

### 6.4 Inter-Service Communication

REST over HTTP is the planned communication mechanism.

Spring Cloud OpenFeign will support selected synchronous service calls.

Proposed service dependencies:

| Calling Service | Destination | Purpose |
|---|---|---|
| Enrollment Service | Auth Service | Verify student information |
| Enrollment Service | Course Service | Verify course availability and capacity |
| Grade Service | Enrollment Service | Verify valid student enrollment |

These dependencies are proposed and must be reviewed before implementation.

Inter-service communication must:

- Use service discovery where applicable.
- Avoid hardcoded service hostnames and ports.
- Apply appropriate authentication and authorization.
- Configure reasonable timeouts.
- Handle unavailable downstream services.
- Avoid uncontrolled retries.
- Avoid direct access to another service's database.

### 6.5 Load Balancing

Spring Cloud LoadBalancer is included in the API Gateway.

When multiple instances of a destination service are registered, the Gateway can resolve and select available instances.

Verification requires:

1. Start multiple instances of the same business microservice.
2. Configure different instance ports.
3. Register all instances using the same application name.
4. Send repeated requests through the API Gateway.
5. Verify that requests reach multiple instances.

Actual load distribution has not yet been verified.

## 7. Data Ownership

Each business microservice owns its entities and persistence schema.

The physical database provisioning strategy remains subject to team approval.

| Service | Proposed Owned Data |
|---|---|
| Auth Service | Users, students, roles, and credential metadata |
| Course Service | Courses, course capacity, and catalog information |
| Enrollment Service | Enrollment records and enrollment status |
| Grade Service | Grade records associated with valid enrollments |

### Database Architecture Rules

- Use PostgreSQL.
- Maintain independent database or schema ownership.
- Use Spring Data JPA and Hibernate.
- Define appropriate primary keys and constraints.
- Avoid direct cross-service database access.
- Use stable identifiers for cross-service references.
- Validate cross-service references through service APIs.
- Use transactions where required.
- Do not assume foreign keys across independent databases.
- Keep database credentials outside source control.

Database names, schemas, and migration strategies must be finalized before implementation.

## 8. Critical Business Flows

The following flows represent proposed business behavior.

### 8.1 Student Registration and Authentication

1. A student submits registration information through the frontend.
2. The frontend sends the request to the API Gateway.
3. The Gateway routes the request to Auth Service.
4. Auth Service validates the submitted information.
5. Auth Service hashes the password and creates the user record.
6. The service returns an appropriate registration response.
7. During login, Auth Service verifies credentials and issues a JWT after successful authentication.

### 8.2 Course Enrollment

1. An authenticated student submits an enrollment request through the Gateway.
2. Enrollment Service verifies authorization.
3. Enrollment Service validates the student and course.
4. Enrollment Service checks applicable enrollment rules.
5. Duplicate enrollment is prevented.
6. Course capacity is verified using the agreed consistency strategy.
7. A valid enrollment is persisted.
8. The service returns confirmation or an explicit rejection reason.

### Enrollment Consistency Risk

Checking course capacity in one service and creating enrollment records in another service can cause overbooking under concurrent requests.

The team must design an atomic capacity reservation or equivalent consistency mechanism.

The design must also handle failures occurring between capacity reservation and enrollment persistence.

The final strategy must be documented and tested.

### 8.3 Course Withdrawal

1. An authenticated student submits a withdrawal request.
2. Enrollment Service verifies ownership and enrollment status.
3. Applicable withdrawal rules are validated.
4. The enrollment is updated or removed according to the approved requirements.
5. Course capacity is adjusted using the agreed consistency strategy.
6. The service returns the result.

### 8.4 Grade Assignment

1. An authorized user submits a grade through the Gateway.
2. Grade Service verifies permissions.
3. Grade Service verifies the target enrollment.
4. The submitted grade is validated against the approved grading scale.
5. Grade Service stores the valid result.
6. The service returns an appropriate response.

Grade-entry permissions and grading rules must match the approved SRS.

## 9. Security Architecture

The proposed security architecture uses Spring Security and JWT.

### Authentication

Auth Service is responsible for:

- Student registration.
- Credential validation.
- BCrypt password hashing.
- JWT generation.
- User identity and role management.

### Authorization

The proposed roles are:

- STUDENT.
- ADMIN.

Protected endpoints must enforce role-based authorization and resource ownership rules.

JWT validation responsibilities must be coordinated between the API Gateway and business microservices.

Business services must enforce their applicable authorization rules.

### Security Requirements

- Never store plaintext passwords.
- Never expose password hashes through APIs.
- Never commit JWT signing secrets or database credentials.
- Validate authentication tokens.
- Restrict access to private academic records.
- Prevent unauthorized grade modifications.
- Avoid logging passwords, tokens, and sensitive information.
- Configure an appropriate CORS policy.
- Secure internal service communication.

The final JWT signing configuration, token lifetime, claims, and internal authentication strategy require team approval.

See [API and Security Standards](../API_STANDARDS.md).

## 10. Backend Layered Architecture

Business microservices should follow the following layered structure:

Controller → Service → Repository → Database

### Controller Layer

- Receives HTTP requests.
- Validates request DTOs.
- Delegates business operations.
- Returns appropriate HTTP responses.

### Service Layer

- Implements business logic.
- Enforces business rules.
- Coordinates transactions.
- Handles required inter-service communication.

### Repository Layer

- Handles database access.
- Uses Spring Data JPA.
- Executes persistence operations.

### DTO Layer

- Defines API request and response contracts.
- Prevents direct exposure of persistence entities.
- Supports input validation.

### Exception Handling

Business microservices should implement consistent exception handling using @RestControllerAdvice and appropriate security handlers.

Error responses must follow the approved shared API contract.

## 11. Cross-Cutting Concerns

The project will implement at least two Spring AOP aspects.

### Logging Aspect

Records relevant application events and selected method execution information.

An appropriate advice type, such as @Before, will be used.

### Execution Time Aspect

Measures the execution duration of selected operations using @Around.

### Additional Concerns

- Input validation.
- Global exception handling.
- Security.
- API documentation.
- Application health monitoring.
- Automated testing.
- Consistent error responses.

AOP pointcuts, advice types, and verified execution results must be documented.

## 12. OCL and Business Rule Enforcement

The project must document at least five OCL constraints.

The constraints must cover applicable:

- Invariants.
- Preconditions.
- Postconditions.

Potential constraint areas include:

- Positive course capacity.
- Maximum enrollment capacity.
- Duplicate enrollment prevention.
- Valid grade ranges.
- Enrollment eligibility.
- Successful enrollment state.

Approved constraints must match the UML Class Diagram.

Their implementation must be enforced through appropriate mechanisms:

- Request validation.
- Service-layer business logic.
- Database constraints.
- Transaction and concurrency controls.

The final report must identify where each approved constraint is enforced.

## 13. Deployment Architecture

Docker will be used to package the application components.

### Planned Containers

| Container | Responsibility |
|---|---|
| discovery-server | Eureka service registry |
| api-gateway | API routing and load balancing |
| auth-service | Authentication and student management |
| course-service | Course management |
| enrollment-service | Enrollment management |
| grade-service | Grade management |
| PostgreSQL container or containers | Database persistence |
| frontend | React application, if included in deployment |

### Docker Networking

Containers will communicate through the Docker Compose network.

Internal container communication must use appropriate service names rather than assuming localhost refers to another container.

For example, the proposed containerized Eureka Registry URL is:

http://discovery-server:8761/eureka/

### Deployment Requirements

- Dockerfile for each backend service.
- Compatible Java runtime images.
- Environment-based configuration.
- Secure credential management.
- Persistent database volumes.
- Appropriate health checks.
- Service startup coordination.
- Verified Eureka registration.
- Verified Gateway routing.

The API Gateway should be the public entry point for business APIs.

Other service ports should only be exposed when required for development or administration.

Dockerfiles, image versions, networking, and Docker Compose configuration remain pending implementation and testing.

## 14. Technology Baseline

| Technology | Version or Choice |
|---|---|
| Java JDK | 25.0.1 |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| Maven | 3.10.1 |
| PostgreSQL | 18.7 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Frontend | React + Vite |
| Service Discovery | Netflix Eureka |
| API Gateway | Spring Cloud Gateway Server WebFlux |
| Load Balancing | Spring Cloud LoadBalancer |
| Service Communication | OpenFeign |
| ORM | Spring Data JPA and Hibernate |
| Security | Spring Security, JWT, and BCrypt |
| Cross-Cutting Concerns | Spring AOP |
| API Documentation | springdoc OpenAPI and Swagger UI |

These technologies represent the recorded team baseline and intended architectural choices.

Not every component or dependency has been implemented or verified.

## 15. Implemented Infrastructure

### 15.1 Eureka Discovery Server

**Directory:** backend/discovery-server

**Main Class:** com.courseregistration.discovery.DiscoveryServerApplication

**Application Name:** discovery-server

**Port:** 8761

**Implementation Commit:** 4b8cd1f

Verified locally:

- Maven build.
- Automated application tests.
- Application startup.
- Eureka Dashboard.

### 15.2 API Gateway

**Directory:** backend/api-gateway

**Main Class:** com.courseregistration.gateway.ApiGatewayApplication

**Application Name:** api-gateway

**Port:** 8090

**Implementation Commit:** de72ed2

Configured dependencies:

- Spring Cloud Gateway Server WebFlux.
- Eureka Discovery Client.
- Spring Cloud LoadBalancer.
- Spring Boot Actuator.

Verified locally:

- Maven build.
- Automated application tests.
- Application startup.
- Eureka registration.
- Gateway health endpoint.
- Initial unavailable-service routing behavior.

Successful routing to business services and actual load distribution remain pending.

## 16. Architecture Verification

### Local Infrastructure Startup

Start Eureka from the repository root in the first CMD window:

    cd backend\discovery-server
    mvn clean verify
    mvn spring-boot:run

Start API Gateway from the repository root in a second CMD window:

    cd backend\api-gateway
    mvn clean verify
    mvn spring-boot:run

Verify the Gateway health endpoint:

    curl -i http://localhost:8090/actuator/health

Expected result: HTTP 200 with health status UP.

Verify Eureka registration through:

http://localhost:8761

### Initial Routing Verification

    curl -i http://localhost:8090/api/auth/test
    curl -i http://localhost:8090/api/students
    curl -i http://localhost:8090/api/courses
    curl -i http://localhost:8090/api/enrollments
    curl -i http://localhost:8090/api/grades

HTTP 503 is expected while destination business services are unavailable.

This does not confirm successful forwarding or load balancing.

## 17. Architecture Decisions Requiring Approval

| Decision | Responsible Members |
|---|---|
| Final user roles and permissions | Member 2 and Team |
| JWT validation and signing strategy | Members 1 and 2 |
| API contracts and endpoint definitions | Backend Members |
| Enrollment capacity consistency | Members 3 and 4 |
| Grade and enrollment relationships | Members 4 and 5 |
| Database isolation and migrations | Backend Members |
| Internal authentication strategy | Members 1 and 2 |
| Timeout and retry policies | Backend Members |
| CORS configuration | Members 1, 2, and 6 |
| Docker deployment topology | Member 1 |
| Final Architecture Diagram | Member 1 and Team |

These responsibilities are coordination proposals and must be reviewed by the team.

## 18. Next Infrastructure Milestone

The next infrastructure milestone is containerization.

Planned work:

1. Create Eureka Discovery Server Dockerfile.
2. Create API Gateway Dockerfile.
3. Prepare initial Docker Compose configuration.
4. Configure container networking.
5. Verify Eureka startup inside Docker.
6. Verify Gateway startup inside Docker.
7. Verify Gateway registration with containerized Eureka.
8. Document verified startup instructions.
9. Integrate business services as they become available.

Detailed task status is maintained in PROGRESS.md.

## 19. Related Documentation

| Document | Purpose |
|---|---|
| [README.md](../../README.md) | Project overview and startup instructions |
| [PROGRESS.md](../PROGRESS.md) | Team progress and individual contributions |
| [DEVELOPMENT_GUIDE.md](../DEVELOPMENT_GUIDE.md) | Development setup and workflow |
| [API_STANDARDS.md](../API_STANDARDS.md) | Shared API and security conventions |
| [SRS.md](../requirements/SRS.md) | Software Requirements Specification |
| docs/database/ | Database design and ERD |
| docs/api/ | Endpoint contracts and testing |
| docs/uml/ | UML diagrams and OCL constraints |

---

**Project:** University Course Registration System

**Architecture:** Microservices

**Architecture Owner:** Member 1

**Approval Status:** Draft — subject to team review.

This document must be updated when architectural decisions are approved or implementation changes occur.
