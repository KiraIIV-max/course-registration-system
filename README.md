# University Course Registration System

A microservices-based university course registration platform built using Spring Boot, Spring Cloud, PostgreSQL, and React.

## 1. Project Overview

The University Course Registration System is a web-based application designed to manage university courses, student enrollment, authentication, and academic grades.

The project follows a **Microservices Architecture**, where independent services handle different business responsibilities and communicate through REST APIs.

The system is being developed as part of the Software Engineering II course.

**Project Status:** Active Development
**Current Milestone:** Backend Infrastructure
**Completed Service:** Eureka Discovery Server
**Next Service:** API Gateway

> **Implementation note:** The features described below are the planned scope unless explicitly marked as completed. At this checkpoint, only the Eureka Discovery Server has been implemented and verified.

## 2. Project Objectives

- Build a modular and maintainable university registration platform.
- Apply Microservices Architecture and separation of concerns.
- Implement secure authentication and role-based authorization.
- Allow administrators to manage courses and academic information.
- Allow students to browse available courses and register for them.
- Enforce registration rules and data validation.
- Implement service discovery and API routing.
- Apply Aspect-Oriented Programming (AOP).
- Document and test REST APIs.
- Containerize the application using Docker.

## 3. Main Features

### Authentication and Authorization (Planned)

- User authentication using JWT.
- Password hashing using BCrypt.
- Role-based access control.
- Student and Administrator roles.
- Protected API endpoints.

### Course Management (Planned)

- Create courses.
- Update course information.
- Delete courses, subject to business constraints.
- View available courses.
- Search and filter courses.
- Manage course capacity.

### Student Enrollment (Planned)

- Register students in available courses.
- Prevent duplicate enrollment.
- Validate available course capacity.
- View student enrollments.
- Drop courses according to enrollment rules.

### Grade Management (Planned)

- Assign and update student grades.
- Retrieve grades for enrolled students.
- Restrict grade management operations to authorized users.
- Validate grade values.

These features describe the planned scope and may be refined during requirements analysis.

## 4. System Architecture

The system uses a microservices-based architecture. The intended request flow is:

```text
                    React + Vite
                         |
                         v
                    API Gateway
                         |
           +-------------+-------------+
           |             |             |
           v             v             v
      Auth Service  Course Service  Enrollment Service
                                       |
                                       v
                                  Grade Service

                Eureka Discovery Server
                  (Service Registry)

             PostgreSQL Data Storage
```

This diagram is conceptual, not a finalized UML or deployment diagram. Service-to-service dependencies and data ownership will be defined during implementation.

### Architecture Components

| Component | Responsibility | Status |
|---|---|---|
| React Frontend | User interface | Planned |
| API Gateway | Routing client requests to backend services | Next |
| Eureka Server | Service registration and discovery | Completed locally |
| Auth Service | Authentication and user authorization | Planned |
| Course Service | Course information and management | Planned |
| Enrollment Service | Student course registration | Planned |
| Grade Service | Academic grade management | Planned |
| PostgreSQL | Persistent relational data storage | Planned integration |

### Communication

- Frontend-to-backend communication through REST APIs.
- API Gateway as the primary client entry point.
- Eureka for service discovery.
- OpenFeign for selected synchronous inter-service requests.
- Spring Cloud LoadBalancer for service instance selection.

### Database Strategy

Each business microservice should own its data and database schema.

Services must not directly modify another service's database.

Cross-service operations should use explicitly defined APIs.

## 5. Technology Stack

### Backend

| Technology | Purpose |
|---|---|
| Java 25 LTS | Backend programming language |
| Spring Boot | Backend application framework |
| Spring Cloud Gateway | API Gateway |
| Netflix Eureka | Service discovery |
| Spring Cloud OpenFeign | Inter-service HTTP communication |
| Spring Cloud LoadBalancer | Client-side load balancing |
| Spring Security | Authentication and authorization |
| JWT | Token-based authentication |
| Spring Data JPA | Database access |
| Hibernate | ORM implementation |
| PostgreSQL | Relational database |
| Flyway | Database schema migrations |
| Jakarta Bean Validation | Input validation |
| Spring AOP | Cross-cutting concerns |
| Maven | Build and dependency management |
| JUnit and Mockito | Unit testing |
| springdoc OpenAPI | API documentation and Swagger UI |

