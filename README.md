# University Course Registration System

A microservices-based university course registration platform built using Spring Boot, Spring Cloud, PostgreSQL, and React.

**Course:** Software Engineering II

**Architecture:** Microservices Architecture

**Project Status:** Active Development

**Active Branch:** development

## 1. Project Overview

The University Course Registration System is a web-based application designed to manage university courses, student enrollment, authentication, and academic grades.

The system follows a Microservices Architecture, where independent services handle different business responsibilities and communicate through REST APIs.

The project demonstrates software engineering principles, distributed systems, secure authentication, business-rule enforcement, database management, and containerized deployment.

For current implementation progress and individual contributions, see [Team Progress Tracker](docs/PROGRESS.md).

## 2. Project Objectives

- Build a modular and maintainable university registration platform.
- Apply Microservices Architecture and separation of concerns.
- Implement secure authentication and role-based authorization.
- Manage university courses and academic information.
- Allow students to register for available courses.
- Enforce business rules and data validation.
- Implement service discovery and API routing.
- Implement client-side load balancing.
- Apply Aspect-Oriented Programming (AOP).
- Apply Object Constraint Language (OCL).
- Document and test REST APIs.
- Containerize the application using Docker.
- Support collaborative development using Git and GitHub.

## 3. Main Features

### Authentication and Authorization

- Student registration.
- User login.
- JWT-based authentication.
- BCrypt password hashing.
- Role-based access control.
- Student profile management.
- Protected API endpoints.

### Course Management

- Create courses.
- Update course information.
- Delete courses according to business rules.
- View available courses.
- Manage course capacity.
- Check course availability.
- Search and filter courses, subject to approved requirements.

### Student Enrollment

- Register students in available courses.
- Prevent duplicate enrollment.
- Validate available course capacity.
- Validate enrollment eligibility.
- View student enrollments.
- Withdraw from courses according to business rules.

### Grade Management

- Assign student grades.
- Update grades.
- Retrieve student grades.
- Validate grade ranges.
- Verify enrollment before assigning grades.
- Calculate GPA, subject to approved requirements.

### Frontend

- Login and registration pages.
- Student Dashboard.
- Administrator Dashboard.
- Course management.
- Student enrollment management.
- Grade management.
- Responsive user interface.
- Role-based navigation.

These features represent the planned project scope. Implementation status is maintained separately in the Team Progress Tracker.

## 4. System Architecture

The application consists of four business microservices, a Discovery Server, an API Gateway, a React frontend, and PostgreSQL data storage.

### Architecture Components

| Component | Responsibility |
|---|---|
| React Frontend | User interface |
| API Gateway | Client entry point and request routing |
| Eureka Discovery Server | Service registration and discovery |
| Auth Service | Authentication and student management |
| Course Service | Course management and availability |
| Enrollment Service | Student course registration |
| Grade Service | Academic grade management |
| PostgreSQL | Persistent relational data storage |

### Communication

- Frontend requests are sent through the API Gateway.
- API Gateway routes requests to the appropriate microservice.
- Business microservices register with Eureka.
- Spring Cloud LoadBalancer selects available service instances.
- OpenFeign handles selected synchronous inter-service communication.
- Each business microservice owns its data.

The finalized architecture diagram will be maintained in the architecture documentation.

See [Architecture Documentation](docs/architecture/ARCHITECTURE.md).

## 5. Technology Stack

### Backend

| Technology | Purpose |
|---|---|
| Java 25 LTS | Backend programming language |
| Spring Boot | Backend application framework |
| Spring Cloud Gateway WebFlux | API Gateway |
| Netflix Eureka | Service discovery |
| Spring Cloud OpenFeign | Inter-service communication |
| Spring Cloud LoadBalancer | Client-side load balancing |
| Spring Security | Authentication and authorization |
| JWT | Token-based authentication |
| BCrypt | Password hashing |
| Spring Data JPA | Database access |
| Hibernate | Object-relational mapping |
| PostgreSQL | Relational database |
| Flyway | Database migrations, subject to team approval |
| Jakarta Bean Validation | Input validation |
| Spring AOP | Cross-cutting concerns |
| Maven | Build and dependency management |
| JUnit | Unit testing |
| Mockito | Dependency mocking |
| springdoc OpenAPI | API documentation |

### Frontend

| Technology | Purpose |
|---|---|
| React | User interface development |
| Vite | Frontend build tooling |
| JavaScript | Frontend programming |
| HTML | Page structure |
| CSS | Styling and responsive design |
| Axios or Fetch API | HTTP communication |

