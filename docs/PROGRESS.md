# Course Registration System — Team Progress Tracker

**Course:** Software Engineering II

**Last Updated:** November 26, 2026

**Active Branch:** development

**Overall Status:** In Progress

## 1. Purpose

This document tracks the development progress, individual contributions, completed tasks, testing results, blockers, and remaining work of all team members.

Each member is responsible for keeping their assigned section up to date.

## 2. Status Legend

| Status | Meaning |
|---|---|
| 🟢 Done | Implemented and verified |
| 🟡 In Progress | Currently being developed |
| 🔵 Ready for Review | Completed but awaiting review |
| 🟠 Blocked | Waiting for a dependency or issue resolution |
| ⚪ To Do | Planned task |
| ⚫ Not Reported | Current progress has not been reported |

## 3. Team Overview

| Member | Responsibility | Status |
|---|---|---|
| Member 1 | Infrastructure & Integration | 🟡 In Progress |
| Member 2 | Authentication & Students | ⚫ Not Reported |
| Member 3 | Course Management | ⚫ Not Reported |
| Member 4 | Enrollment Management | ⚫ Not Reported |
| Member 5 | Grades & AOP | ⚫ Not Reported |
| Member 6 | Frontend & Documentation | ⚫ Not Reported |

---

## 4. Member 1 — Infrastructure & Integration

**Name:** [Member Name]

**Last Updated:** November 26, 2026

**Overall Status:** 🟡 In Progress

### Task Checklist

- [x] Prepare development environment.
- [x] Establish agreed technology versions.
- [x] Initialize GitHub repository.
- [x] Create project folder structure.
- [x] Configure .gitignore and .editorconfig.
- [x] Establish main and development branches.
- [x] Prepare initial project documentation.
- [x] Initialize Eureka Discovery Server.
- [x] Configure Eureka Server on port 8761.
- [x] Build and test Discovery Server.
- [x] Verify Eureka Dashboard.
- [x] Initialize API Gateway.
- [x] Configure Gateway on port 8090.
- [x] Configure Eureka Discovery Client.
- [x] Add Spring Cloud LoadBalancer dependency.
- [x] Configure Gateway routes.
- [x] Build and test API Gateway.
- [x] Verify Gateway registration with Eureka.
- [x] Verify Gateway health endpoint.
- [x] Test initial unavailable-service routing behavior.
- [x] Draft shared API and security standards.
- [ ] Obtain team approval for shared standards.
- [x] Update existing infrastructure documentation.
- [x] Create Eureka Dockerfile.
- [x] Create API Gateway Dockerfile.
- [x] Create initial Docker Compose configuration.
- [x] Verify infrastructure containers.
- [x] Standardize infrastructure Maven wrappers and pin Docker base images.
- [x] Commit and push the initial Docker milestone and documentation (925337c).
- [x] Add Compose health checks and healthy startup dependency.
- [x] Verify a clean Docker Compose build locally.
- [ ] Verify a clean build on another team member’s machine.
- [ ] Commit and push health-check improvements and updated documentation.
- [ ] Prepare Architecture Diagram.
- [ ] Verify successful business-service routing.
- [ ] Perform Load Balancing Testing.
- [ ] Coordinate security integration.
- [ ] Integrate all business microservices.
- [ ] Collect and organize Postman collections.
- [ ] Complete final Docker Compose configuration.
- [ ] Perform system and integration testing.
- [ ] Finalize README and deployment instructions.

### Verified Testing Results