Not every listed dependency is installed yet. Each service should add only the dependencies it needs.

### Frontend

| Technology | Purpose |
|---|---|
| React | UI development |
| Vite | Frontend tooling |
| JavaScript | Frontend programming |
| HTML and CSS | UI structure and styling |
| Axios or Fetch API | HTTP requests |

The HTTP client will be selected during frontend setup.

### Development Tools

| Tool | Purpose |
|---|---|
| VS Code | Development environment |
| Git | Version control |
| GitHub | Collaboration and code hosting |
| Postman | API testing |
| DBeaver | Database management |
| Docker | Containerization |
| Docker Compose | Multi-container orchestration |
| draw.io | Architecture and UML diagrams |

### Version Management

The team should use the agreed versions consistently to avoid environment-specific build failures.

| Tool | Agreed / Recorded Version |
|---|---|
| Java JDK | 25.0.1 |
| Maven | 3.10.1 |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| PostgreSQL | 18.7 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Git | 2.49.0 |

The Spring Boot and Spring Cloud versions above are configured in the completed Discovery Server. Future services should use the team's agreed compatible versions and verify their builds.

Dependency versions should be managed through Spring Boot dependency management and the Spring Cloud BOM, rather than manually assigning versions to every dependency.

**Windows Java note:** A machine may have multiple JDKs installed. Maven must run with JDK 25 when the project targets Java 25. Check with `java -version`, `javac -version`, and `mvn -version`.

## 6. Repository Structure

```text
course-registration-system/
|-- backend/
|   |-- discovery-server/       # Implemented
|   |-- api-gateway/            # Next to initialize
|   |-- auth-service/           # Planned
|   |-- course-service/         # Planned
|   |-- enrollment-service/     # Planned
|   `-- grade-service/          # Planned
|-- frontend/                   # Planned React application
|-- docker/                     # Planned container configuration
|-- docs/
|   |-- requirements/
|   |-- architecture/
|   |-- uml/
|   |-- database/
|   `-- api/
|-- .github/
|   `-- workflows/
|-- .gitignore
`-- README.md
```

Some directories are placeholders and do not yet contain working applications. See the project progress documentation as it is added.

## 7. User Roles and Permissions

### Student

- Log in to the system.
- View available courses.
- Search courses.
- Enroll in eligible courses.
- Drop courses according to defined rules.
- View personal enrollments and grades.

### Administrator

- Manage course information.
- Manage course capacity.
- View enrollment information.
- Perform authorized academic administration operations.

Grade assignment permissions and any additional roles will be finalized in the requirements specification.

## 8. Business Rules

The following are initial business rules to be validated during requirements analysis.

1. A student cannot enroll in the same course more than once.
2. A course cannot exceed its maximum enrollment capacity.
3. Enrollment requires an authenticated student.
4. A student can view only their own private academic information unless explicitly authorized otherwise.
5. Grades must satisfy the configured grading scale.
6. Enrollment and grade operations must reference valid academic records.

Additional rules, including enrollment deadlines, prerequisites, course deletion restrictions, and maximum course loads, will be specified before implementation.

## 9. OCL Constraints

The project will document at least five Object Constraint Language (OCL) constraints.

The constraints will cover relevant invariants, preconditions, and postconditions.

Potential areas include:

- Course capacity.
- Duplicate enrollment prevention.
- Valid grade ranges.
- Enrollment eligibility.
- Successful registration state.

Formal OCL expressions will be written and checked against the final UML class model.

## 10. Aspect-Oriented Programming

The backend will include at least two AOP aspects.

### Logging Aspect

Responsible for recording relevant application events and method execution information.

### Execution Time Aspect

Responsible for measuring execution duration for selected service operations.

Logging must avoid exposing passwords, JWT tokens, or other sensitive information.

## 11. API Documentation