### Development Tools

| Tool | Purpose |
|---|---|
| VS Code | Development environment |
| Git | Version control |
| GitHub | Team collaboration |
| Postman | API testing |
| Swagger UI | Interactive API documentation |
| DBeaver | Database management |
| Docker | Containerization |
| Docker Compose | Multi-container orchestration |
| draw.io | UML and architecture diagrams |

### Technology Versions

| Technology | Recorded Team Version |
|---|---|
| Java JDK | 25.0.1 |
| Maven | 3.10.1 |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| PostgreSQL | 18.7 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Git | 2.49.0 |

All team members should use the agreed technology baseline.

Spring Boot and Spring Cloud dependencies should be managed through Spring Boot dependency management and the Spring Cloud BOM.

Each service should include only the dependencies required for its responsibilities.

## 6. Repository Structure

    course-registration-system/
    |-- backend/
    |   |-- discovery-server/
    |   |-- api-gateway/
    |   |-- auth-service/
    |   |-- course-service/
    |   |-- enrollment-service/
    |   `-- grade-service/
    |-- frontend/
    |-- docker/
    |-- docs/
    |   |-- requirements/
    |   |   `-- SRS.md
    |   |-- architecture/
    |   |   `-- ARCHITECTURE.md
    |   |-- uml/
    |   |-- database/
    |   |-- api/
    |   |-- API_STANDARDS.md
    |   |-- DEVELOPMENT_GUIDE.md
    |   `-- PROGRESS.md
    |-- .github/
    |   `-- workflows/
    |-- .editorconfig
    |-- .gitignore
    `-- README.md

Some directories may contain placeholders until their assigned services or documentation are implemented.

## 7. Service Names and Ports

| Service | Application Name | Port |
|---|---|---:|
| Discovery Server | discovery-server | 8761 |
| API Gateway | api-gateway | 8090 |
| Auth Service | auth-service | 8081 |
| Course Service | course-service | 8082 |
| Enrollment Service | enrollment-service | 8083 |
| Grade Service | grade-service | 8084 |
| PostgreSQL | postgresql | 5432 |
| React Frontend | frontend | 5173 |

Business service ports are the proposed development assignments and must be coordinated before changes are made.

## 8. API Gateway and Routing

The API Gateway is the primary entry point for external clients.

**Base URL:** http://localhost:8090

### Configured Routes