| Test | Result |
|---|---|
| Discovery Server Maven Build | Passed |
| Discovery Server Tests | Passed |
| Eureka Dashboard | Passed |
| API Gateway Maven Build | Passed |
| API Gateway Tests | Passed |
| Gateway Eureka Registration | Passed |
| Gateway Health Check | HTTP 200 — UP |
| Initial Auth Route | Expected HTTP 503 |
| Initial Students Route | Expected HTTP 503 |
| Initial Course Route | Expected HTTP 503 |
| Initial Enrollment Route | Expected HTTP 503 |
| Initial Grade Route | Expected HTTP 503 |
| Successful Business-Service Routing | Pending |
| Load Balancing Testing | Pending |
| Docker Compose configuration and cached build | Passed |
| Multi-stage infrastructure images | Built and tested |
| Shared Docker bridge network | Verified |
| Containerized Eureka health | HTTP 200 — UP |
| Containerized Gateway health | HTTP 200 — UP |
| Eureka registry lookup | HTTP 200 — API-GATEWAY UP |
| Gateway registration logs | HTTP 204 |
| Containerized course route without destination | Expected HTTP 503 |
| Clean Compose build on another machine | Pending |
| Compose readiness checks | Passed — both containers healthy (0.0.3) |
| Local clean Compose build | Passed — both images built in 143.6 seconds |
| Healthy startup dependency | Passed — Eureka healthy before Gateway startup |
| Version 0.0.3 regression checks | Passed — health 200, registry UP, registration 204, expected course route 503 |

The HTTP 503 responses are expected while the destination business services are unavailable. They do not confirm successful request forwarding or load distribution.

### Contribution History

| Date | Contribution | Commit |
|---|---|---|
| 2026-11-21 | Eureka Discovery Server implementation | 4b8cd1f |
| — | Initial project documentation | 1ba3026 |
| 2026-11-26 | API Gateway initialization and routing | de72ed2 |
| 2026-11-26 | Draft shared API and security standards and documentation | 5c6318f |
| 2026-11-26 | Pinned multi-stage Dockerfiles, Maven Wrapper alignment, initial Compose, verified infrastructure and updated documentation | 925337c |
| 2026-11-26 | Compose health checks, curl runtime installation, healthy startup dependency, clean build and infrastructure regression testing | Pending commit |

### Blockers

- Load balancing testing requires multiple running instances of a business microservice.

- Successful business-service routing requires running destination services.

- Final integration depends on the remaining team members.

- Shared security conventions require coordination with the authentication service owner.

### Next Steps

1. Review, commit and push the health-check milestone and updated documentation.
2. Coordinate team approval of shared API and security standards.
3. Prepare the Architecture Diagram with the team.
4. Verify a clean Compose build on another team member's machine.
5. Integrate business services as they become available.
6. Verify successful routing, load balancing, security and final system integration.

The initial Compose environment contains only Eureka and API Gateway. Both version 0.0.3 containers are healthy. Health checks use curl against `/actuator/health` every 10 seconds, with a 5-second timeout, 5 retries and a 40-second start period. Gateway startup depends on Eureka being healthy. The curl package version is not pinned. Docker packaging skips Maven tests; local Maven verification was performed separately. Full deployment remains pending.

---

## 5. Member 2 — Authentication & Students

**Name:** [Member Name]

**Assigned Service:** auth-service

**Assigned Port:** 8081

**Last Updated:** —

**Overall Status:** ⚫ Not Reported

### Task Checklist

- [ ] Initialize Auth Service.
- [ ] Configure PostgreSQL database.
- [ ] Design user and student entities.
- [ ] Implement repositories.
- [ ] Implement request and response DTOs.
- [ ] Implement student registration.
- [ ] Implement user login.
- [ ] Implement student profile management.
- [ ] Implement BCrypt password hashing.
- [ ] Implement JWT generation and validation.
- [ ] Implement Spring Security.
- [ ] Implement STUDENT and ADMIN roles.
- [ ] Implement endpoint-level authorization.
- [ ] Implement input validation.
- [ ] Implement global exception handling.
- [ ] Configure Eureka registration.
- [ ] Coordinate JWT configuration with other services.
- [ ] Prepare Swagger documentation.
- [ ] Prepare Postman requests.
- [ ] Implement unit and integration tests.
- [ ] Create Dockerfile.
- [ ] Prepare Use Case Diagram.
- [ ] Document main use case descriptions.
- [ ] Prepare student-related ERD components.
- [ ] Prepare authentication-related Class Diagram components.
- [ ] Document functional requirements and API contracts.
- [ ] Verify integration through API Gateway.