Each applicable business microservice will expose API documentation using OpenAPI and Swagger UI.

Postman collections will be maintained for integration and API testing.

API documentation should include:

- Endpoint paths.
- HTTP methods.
- Request and response formats.
- Authentication requirements.
- Expected status codes.
- Error responses.

Actual endpoint definitions will be added as APIs are implemented.

## 12. Testing Strategy

### Unit Testing

- JUnit for test execution.
- Mockito for mocking dependencies.
- Business logic and validation tests.

### Integration Testing

- Database integration.
- API endpoint behavior.
- Service communication.
- Authentication and authorization flows.

### Manual API Testing

- Postman collections.
- Positive and negative test cases.
- Validation and error handling.

### Current Verified Test Status

The Discovery Server completed `mvn clean verify` successfully on JDK 25, and its Eureka Dashboard was opened successfully in a browser. This does **not** mean the other planned services or end-to-end flows have been tested.

## 13. Docker and Deployment

Docker will be used to package backend services and required infrastructure.

Docker Compose will coordinate the local development environment.

The planned containerized environment includes:

- Eureka Server.
- API Gateway.
- Business microservices.
- PostgreSQL database instances or isolated databases.
- Frontend, if included in the deployment configuration.

Actual Dockerfiles, Compose configuration, ports, and environment variables will be added during implementation. Docker deployment is not yet verified.

## 14. Local Development Setup

### Prerequisites

Install and configure:

- Java JDK 25.
- Apache Maven.
- PostgreSQL (for future database-backed services).
- Node.js and npm (for the future frontend).
- Docker Desktop (for future containerized development).
- Git.
- VS Code with Java and Spring Boot extensions (recommended).

### Clone the Repository

```bash
git clone https://github.com/KiraIIV-max/course-registration-system.git
cd course-registration-system
git switch development
git pull --ff-only origin development
```

### Check Java and Maven

On Windows CMD:

```cmd
java -version
javac -version
mvn -version
```

All Java-related commands should report JDK 25 for the current backend configuration. If Maven uses an older JDK, set `JAVA_HOME` and prepend its `bin` directory to `PATH` in the current CMD session:

```cmd
set "JAVA_HOME=C:\Program Files\Java\jdk-25"
set "PATH=%JAVA_HOME%\bin;%PATH%"
mvn -version
```

The JDK installation path is an example from the initial developer machine; adjust it to the path on your computer. These `set` commands affect only the current CMD window.

### Discovery Server — Build and Run

From the repository root on Windows CMD:

```cmd
cd backend\discovery-server
mvn clean verify
mvn spring-boot:run
```

When the server starts, open:

**http://localhost:8761**

The Eureka Dashboard should load. `No instances available` is expected until other microservices register with Eureka.

To stop the server, press `Ctrl + C` in the terminal running it.

The Discovery Server is configured in `backend/discovery-server/src/main/resources/application.yaml` to use port `8761` and not register itself as a Eureka client.

### Frontend Setup

Frontend setup instructions will be added after the React application is initialized.

### Docker Setup

Docker Compose startup instructions will be documented after the Compose configuration is created.

## 15. Git Workflow

The team has agreed to use **two shared branches only**:

| Branch | Purpose |
|---|---|
| `main` | Stable, reviewed milestones |
| `development` | Active team development and integration |

Do not create `feature/*` or `fix/*` branches under the current team workflow unless the team explicitly changes this agreement.

### Contribution Workflow

1. Coordinate task ownership and affected files with teammates.
2. Switch to `development` and pull the latest remote changes.
3. Implement the assigned task without overwriting teammates' work.
4. Run relevant builds and tests locally.
5. Review changed files and confirm no secrets or generated build outputs are staged.
6. Commit with a meaningful message.
7. Pull/reconcile remote changes if necessary, then push to `development`.
8. Update documentation when behavior, setup, or progress changes.
9. Open a reviewed Pull Request from `development` to `main` when a milestone is stable.

### Start Working

```cmd
git switch development
git pull --ff-only origin development
git status --short
```

### Example Commit Messages