| API Path | Destination |
|---|---|
| /api/auth/** | lb://auth-service |
| /api/students/** | lb://auth-service |
| /api/courses/** | lb://course-service |
| /api/enrollments/** | lb://enrollment-service |
| /api/grades/** | lb://grade-service |

The Gateway uses Eureka service discovery and Spring Cloud LoadBalancer.

External clients should communicate with business microservices through the Gateway.

Successful request forwarding and load distribution must be verified once the destination services are available.

## 9. User Roles and Permissions

### STUDENT

- Register and log in.
- Manage permitted personal profile information.
- View available courses.
- Register for eligible courses.
- Withdraw from courses according to business rules.
- View personal enrollments and grades.

### ADMIN

- Manage course information.
- Manage course capacity.
- Manage authorized student information.
- Manage enrollments.
- Assign and update grades.

Final permissions must match the approved Software Requirements Specification.

## 10. Business Rules and OCL

### Business Rules

1. A student cannot enroll in the same course more than once.
2. A course cannot exceed its maximum enrollment capacity.
3. Enrollment requires an authenticated student.
4. Students cannot access another student's private academic information without authorization.
5. Grades must satisfy the approved grading scale.
6. Grade assignment requires a valid enrollment.
7. Enrollment and grade operations must reference valid academic records.

Additional business rules will be finalized during requirements analysis.

### Object Constraint Language

The project must include at least five OCL constraints covering invariants, preconditions, and postconditions.

Potential constraint areas include:

- Positive course capacity.
- Maximum enrollment capacity.
- Duplicate enrollment prevention.
- Valid grade ranges.
- Enrollment eligibility.
- Successful registration state.

Approved OCL constraints must be documented in the UML model and enforced in the implementation.

## 11. Security and API Standards

The proposed security architecture includes:

- Spring Security.
- JWT authentication.
- BCrypt password hashing.
- Role-based authorization.
- Protected API endpoints.
- Resource ownership validation.
- Secure environment variables.
- Request DTO validation.
- Consistent exception handling.

Passwords, JWT signing secrets, and database credentials must never be committed to GitHub.

Business microservices should follow a layered architecture:

Controller → Service → Repository → Database

Entities must not be exposed directly through public APIs.

All members should review and follow the shared API standards after team approval.

See [API and Security Standards](docs/API_STANDARDS.md).

## 12. Aspect-Oriented Programming

The project must implement at least two AOP aspects.

### Logging Aspect

Records relevant method execution and application events using an appropriate advice type, such as @Before.

### Execution Time Aspect

Measures method execution duration using @Around.

Pointcuts, advice types, and execution results must be documented.

Sensitive information must not appear in application logs.

## 13. Database Architecture

PostgreSQL is the primary relational database.

Each business microservice should own its database or schema.

Database access is implemented using Spring Data JPA and Hibernate.

### Database Guidelines

- Define primary keys and appropriate constraints.
- Use consistent naming conventions.
- Use transactions where required.
- Prevent invalid and duplicate records.
- Avoid direct access to another service's database.
- Communicate through service APIs for cross-service operations.
- Store database credentials securely.
- Maintain database migration history when migrations are adopted.

The final database design and ERD will be maintained under docs/database/.

## 14. API Documentation and Testing

### API Documentation

Each business microservice must provide Swagger/OpenAPI documentation covering:

- Endpoint paths.
- HTTP methods.
- Request and response DTOs.
- Authentication requirements.
- Request and response examples.
- HTTP status codes.
- Validation and error responses.

### Testing Strategy

**Unit Testing**

- JUnit.
- Mockito.
- Business logic tests.
- Validation tests.

**Integration Testing**

- Database integration.
- API endpoint behavior.
- Service communication.
- Authentication and authorization.
- Gateway routing.
- Service discovery.
- Load balancing.

**Manual Testing**

- Postman collections.
- Positive and negative test cases.
- Business-rule validation.
- Frontend integration.

All external API tests should use the API Gateway.

Verified test results are recorded in [PROGRESS.md](docs/PROGRESS.md).

## 15. Local Development Setup

### Prerequisites

Install and configure:

- Java JDK 25.
- Apache Maven.
- PostgreSQL.
- Node.js and npm.
- Docker Desktop.
- Git.
- VS Code with Java and Spring Boot extensions.

### Clone the Repository

    git clone -b development https://github.com/KiraIIV-max/course-registration-system.git
    cd course-registration-system

### Verify the Environment

    java -version
    javac -version
    mvn -version
    node -v
    npm -v
    git --version
    docker --version
    docker compose version
    psql --version

Maven must use JDK 25 for the current backend configuration.

### Windows Java and Maven Configuration

If the wrong Java or Maven version is selected, configure the current Windows CMD session:

    set "JAVA_HOME=C:\Program Files\Java\jdk-25"
    set "MAVEN_HOME=C:\apache-maven-3.10.1"
    set "PATH=%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%PATH%"

Verify the configuration:

    java -version
    javac -version
    mvn -version

Adjust installation paths according to your computer.

These commands affect only the current CMD session.

## 16. Running the Backend Infrastructure

### Step 1 — Start Eureka Discovery Server

Open a terminal from the repository root:

    cd backend\discovery-server
    mvn clean verify
    mvn spring-boot:run

Open the Eureka Dashboard:

http://localhost:8761

Keep the Discovery Server running.

### Step 2 — Start API Gateway

Open a second terminal from the repository root:

    cd backend\api-gateway
    mvn clean verify
    mvn spring-boot:run

Ensure the correct Java and Maven versions are configured in this terminal.

### Step 3 — Verify API Gateway

Open:

http://localhost:8090/actuator/health

Expected HTTP status: 200 OK.

Expected health status: UP.

### Step 4 — Verify Eureka Registration

Open:

http://localhost:8761

The Eureka Dashboard should display API-GATEWAY with status UP after registration completes.

### Step 5 — Test Initial Gateway Routing

Open a third terminal:

    curl -i http://localhost:8090/api/auth/test
    curl -i http://localhost:8090/api/students
    curl -i http://localhost:8090/api/courses
    curl -i http://localhost:8090/api/enrollments
    curl -i http://localhost:8090/api/grades

HTTP 503 Service Unavailable is expected when no destination business-service instances are registered.

These initial checks do not establish successful business-service routing or load balancing.

### Step 6 — Stop the Services

Press Ctrl + C in each running terminal.

### Troubleshooting

If port 8090 is already in use:

    netstat -ano | findstr :8090

Check whether an existing Gateway instance is running:

    curl -i http://localhost:8090/actuator/health

Do not start duplicate instances on the same port.

If Eureka is unavailable, verify that the Discovery Server is running on port 8761.

## 17. Docker and Deployment

Docker will be used to package backend services and required infrastructure.

The planned containerized environment includes:

- Eureka Discovery Server.
- API Gateway.
- Auth Service.
- Course Service.
- Enrollment Service.
- Grade Service.
- PostgreSQL databases.
- React frontend, if included in the final deployment configuration.

Each backend service must provide a Dockerfile.

Docker Compose will coordinate the complete application environment.

Dockerfiles and Compose startup instructions will be documented after implementation and verification.

Docker deployment is not yet verified at the infrastructure checkpoint.

## 18. Git Workflow and Team Responsibilities

### Branches

| Branch | Purpose |
|---|---|
| main | Stable and reviewed milestones |
| development | Active development and integration |

All team members should work on the shared development branch according to the agreed workflow.

### Start Working

    git switch development
    git pull --ff-only origin development
    git status

### Contribution Guidelines

- Coordinate task ownership before making changes.
- Avoid overwriting teammates' work.
- Test changes before committing.
- Use meaningful commit messages.
- Never commit credentials or generated build outputs.
- Update relevant documentation.
- Record completed work in PROGRESS.md.
- Resolve conflicts without discarding other members' contributions.
- Do not force-push the shared development branch.
- Merge stable milestones into main through reviewed Pull Requests.

### Example Commit Messages

    feat(auth): implement login endpoint
    feat(course): add course management
    fix(enrollment): prevent duplicate enrollment
    test(grade): add grade validation tests
    docs: update API documentation
    chore: configure Docker environment

### Team Responsibilities

| Member | Responsibility |
|---|---|
| Member 1 | Infrastructure and Integration |
| Member 2 | Authentication and Students |
| Member 3 | Course Management |
| Member 4 | Enrollment Management |
| Member 5 | Grades and AOP |
| Member 6 | Frontend and Documentation |

Detailed responsibilities, individual task checklists, contributions, blockers, and testing results are maintained in the Team Progress Tracker.

## 19. Project Documentation

| Document | Purpose |
|---|---|
| [README.md](README.md) | Project overview and startup instructions |
| [PROGRESS.md](docs/PROGRESS.md) | Team progress and individual contributions |
| [DEVELOPMENT_GUIDE.md](docs/DEVELOPMENT_GUIDE.md) | Development setup and team workflow |
| [API_STANDARDS.md](docs/API_STANDARDS.md) | Shared API and security conventions |
| [SRS.md](docs/requirements/SRS.md) | Software Requirements Specification |
| [ARCHITECTURE.md](docs/architecture/ARCHITECTURE.md) | System architecture |

### Required Project Deliverables

- Software Requirements Specification.
- Use Case Diagram and descriptions.
- Class Diagram.
- Sequence Diagram.
- Activity Diagram.
- Architecture Diagram.
- Entity Relationship Diagram.
- At least five OCL constraints.
- OCL enforcement documentation.
- AOP implementation and documentation.
- Swagger/OpenAPI documentation.
- Postman collection.
- Dockerfiles.
- Docker Compose configuration.
- Final project report.
- Individual member contributions.
- Project cover sheet.

All diagrams and documentation must remain consistent with the actual implementation.

## 20. Project Progress and Development Roadmap

### Development Roadmap

1. Environment and repository setup.
2. Requirements analysis and initial documentation.
3. Eureka Discovery Server.
4. API Gateway.
5. Shared API and security standards.
6. Infrastructure containerization.
7. Authentication and Students Service.
8. Course Service.
9. Enrollment Service.
10. Grade Service and AOP.
11. React Frontend.
12. Service integration and testing.
13. Complete Docker Compose environment.
14. Final documentation and project delivery.

### Infrastructure Checkpoint

The Eureka Discovery Server and API Gateway have been initialized, built, and verified locally.

Gateway registration, health checks, and initial unavailable-service routing behavior have been tested.

Successful business-service routing, load balancing, security implementation, Docker deployment, and full system integration remain pending verification.

For the latest status of every task and team member, refer to:

**[Team Progress Tracker](docs/PROGRESS.md)**

This README should be updated when project architecture, setup instructions, dependencies, or major milestones change. Daily task updates should be recorded in PROGRESS.md.

---

**Project:** University Course Registration System

**Course:** Software Engineering II

**Architecture:** Microservices

**Repository:** https://github.com/KiraIIV-max/course-registration-system

**Documentation Status:** Active Development