### Contribution History

| Date | Contribution | Commit | Test Result |
|---|---|---|---|
| — | — | — | — |

### Blockers

- Not reported.

### Next Steps

- To be updated by Member 2.

---

## 6. Member 3 — Course Management

**Name:** [Member Name]

**Assigned Service:** course-service

**Assigned Port:** 8082

**Last Updated:** —

**Overall Status:** ⚫ Not Reported

### Task Checklist

- [ ] Initialize Course Service.
- [ ] Configure PostgreSQL database.
- [ ] Design course entities.
- [ ] Implement repositories.
- [ ] Implement request and response DTOs.
- [ ] Implement course creation.
- [ ] Implement course updates.
- [ ] Implement course deletion rules.
- [ ] Implement course listing.
- [ ] Implement course search and filtering, subject to approved scope.
- [ ] Implement course capacity management.
- [ ] Implement course availability checks.
- [ ] Implement course business rules.
- [ ] Implement input validation.
- [ ] Implement global exception handling.
- [ ] Implement JWT validation and role authorization.
- [ ] Configure Eureka registration.
- [ ] Prepare Swagger documentation.
- [ ] Prepare Postman requests.
- [ ] Implement unit and integration tests.
- [ ] Create Dockerfile.
- [ ] Coordinate the system Class Diagram.
- [ ] Prepare course-related ERD components.
- [ ] Define course-related OCL constraints.
- [ ] Enforce approved OCL constraints in the implementation.
- [ ] Document functional requirements and API contracts.
- [ ] Verify integration through API Gateway.

### Contribution History

| Date | Contribution | Commit | Test Result |
|---|---|---|---|
| — | — | — | — |

### Blockers

- Not reported.

### Next Steps

- To be updated by Member 3.

---

## 7. Member 4 — Enrollment Management

**Name:** [Member Name]

**Assigned Service:** enrollment-service

**Assigned Port:** 8083

**Last Updated:** —

**Overall Status:** ⚫ Not Reported

### Task Checklist

- [ ] Initialize Enrollment Service.
- [ ] Configure PostgreSQL database.
- [ ] Design enrollment entities.
- [ ] Implement repositories.
- [ ] Implement request and response DTOs.
- [ ] Implement course registration.
- [ ] Implement course withdrawal.
- [ ] Implement enrollment listing.
- [ ] Prevent duplicate enrollment.
- [ ] Validate course capacity.
- [ ] Validate approved enrollment eligibility rules.
- [ ] Handle concurrent enrollment requests.
- [ ] Configure OpenFeign.
- [ ] Integrate with Auth Service.
- [ ] Integrate with Course Service.
- [ ] Implement input validation.
- [ ] Implement global exception handling.
- [ ] Implement JWT validation and role authorization.
- [ ] Configure Eureka registration.
- [ ] Prepare Swagger documentation.
- [ ] Prepare Postman requests.
- [ ] Implement unit and integration tests.
- [ ] Create Dockerfile.
- [ ] Prepare Sequence Diagram.
- [ ] Prepare Activity Diagram.
- [ ] Prepare enrollment-related ERD components.
- [ ] Prepare enrollment-related Class Diagram components.
- [ ] Define enrollment-related OCL constraints.
- [ ] Enforce approved OCL constraints in the implementation.
- [ ] Document functional requirements and API contracts.
- [ ] Verify integration through API Gateway.

### Contribution History

| Date | Contribution | Commit | Test Result |
|---|---|---|---|
| — | — | — | — |

### Blockers

- Not reported.

### Next Steps

- To be updated by Member 4.

---

## 8. Member 5 — Grades & AOP

**Name:** [Member Name]

**Assigned Service:** grade-service

**Assigned Port:** 8084

**Last Updated:** —

**Overall Status:** ⚫ Not Reported

