# Development Progress — Course Registration System

**Last Updated:** November 21, 2026  
**Current Branch:** `development`  
**Current Phase:** Backend Infrastructure  
**Overall Status:** In Progress

## 1. Completed Work

### Repository Setup
- Initialized the GitHub repository.
- Created the project folder structure.
- Established `main` and `development` branches.
- Configured the initial `.gitignore` and `.editorconfig`.
- Prepared the development environment.

### Eureka Discovery Server
**Status: Completed and verified locally**

- Generated the Spring Boot project using Spring Initializr.
- Added the Eureka Server dependency.
- Enabled Eureka using `@EnableEurekaServer`.
- Configured the application name as `discovery-server`.
- Configured port `8761`.
- Disabled self-registration and registry fetching.
- Successfully compiled and packaged the application.
- Passed the Maven build and tests.
- Started the application and verified the Eureka Dashboard.

**Latest completed implementation commit:** `4b8cd1f`

**GitHub branch:** `development`

## 2. Verified Technology Versions

| Technology | Version |
|---|---|
| Java JDK | 25 |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| Maven | 3.10.1 |
| PostgreSQL | 18.7 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Git | 2.49.0 |

The versions above are the agreed development baseline. Only the Discovery Server has been built and tested so far.

## 3. Current Microservices Status

| Component | Status |
|---|---|
| Eureka Discovery Server | Completed |
| API Gateway | Not started |
| Auth Service | Not started |
| Course Service | Not started |
| Enrollment Service | Not started |
| Grade Service | Not started |
| React Frontend | Not started |
| Docker Configuration | Not started |

## 4. How to Run the Completed Service

Open Windows CMD and run:

```cmd
git switch development
git pull --ff-only origin development
cd backend\discovery-server
mvn clean verify
mvn spring-boot:run
```

Open http://localhost:8761 to access the Eureka Dashboard.

The message `No instances available` is expected until other microservices are registered.

**Java configuration:** The project requires JDK 25. If Maven uses an older Java version, configure `JAVA_HOME` and `PATH` accordingly.

## 5. Next Task — API Gateway

**Status: Not started**

Create the API Gateway Spring Boot project using:

| Setting | Value |
|---|---|
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| Java | 25 |
| Group | `com.courseregistration` |
| Artifact | `api-gateway` |
| Package | `com.courseregistration.gateway` |
| Build | Maven |
| Configuration | YAML |

Required dependencies:

- Spring Cloud Gateway Server WebFlux
- Eureka Discovery Client

Implementation objectives:

1. Generate the project in `backend/api-gateway`.
2. Verify the generated `pom.xml` and dependency versions.
3. Configure the Gateway application.
4. Configure Eureka connectivity.
5. Define and test initial routing behavior.
6. Run `mvn clean verify`.
7. Verify the Gateway works with the running Discovery Server.
8. Commit and push the verified changes to `development`.

Do not mark the API Gateway as completed until its configuration and tests have been verified.

## 6. Remaining Documentation

- Finalize functional and non-functional requirements.
- Define microservice responsibilities and API contracts.
- Prepare UML diagrams and OCL constraints.
- Design database schemas and ERD.
- Document testing procedures.
- Prepare Docker deployment instructions.

## 7. Team Handoff Notes

- Work on the shared `development` branch.
- Do not commit directly to `main`.
- Pull the latest changes before starting.
- Coordinate ownership of shared files.
- Test changes before pushing.
- Never commit credentials or secrets.
- Update this progress document after completing a task.
- Merge into `main` through a reviewed Pull Request when a stable milestone is ready.

**Next development starting point:** `backend/api-gateway`.
