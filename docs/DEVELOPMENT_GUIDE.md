# Development Guide — Course Registration System

**Project:** University Course Registration System

**Course:** Software Engineering II

**Active Branch:** development

**Last Updated:** November 26, 2026

## 1. Purpose

This guide defines the shared development environment, project structure, service configuration, Git workflow, testing procedures, and team collaboration rules.

All contributors should follow these conventions to maintain consistency and avoid integration problems.

## 2. Technology Versions

| Technology | Recorded Team Version |
|---|---|
| Java JDK | 25.0.1 |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| Apache Maven | 3.10.1 |
| PostgreSQL | 18.7 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Git | 2.49.0 |

### Version Management Rules

- Use the agreed technology versions.
- Do not independently upgrade dependencies.
- Use Spring Boot dependency management.
- Use the Spring Cloud BOM.
- Verify Maven Wrapper versions before using them.
- Verify Java and Maven configuration before building.
- Coordinate dependency changes with the team.

## 3. Initial Setup

### Clone the Repository

    git clone -b development https://github.com/KiraIIV-max/course-registration-system.git
    cd course-registration-system

### Verify Installed Tools

    java -version
    javac -version
    mvn -version
    node -v
    npm -v
    git --version
    docker --version
    docker compose version
    psql --version

Confirm that Maven uses Java 25 and the agreed Maven version.

### Windows Java and Maven Configuration

If Windows selects an incorrect Java or Maven version, configure the current CMD session:

    set "JAVA_HOME=C:\Program Files\Java\jdk-25"
    set "MAVEN_HOME=C:\apache-maven-3.10.1"
    set "PATH=%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%PATH%"

Verify:

    java -version
    javac -version
    mvn -version
    where java
    where mvn

Adjust the installation paths according to your computer.

These commands affect only the current CMD session.

## 4. Repository Structure

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
    |   |-- api/
    |   |-- architecture/
    |   |-- database/
    |   |-- requirements/
    |   |-- uml/
    |   |-- API_STANDARDS.md
    |   |-- DEVELOPMENT_GUIDE.md
    |   `-- PROGRESS.md
    |-- .github/
    |   `-- workflows/
    |-- .editorconfig
    |-- .gitignore
    `-- README.md

Each member must work within their assigned directories and coordinate changes to shared files.

## 5. Service Configuration

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

The Discovery Server and API Gateway ports are configured and verified locally.

The remaining application ports are proposed assignments for team integration.

### Important URLs

| Resource | URL |
|---|---|
| Eureka Dashboard | http://localhost:8761 |
| Eureka Registry | http://localhost:8761/eureka/ |
| API Gateway | http://localhost:8090 |
| Gateway Health | http://localhost:8090/actuator/health |
| React Frontend | http://localhost:5173 |

The frontend URL becomes available after the frontend is initialized and started.

## 6. Running Backend Infrastructure

### Terminal 1 — Discovery Server

From the repository root:

    cd backend\discovery-server
    mvn clean verify
    mvn spring-boot:run

Open:

http://localhost:8761

Keep the Discovery Server running.

### Terminal 2 — API Gateway

Open another CMD window from the repository root:

    cd backend\api-gateway
    mvn clean verify
    mvn spring-boot:run

Ensure that Java and Maven are configured correctly in both terminals.

### Verify Gateway Health

    curl -i http://localhost:8090/actuator/health

Expected result:

- HTTP 200 OK.
- Health status: UP.

### Verify Eureka Registration

Open:

http://localhost:8761

Confirm that API-GATEWAY appears with status UP after registration completes.

### Initial Routing Tests

    curl -i http://localhost:8090/api/auth/test
    curl -i http://localhost:8090/api/students
    curl -i http://localhost:8090/api/courses
    curl -i http://localhost:8090/api/enrollments
    curl -i http://localhost:8090/api/grades

HTTP 503 is expected when the destination business microservices are unavailable.

Successful request forwarding and load balancing require additional testing with running business microservices.

### Stop the Applications

Press Ctrl + C in each running terminal.

## 7. Business Microservice Initialization

Each backend member must initialize their assigned Spring Boot application using the agreed project baseline.

### Common Spring Initializr Settings

| Setting | Value |
|---|---|
| Project | Maven |
| Language | Java |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| Java | 25 |
| Group | com.courseregistration |
| Packaging | Jar |
| Configuration | YAML |

### Service Naming

| Service | Artifact | Base Package |
|---|---|---|
| Auth | auth-service | com.courseregistration.auth |
| Course | course-service | com.courseregistration.course |
| Enrollment | enrollment-service | com.courseregistration.enrollment |
| Grade | grade-service | com.courseregistration.grade |

### Recommended Business Service Dependencies

- Spring Web MVC.
- Spring Data JPA.
- PostgreSQL Driver.
- Validation.
- Eureka Discovery Client.
- Spring Security.
- Spring Boot Actuator.
- Spring Boot Test.

Additional dependencies should be selected according to service requirements:

- JWT libraries for authentication and token validation.
- OpenFeign for inter-service communication.
- Spring Cloud LoadBalancer where required.
- Spring AOP for cross-cutting concerns.
- springdoc OpenAPI for API documentation.
- Flyway for database migrations, subject to team approval.

Verify dependency compatibility before committing.

The API Gateway uses WebFlux rather than Spring Web MVC.

## 8. Backend Development Conventions

### Layered Architecture

Controller → Service → Repository → Database

### Recommended Package Structure

    com.courseregistration.servicename/
    |-- controller/
    |-- service/
    |-- repository/
    |-- entity/
    |-- dto/
    |-- exception/
    |-- config/
    |-- security/
    |-- client/
    `-- aspect/