### Task Checklist

- [ ] Initialize Grade Service.
- [ ] Configure PostgreSQL database.
- [ ] Design grade entities.
- [ ] Implement repositories.
- [ ] Implement request and response DTOs.
- [ ] Implement grade assignment.
- [ ] Implement grade updates.
- [ ] Implement student grade retrieval.
- [ ] Implement GPA calculation, subject to approved scope.
- [ ] Validate grade ranges.
- [ ] Verify student enrollment before assigning grades.
- [ ] Configure required inter-service communication.
- [ ] Implement input validation.
- [ ] Implement global exception handling.
- [ ] Implement JWT validation and role authorization.
- [ ] Configure Eureka registration.
- [ ] Implement Logging Aspect.
- [ ] Implement Execution Time Aspect using @Around.
- [ ] Document AOP pointcuts and advice types.
- [ ] Verify AOP execution logs.
- [ ] Prepare Swagger documentation.
- [ ] Prepare Postman requests.
- [ ] Implement unit and integration tests.
- [ ] Create Dockerfile.
- [ ] Prepare grade-related ERD components.
- [ ] Prepare grade-related Class Diagram components.
- [ ] Define grade-related OCL constraints.
- [ ] Enforce approved OCL constraints in the implementation.
- [ ] Document functional requirements and API contracts.
- [ ] Verify integration through API Gateway.

### Contribution History

| Date | Contribution | Commit | Test Result |
|---|---|---|---|
| — | — | — | — |

### Blockers

- Not reported.

### Next Steps

- To be updated by Member 5.

---

## 9. Member 6 — Frontend & Documentation

**Name:** [Member Name]

**Assigned Application:** React Frontend

**Assigned Port:** 5173

**Last Updated:** —

**Overall Status:** ⚫ Not Reported

### Task Checklist

- [ ] Initialize React and Vite application.
- [ ] Establish frontend folder structure.
- [ ] Implement responsive layout.
- [ ] Implement login page.
- [ ] Implement registration page.
- [ ] Implement Student Dashboard.
- [ ] Implement Admin Dashboard.
- [ ] Implement student profile pages.
- [ ] Implement course listing pages.
- [ ] Implement course management pages.
- [ ] Implement enrollment pages.
- [ ] Implement grade management pages.
- [ ] Implement student grades page.
- [ ] Implement GPA display, subject to approved scope.
- [ ] Implement JWT authentication handling.
- [ ] Implement protected routes.
- [ ] Implement role-based navigation.
- [ ] Configure API Gateway integration.
- [ ] Implement loading states.
- [ ] Implement error handling.
- [ ] Test frontend functionality.
- [ ] Verify frontend and backend integration.
- [ ] Coordinate Software Requirements Specification.
- [ ] Collect and review UML diagrams.
- [ ] Coordinate final ERD.
- [ ] Collect at least five approved OCL constraints.
- [ ] Document OCL enforcement locations.
- [ ] Prepare final project report.
- [ ] Collect individual member contributions.
- [ ] Prepare project cover sheet.
- [ ] Review final documentation consistency.

### Contribution History

| Date | Contribution | Commit | Test Result |
|---|---|---|---|
| — | — | — | — |

### Blockers

- Not reported.

### Next Steps

- To be updated by Member 6.

---

## 10. Service Status

| Service | Port | Status |
|---|---:|---|
| Discovery Server | 8761 | 🟢 Verified locally and in Docker |
| API Gateway | 8090 | 🟢 Infrastructure verified locally and in Docker |
| Auth Service | 8081 | ⚫ Not Reported |
| Course Service | 8082 | ⚫ Not Reported |
| Enrollment Service | 8083 | ⚫ Not Reported |
| Grade Service | 8084 | ⚫ Not Reported |
| React Frontend | 5173 | ⚫ Not Reported |
| PostgreSQL | 5432 | Database integration pending verification |
| Docker Compose | — | 🟢 Initial infrastructure and health checks verified (0.0.3); complete system pending |

