# University Course Registration System

A microservices-based university course registration platform built using Spring Boot, Spring Cloud, PostgreSQL, and React.

## 1. Project Overview

The University Course Registration System is a web-based application designed to manage university courses, student enrollment, authentication, and academic grades.

The project follows a **Microservices Architecture**, where independent services handle different business responsibilities and communicate through REST APIs.

The system is being developed as part of the Software Engineering II course.

**Project Status:** Planning and Environment Setup

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

### Authentication and Authorization

- User authentication using JWT.
- Password hashing using BCrypt.
- Role-based access control.
- Student and Administrator roles.
- Protected API endpoints.

### Course Management

- Create courses.
- Update course information.
- Delete courses, subject to business constraints.
- View available courses.
- Search and filter courses.
- Manage course capacity.

### Student Enrollment

- Register students in available courses.
- Prevent duplicate enrollment.
- Validate available course capacity.
- View student enrollments.
- Drop courses according to enrollment rules.

### Grade Management

- Assign and update student grades.
- Retrieve grades for enrolled students.
- Restrict grade management operations to authorized users.
- Validate grade values.

These features describe the planned scope and may be refined during requirements analysis.

## 4. System Architecture

The system uses a microservices-based architecture.

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
        |             |             |
        +-------------+-------------+
                      |
                Grade Service

             Eureka Discovery Server

          PostgreSQL Databases
```

The diagram is conceptual. Actual service-to-service dependencies will be defined during implementation.

### Architecture Components

| Component | Responsibility |
|---|---|
| React Frontend | User interface |
| API Gateway | Routing client requests to backend services |
| Eureka Server | Service registration and discovery |
| Auth Service | Authentication and user authorization |
| Course Service | Course information and management |
| Enrollment Service | Student course registration |
| Grade Service | Academic grade management |
| PostgreSQL | Persistent relational data storage |

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

The team must use compatible and agreed-upon development versions.

Current local environment baseline:

| Tool | Version |
|---|---|
| Java | 25.0.1 |
| Maven | 3.10.1 |
| PostgreSQL | 18.7 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Git | 2.49.0 |

Spring Boot and Spring Cloud versions will be locked and documented when the backend projects are initialized.

Dependency versions should be managed centrally through Spring Boot dependency management and the Spring Cloud BOM.

## 6. Planned Repository Structure

```text
course-registration-system/
|
|-- backend/
|   |-- discovery-server/
|   |-- api-gateway/
|   |-- auth-service/
|   |-- course-service/
|   |-- enrollment-service/
|   |-- grade-service/
|
|-- frontend/
|
|-- docker/
|
|-- docs/
|   |-- requirements/
|   |-- architecture/
|   |-- uml/
|   |-- database/
|   |-- api/
|
|-- .gitignore
|-- README.md
```

This is the planned structure. Directories will be created as the corresponding project components are initialized.

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

## 13. Docker and Deployment

Docker will be used to package backend services and required infrastructure.

Docker Compose will coordinate the local development environment.

The planned containerized environment includes:

- Eureka Server.
- API Gateway.
- Business microservices.
- PostgreSQL database instances or isolated databases.
- Frontend, if included in the deployment configuration.

Actual Dockerfiles, Compose configuration, ports, and environment variables will be added during implementation.

## 14. Local Development Setup

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

```bash
git clone https://github.com/KiraIIV-max/course-registration-system.git
cd course-registration-system
```

### Backend Setup

Backend setup instructions will be added after the individual Spring Boot projects are initialized.

### Frontend Setup

Frontend setup instructions will be added after the React application is initialized.

### Docker Setup

Docker Compose startup instructions will be documented after the Compose configuration is created.

## 15. Git Workflow

The team will use a branch-based development workflow.

### Main Branches

| Branch | Purpose |
|---|---|
| main | Stable and reviewed project code |
| development | Integration branch for ongoing work |
| feature/* | Individual features and tasks |
| fix/* | Bug fixes |

### Contribution Workflow

1. Pull the latest `development` branch.
2. Create a feature branch.
3. Implement the assigned task.
4. Test changes locally.
5. Commit changes with meaningful messages.
6. Push the feature branch.
7. Open a Pull Request targeting `development`.
8. Request review before merging.

### Example Branch Names

```text
feature/authentication
feature/course-management
feature/enrollment
feature/grade-management
feature/frontend
fix/enrollment-validation
```

### Example Commit Messages

```text
feat(auth): implement login endpoint
feat(course): add course management
fix(enrollment): prevent duplicate enrollment
docs: update API documentation
test(course): add service unit tests
```

Direct commits to `main` should be avoided after initial repository setup.

## 16. Team Collaboration

All team members should:

- Use the agreed technology versions.
- Follow the repository structure.
- Work on assigned feature branches.
- Avoid committing credentials or environment secrets.
- Keep documentation updated.
- Test their work before requesting a merge.
- Review changes before integration.
- Communicate changes that affect shared APIs or database contracts.

Task assignments and ownership will be documented after the team finalizes responsibilities.

## 17. Project Documentation

The `docs/` directory will contain:

- Software Requirements Specification (SRS).
- UML Use Case Diagram.
- UML Class Diagram.
- UML Sequence Diagrams.
- Architecture documentation.
- Database ERD.
- OCL constraints.
- API specifications.
- Testing documentation.
- Deployment instructions.

## 18. Development Roadmap

| Phase | Planned Work | Status |
|---|---|---|
| 1 | Environment setup | Completed locally |
| 2 | Repository and team workflow | In progress |
| 3 | Requirements and UML design | Planned |
| 4 | Eureka Server and API Gateway | Planned |
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

## 20. Project Status

The project is currently in the initial setup and planning phase.

The development environment has been prepared, and the next steps are to finalize the team workflow, requirements, architecture, and service initialization.

---

**Project:** University Course Registration System

**Course:** Software Engineering II

**Architecture:** Microservices

**Repository:** https://github.com/KiraIIV-max/course-registration-system

**Documentation Status:** Initial draft — subject to updates during development.