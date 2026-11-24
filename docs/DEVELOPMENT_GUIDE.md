# Development Guide — Course Registration System

**Project:** University Course Registration System  
**Course:** Software Engineering II  
**Active Branch:** `development`  
**Last Updated:** November 21, 2026

## 1. Purpose

This guide defines the shared development environment, Git workflow, project structure, and team collaboration rules.

All contributors should follow these conventions to keep the project consistent and avoid integration problems.

## 2. Technology Versions

The team should use the following agreed development baseline:

| Technology | Version |
|---|---|
| Java JDK | 25 |
| Spring Boot | 4.1.3 |
| Spring Cloud | 2025.1.5 |
| Apache Maven | 3.10.1 |
| PostgreSQL | 18.7 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Docker Engine | 29.8.0 |
| Docker Compose | 5.5.1 |
| Git | 2.49.0 |

Use the existing Maven Wrapper where available. Do not independently upgrade project dependencies without team agreement.

## 3. Initial Setup

### Clone the Repository

```cmd
git clone https://github.com/KiraIIV-max/course-registration-system.git
cd course-registration-system
git switch development
git pull --ff-only origin development
```

### Verify Tools

Run:

```cmd
java -version
javac -version
mvn -version
node -v
npm -v
git --version
docker --version
docker compose version
```

Confirm that Maven uses JDK 25.

### Java Configuration on Windows

If Maven uses an older Java version, locate JDK 25 and configure `JAVA_HOME` and `PATH`.

For a temporary CMD session, if JDK 25 is installed at `C:\Program Files\Java\jdk-25`:

```cmd
set "JAVA_HOME=C:\Program Files\Java\jdk-25"
set "PATH=%JAVA_HOME%\bin;%PATH%"
mvn -version
```

These changes apply only to the current CMD session.

## 4. Repository Structure

```text
course-registration-system/
├── backend/
│   ├── discovery-server/
│   ├── api-gateway/
│   ├── auth-service/
│   ├── course-service/
│   ├── enrollment-service/
│   └── grade-service/
├── frontend/
├── docker/
├── docs/
│   ├── api/
│   ├── architecture/
│   ├── database/
│   ├── requirements/
│   ├── uml/
│   ├── DEVELOPMENT_GUIDE.md
│   └── PROGRESS.md
└── README.md
```

Some directories are placeholders for components that have not been implemented yet.

## 5. Current Implementation

The Eureka Discovery Server is the first completed microservice.

**Location:** `backend/discovery-server`  
**Application name:** `discovery-server`  
**Port:** `8761`

The API Gateway and business microservices have not been implemented yet.

For the latest task status, see `docs/PROGRESS.md`.

## 6. Running Eureka Discovery Server

From the repository root:

```cmd
cd backend\discovery-server
mvn clean verify
mvn spring-boot:run
```

Open:

http://localhost:8761

The Eureka Dashboard should load successfully.

`No instances available` is expected until other microservices register with Eureka.

Stop the application with `Ctrl + C`.

## 7. Git Branching Strategy

The project uses two shared branches:

| Branch | Purpose |
|---|---|
| `main` | Stable, reviewed milestones |
| `development` | Active development and integration |

Do not commit directly to `main`.

Feature branches are not part of the currently agreed team workflow.

### Before Starting Work

```cmd
git switch development
git pull --ff-only origin development
git status
```

Ensure the working tree is clean before beginning a new task.

### After Completing a Task

Run the appropriate tests and inspect the changes:

```cmd
git status --short
git diff
```

Stage only the files related to your task:

```cmd
git add path/to/changed/files
git diff --cached
```

Commit and push:

```cmd
git commit -m "feat: describe completed task"
git pull --rebase origin development
git push origin development
```

If Git reports conflicts, stop and coordinate with the team before resolving them.

### Merge to Main

Merge `development` into `main` only after a milestone is stable, tested, and reviewed.

Use a GitHub Pull Request for milestone integration.

## 8. Team Collaboration Rules

- Coordinate task ownership before editing shared files.
- Pull the latest `development` changes before starting work.
- Avoid changing files assigned to another contributor without agreement.
- Keep commits focused on one task.
- Review changes before pushing.
- Run relevant tests before pushing.
- Never commit passwords, tokens, credentials, or environment secrets.
- Update documentation when implementation changes.
- Notify the team when API contracts or shared configuration change.
- Do not force-push shared branches.

## 9. Backend Development Conventions

- Keep each microservice in its assigned backend directory.
- Use the agreed Java, Spring Boot, and Spring Cloud versions.
- Use Maven for backend builds.
- Keep application configuration in the appropriate resources directory.
- Prefer environment variables for secrets and machine-specific settings.
- Use meaningful class, package, and method names.
- Keep controllers, services, repositories, and data models separated as appropriate.
- Validate external inputs.
- Add tests for implemented behavior.
- Do not access another microservice's database directly.

## 10. Testing Checklist

Before marking a backend task as complete:

- [ ] The project compiles.
- [ ] `mvn clean verify` succeeds.
- [ ] Relevant tests pass.
- [ ] The service starts successfully.
- [ ] Required endpoints or infrastructure features are verified.
- [ ] No secrets or generated build artifacts are staged.
- [ ] Documentation reflects the completed work.

## 11. Documentation Responsibilities

Update the relevant documentation when a task changes the project:

| Document | When to update |
|---|---|
| `README.md` | Project-wide changes, setup, and architecture overview |
| `docs/PROGRESS.md` | After completing or starting a milestone |
| `docs/DEVELOPMENT_GUIDE.md` | Development workflow or tooling changes |
| `docs/requirements/` | Requirements and business rules |
| `docs/architecture/` | Service boundaries and architecture decisions |
| `docs/database/` | Database schemas and relationships |
| `docs/api/` | API contracts and endpoint documentation |
| `docs/uml/` | UML models and OCL constraints |

## 12. Next Task

**API Gateway initialization**

Use Spring Initializr with:

- Spring Boot `4.1.3`
- Spring Cloud `2025.1.5`
- Java `25`
- Maven
- Group: `com.courseregistration`
- Artifact: `api-gateway`
- Package: `com.courseregistration.gateway`
- Configuration: YAML
- Dependencies: Spring Cloud Gateway Server WebFlux and Eureka Discovery Client

Create the project under `backend/api-gateway`.

Verify the generated Maven configuration before building, then configure and test Eureka integration.

Do not mark the Gateway as completed until it has passed its build and runtime checks.

## 13. Handoff Checklist

Before ending a work session:

- [ ] Save and test completed work.
- [ ] Update `docs/PROGRESS.md`.
- [ ] Check Git status.
- [ ] Commit only intended files.
- [ ] Push verified changes to `development`.
- [ ] Share the completed task, latest commit, and next action with the team.

---

**Repository:** https://github.com/KiraIIV-max/course-registration-system

**Primary integration branch:** `development`