Create packages according to the responsibilities of each service.

### Development Rules

- Keep business logic in the service layer.
- Use DTOs for API requests and responses.
- Do not expose entities directly through public APIs.
- Use Jakarta Bean Validation.
- Implement consistent exception handling.
- Enforce approved business rules and OCL constraints.
- Use meaningful class, method, and variable names.
- Avoid unnecessary dependencies.
- Do not directly access another service's database.
- Use environment variables for credentials and secrets.
- Add tests for implemented behavior.

For detailed conventions, see [API_STANDARDS.md](API_STANDARDS.md).

## 9. API Gateway Integration

All external business API requests must use the Gateway.

Base URL:

http://localhost:8090

| Route | Destination |
|---|---|
| /api/auth/** | lb://auth-service |
| /api/students/** | lb://auth-service |
| /api/courses/** | lb://course-service |
| /api/enrollments/** | lb://enrollment-service |
| /api/grades/** | lb://grade-service |

### Integration Requirements

- Use the assigned Spring application name.
- Register each business microservice with Eureka.
- Use the assigned development port.
- Match the configured Gateway route prefixes.
- Preserve the request path unless a coordinated routing change is introduced.
- Verify service registration before Gateway testing.
- Coordinate API contract changes with Member 1.

## 10. Database Development

Each business microservice should own its database or schema.

### Database Rules

- Use PostgreSQL.
- Use Spring Data JPA and Hibernate.
- Define appropriate primary keys and constraints.
- Maintain consistent naming conventions.
- Use transactions where required.
- Do not share entity classes between independent microservices.
- Do not directly modify another service's database.
- Use service APIs for cross-service operations.
- Never commit database credentials.
- Document database configuration and schema changes.

Database names and migration strategies must be coordinated before implementation.

## 11. Security Conventions

The proposed shared security architecture includes:

- Spring Security.
- JWT authentication.
- BCrypt password hashing.
- STUDENT and ADMIN roles.
- Endpoint-level authorization.
- Resource ownership checks.
- Secure environment variables.
- Consistent authentication and authorization errors.

Member 2 must coordinate JWT configuration and authentication contracts with all backend members.

The final token lifetime, signing configuration, claims, internal authentication strategy, and CORS policy must be agreed before integration.

Security implementation must be tested before protected endpoints are considered complete.

See [API_STANDARDS.md](API_STANDARDS.md).

## 12. Git Branching Strategy

| Branch | Purpose |
|---|---|
| main | Stable and reviewed milestones |
| development | Active development and integration |

The team uses two shared branches.

Do not create additional branches without team agreement.

Do not commit directly to main.

### Before Starting Work

    git switch development
    git pull --ff-only origin development
    git status

Ensure that the working tree is clean before starting a new task.

### Review Changes

    git status --short
    git --no-pager diff

### Stage Intended Files

    git add path/to/changed/files

Review staged changes:

    git --no-pager diff --cached --stat
    git --no-pager diff --cached
    git --no-pager diff --cached --check

Replace the example path with the actual files related to your task.

### Commit and Push

    git commit -m "feat: describe completed task"
    git pull --rebase origin development
    git push origin development

If conflicts occur, coordinate with the affected team members before resolving them.

Never discard another member's work or force-push the shared development branch.

### Commit Message Conventions

| Prefix | Purpose |
|---|---|
| feat | New functionality |
| fix | Bug fix |
| docs | Documentation |
| test | Tests |
| refactor | Code restructuring |
| chore | Maintenance and configuration |
| build | Build system changes |

Examples:

    feat(auth): implement student registration
    feat(course): implement course management
    fix(enrollment): prevent duplicate registration
    test(grade): add grade validation tests
    docs: update team progress
    chore: configure Docker environment

### Merge to Main

Merge development into main through a reviewed GitHub Pull Request when a milestone is stable and tested.

Keep the development branch available for continued development.

## 13. Team Responsibilities

| Member | Responsibility |
|---|---|
| Member 1 | Infrastructure and Integration |
| Member 2 | Authentication and Students |
| Member 3 | Course Management |
| Member 4 | Enrollment Management |
| Member 5 | Grades and AOP |
| Member 6 | Frontend and Documentation |

Detailed task assignments are maintained in [PROGRESS.md](PROGRESS.md).

### Collaboration Rules

- Coordinate shared configuration changes.
- Pull the latest changes before starting work.
- Work on assigned files and services.
- Keep commits focused on specific tasks.
- Test changes before pushing.
- Notify teammates about API contract changes.
- Avoid committing credentials or generated build artifacts.
- Maintain accurate documentation.
- Commit individual contributions using individual GitHub accounts.

## 14. Testing Requirements

### Backend Testing Checklist

- [ ] Project compiles successfully.
- [ ] Maven clean verify succeeds.
- [ ] Relevant unit tests pass.
- [ ] Relevant integration tests pass.
- [ ] Application starts successfully.
- [ ] Database connectivity is verified, where applicable.
- [ ] Eureka registration is verified.
- [ ] API endpoints are verified.
- [ ] Request validation is verified.
- [ ] Business rules are verified.
- [ ] Authentication and authorization are verified.
- [ ] Error responses follow the agreed contract.
- [ ] Swagger documentation is available.
- [ ] Postman requests are prepared.
- [ ] Gateway integration is verified.
- [ ] Documentation is updated.

Mark only applicable and verified items as completed.

### Integration Testing

Integration testing should cover:

- Service discovery.
- Gateway routing.
- Inter-service communication.
- Load balancing.
- Authentication and authorization.
- Database operations.
- Business-rule enforcement.
- Frontend integration.
- Docker deployment.

### Load Balancing Testing

Load balancing must be tested using multiple running instances of the same business microservice.

Both instances must register with Eureka using the same application name and different ports.

Repeated Gateway requests must demonstrate that requests reach multiple instances.

This test remains pending until the required business-service instances are available.

## 15. Docker Development

Each backend service must provide a Dockerfile.

Member 1 coordinates the Docker Compose configuration for the complete application.

### Docker Guidelines

- Use compatible Java runtime images.
- Use reproducible image versions.
- Do not include credentials in images.
- Configure environment-specific settings through environment variables.
- Use container service names for container-to-container communication.
- Do not assume localhost refers to another container.
- Verify container startup and health.
- Verify Eureka registration inside the containerized environment.
- Verify Gateway routing inside the containerized environment.

Docker build and startup commands will be documented after the configuration is implemented and tested.

## 16. Documentation Responsibilities

| Document | Responsibility |
|---|---|
| README.md | Project overview and startup instructions |
| docs/PROGRESS.md | Individual contributions and task tracking |
| docs/DEVELOPMENT_GUIDE.md | Development workflow and tooling |
| docs/API_STANDARDS.md | Shared API and security conventions |
| docs/requirements/ | Requirements and business rules |
| docs/architecture/ | Architecture and service boundaries |
| docs/database/ | Database schemas and ERD |
| docs/api/ | API contracts and Postman documentation |
| docs/uml/ | UML diagrams and OCL constraints |

Update only the documentation affected by your task.

Do not duplicate daily progress information across multiple documents.

## 17. Progress Tracking

Each member must maintain their own section in docs/PROGRESS.md.

After completing a task, record:

- Member name.
- Task description.
- Completion date.
- Task status.
- Git commit ID.
- Testing results.
- Current blockers.
- Next steps.

A task should be marked Done only after its implementation has been verified.

Coordinate edits to the shared progress file to avoid merge conflicts.

## 18. Troubleshooting

### Incorrect Java or Maven Version

    java -version
    javac -version
    mvn -version
    where java
    where mvn

Correct JAVA_HOME and PATH if necessary.

### Spring Boot Maven Plugin Not Found

Ensure that Maven commands are executed inside the appropriate service directory containing pom.xml.

### Port Already in Use

Example for the API Gateway:

    netstat -ano | findstr :8090

Check whether an existing application instance is running before attempting to stop any process.

### Eureka Connection Refused

- Verify that Discovery Server is running.
- Verify port 8761.
- Verify the configured Eureka service URL.
- Allow time for service registration.

### Gateway HTTP 503

- Verify that the destination service is running.
- Verify its Eureka registration.
- Verify the Spring application name.
- Verify the Gateway route configuration.

HTTP 503 is expected when the required destination service is unavailable.

### Git Opens a Full-Screen Pager

Press q to exit the pager.

Use the following command to avoid opening the pager:

    git --no-pager diff

### Git Line Ending Warnings

LF and CRLF warnings may occur on Windows.

Review the repository configuration and avoid unnecessary line-ending changes across unrelated files.

## 19. Work Session Handoff Checklist

Before ending a development session:

- [ ] Save completed work.
- [ ] Run relevant tests.
- [ ] Update the assigned section in PROGRESS.md.
- [ ] Record blockers and next steps.
- [ ] Review Git status.
- [ ] Stage only intended files.
- [ ] Review staged changes.
- [ ] Commit completed work.
- [ ] Reconcile remote changes.
- [ ] Push verified changes to development.
- [ ] Share relevant updates with the team.

## 20. Current Infrastructure Checkpoint

The following infrastructure components have been verified locally:

- Eureka Discovery Server.
- API Gateway.
- Gateway registration with Eureka.
- Gateway health endpoint.
- Initial unavailable-service routing behavior.

Shared API and security standards have been drafted and require team approval.

Successful business-service routing, load balancing, Docker deployment, security implementation, and complete system integration remain pending verification.

For the latest development status, refer to [PROGRESS.md](PROGRESS.md).

---

**Repository:** https://github.com/KiraIIV-max/course-registration-system

**Primary Integration Branch:** development

**Document Status:** Active Development