## 11. Technology Baseline

| Technology | Version |
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

Infrastructure Maven Wrapper: 3.3.4 (Maven 3.10.1). Docker Java runtime: Eclipse Temurin 25.0.5.1+2. Docker Engine 29.9.3 and Compose 5.7.0 were tested locally; team adoption is pending.

These versions represent the recorded team baseline. Each member must verify their local environment and service builds.

## 12. Project Deliverables Tracker

| Deliverable | Owner | Status |
|---|---|---|
| Software Requirements Specification | Member 6 + Team | Draft |
| Architecture Diagram | Member 1 | Pending |
| Use Case Diagram | Member 2 | Not Reported |
| Use Case Descriptions | Member 2 | Not Reported |
| Class Diagram | Member 3 + Team | Not Reported |
| Sequence Diagram | Member 4 | Not Reported |
| Activity Diagram | Member 4 | Not Reported |
| ERD | Member 6 + Backend Members | Not Reported |
| OCL Constraints | Members 3, 4, 5 | Not Reported |
| API & Security Standards | Member 1 + Member 2 | Draft |
| Swagger Documentation | Backend Members | Not Reported |
| Postman Collection | Member 1 + Backend Members | Pending |
| Dockerfiles | Backend Members | Eureka and Gateway verified; business services pending |
| Docker Compose | Member 1 | Initial infrastructure and health checks verified; final configuration pending |
| AOP Implementation | Member 5 | Not Reported |
| Final Report | Member 6 + Team | Pending |
| Member Contributions | All Members | In Progress |
| Final Integration Testing | Member 1 + Team | Pending |

## 13. Shared Documentation

- README.md — Project overview and startup instructions.

- docs/DEVELOPMENT_GUIDE.md — Development environment and team workflow.

- docs/API_STANDARDS.md — Proposed API and security conventions.

- docs/requirements/SRS.md — Software Requirements Specification.

- docs/architecture/ARCHITECTURE.md — Architecture documentation.

- docs/PROGRESS.md — Team progress and contribution tracking.

## 14. Progress Update Rules

Every team member must:

1. Update their own section after completing a task.

2. Mark a task as completed only after verifying its implementation.

3. Record the date and Git commit ID.

4. Record relevant testing results.

5. Report blockers and dependencies.

6. Update their next steps.

7. Pull the latest development branch before editing.

8. Avoid modifying other members' sections without coordination.

9. Never include credentials, tokens, or secrets.

10. Commit their own work and progress updates.

An unchecked task does not necessarily mean work has not started. Members must report their actual status.

## 15. Progress Update Template

Use the following information when recording a new contribution:

**Date:** YYYY-MM-DD

**Member:** [Name]

**Task:** [Task name]

**Status:** [Done / In Progress / Blocked / Ready for Review]

**Description:** [Summary of the work]

**Commit ID:** [Git commit hash]

**Testing:** [Test performed and result]

**Blockers:** [None or description]

**Next Steps:** [Upcoming work]

## 16. Final Completion Criteria

The project is ready for final delivery when:

- [ ] All four business microservices are implemented.
- [ ] All required services register with Eureka.
- [ ] API Gateway routes successfully to business services.
- [ ] Load balancing is verified.
- [ ] Authentication and authorization are verified.
- [ ] Business rules are enforced.
- [ ] Required OCL constraints are documented and enforced.
- [ ] At least two AOP aspects are implemented and verified.
- [ ] Frontend is fully integrated.
- [ ] Swagger documentation is complete.
- [ ] Postman collection is complete.
- [ ] Docker Compose starts the complete system.
- [ ] Required UML diagrams and ERD are complete.
- [ ] Final SRS and report are complete.
- [ ] Individual contributions are documented.
- [ ] All team members understand the complete system.
- [ ] Final reviewed milestone is merged into main.

---

**Repository:** https://github.com/KiraIIV-max/course-registration-system

**Document Owner:** All Team Members

**Progress Coordinator:** Member 1