```text
feat(auth): implement login endpoint
feat(course): add course management
fix(enrollment): prevent duplicate enrollment
docs: update API documentation
test(course): add service unit tests
```

Avoid direct commits to `main`. Because the team shares `development`, coordinate concurrent edits carefully and do not force-push or overwrite another member's changes.

## 16. Team Collaboration

All team members should:

- Use the agreed technology versions.
- Follow the repository structure.
- Work on assigned tasks on the shared `development` branch.
- Coordinate before editing shared files or pushing concurrent changes.
- Avoid committing credentials or environment secrets.
- Keep documentation updated.
- Test their work before committing and pushing.
- Review changes before integrating milestones into `main`.
- Communicate changes that affect shared APIs or database contracts.

Task assignments and ownership will be documented after the team finalizes responsibilities.

## 17. Project Documentation

The `docs/` directory is organized as follows:

| Directory | Intended Content |
|---|---|
| `docs/requirements/` | Software Requirements Specification (SRS), functional and non-functional requirements |
| `docs/architecture/` | Architecture decisions, service responsibilities, communication |
| `docs/uml/` | Use Case, Class, Sequence diagrams and OCL constraints |
| `docs/database/` | Database design and ERD |
| `docs/api/` | API specifications and testing references |

Additional team handoff documents may be added under `docs/`, including a development guide and progress tracker. These are not yet claimed as completed deliverables.

## 18. Development Roadmap

| Phase | Planned Work | Status |
|---|---|---|
| 1 | Environment setup | Completed locally |
| 2 | Repository and team workflow | Established; documentation being updated |
| 3 | Requirements and UML design | Planned / to be finalized |
| 4A | Eureka Discovery Server | Completed and verified |
| 4B | API Gateway | Next |
| 5 | Authentication Service | Planned |
| 6 | Course Service | Planned |
| 7 | Enrollment Service | Planned |
| 8 | Grade Service | Planned |
| 9 | React Frontend | Planned |
| 10 | Integration, testing, and Docker | Planned |
| 11 | Final documentation and delivery | Planned |

## 19. Security Considerations

- Store passwords using a secure password-hashing mechanism.
- Validate JWT tokens.
- Enforce role-based access control.
- Validate all user inputs.
- Protect sensitive student information.
- Do not commit passwords, database credentials, or JWT signing secrets.
- Configure secrets through environment variables or other secure mechanisms.
- Apply authorization checks at the appropriate service boundaries.

## 20. Project Status and Team Handoff

**Last updated:** November 21, 2026
**Active branch:** `development`
**Last completed implementation commit:** `4b8cd1f` — `feat: initialize Eureka discovery server`

### Completed

- Initial repository and folder structure created.
- Development environment prepared.
- Shared `development` branch created and pushed.
- Eureka Discovery Server initialized with Spring Boot `4.1.3`, Spring Cloud `2025.1.5`, and Java `25`.
- Eureka Server enabled and configured on port `8761`.
- Maven build and tests passed using JDK 25.
- Eureka Dashboard opened successfully at `http://localhost:8761`.
- Discovery Server implementation committed and pushed to `development`.

### Next Steps

1. Initialize `backend/api-gateway` using Spring Initializr.
2. Use Java `25`, Spring Boot `4.1.3`, and the team's Spring Cloud `2025.1.5` baseline.
3. Add Spring Cloud Gateway Server WebFlux and Eureka Discovery Client dependencies.
4. Configure Gateway service discovery and routing.
5. Verify the Gateway build and its integration with Eureka before committing.
6. Continue with Auth, Course, Enrollment, and Grade services.
7. Complete requirements, UML, database, API, testing, and deployment documentation alongside implementation.

**Handoff note:** The API Gateway has **not** been generated, configured, or tested at this checkpoint. Do not mark it complete until the team verifies it.

---

**Project:** University Course Registration System
**Course:** Software Engineering II
**Architecture:** Microservices
**Repository:** https://github.com/KiraIIV-max/course-registration-system
**Documentation Status:** Working documentation; update it as the project progresses.
