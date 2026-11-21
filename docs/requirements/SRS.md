# Software Requirements Specification (SRS)

**Project:** University Course Registration System  
**Course:** Software Engineering II  
**Version:** Draft 1.0  
**Date:** November 21, 2026  
**Status:** Proposed requirements — pending team and instructor review

## 1. Introduction

### 1.1 Purpose
This document describes the proposed functional and non-functional requirements of a web-based university course registration platform. It provides a common reference for design, implementation, and testing.

### 1.2 Scope
The platform will support authentication, course management, student enrollment, and grade management through a React frontend and Spring Boot microservices. This draft does not assume integration with an existing university student-information system.

### 1.3 Stakeholders and Actors
- **Student:** browses courses, enrolls, drops courses, and views personal records.
- **Administrator:** maintains courses, capacity, and authorized academic records.
- **Academic staff / instructor (proposed):** may enter grades if the team approves a separate role; otherwise grade permissions must be assigned explicitly to an existing role.
- **Development team:** builds, tests, deploys, and maintains the platform.

### 1.4 Terminology
- **Course:** an academic offering available for registration.
- **Enrollment:** a student's registration in a course.
- **Capacity:** maximum allowed active enrollments for a course.
- **JWT:** JSON Web Token used for authenticated requests.
- **OCL:** Object Constraint Language for expressing model constraints.

## 2. Product Description

### 2.1 Product Perspective
The proposed system is a standalone educational project using independent backend services behind an API Gateway. Eureka provides service discovery. Each business service owns its data; cross-service operations use defined APIs rather than direct database access.

### 2.2 Assumptions and Dependencies
- Users have unique identifiers and authenticated accounts.
- Courses have unique identifiers and defined capacities.
- An enrollment links a student and a course.
- The grading scale, enrollment deadlines, prerequisites, maximum course load, and academic term rules are **not finalized**.
- Availability and correctness of cross-service APIs are required for enrollment validation.

## 3. Functional Requirements

The following IDs are proposed and can be traced to future test cases.

### 3.1 Authentication and Authorization
| ID | Requirement | Priority |
|---|---|---|
| FR-AUTH-01 | The system shall authenticate registered users using credentials. | Must |
| FR-AUTH-02 | The system shall issue JWTs after successful authentication. | Must |
| FR-AUTH-03 | The system shall hash stored passwords using an appropriate secure algorithm such as BCrypt. | Must |
| FR-AUTH-04 | The system shall enforce role-based authorization for protected operations. | Must |
| FR-AUTH-05 | The system shall reject unauthenticated or unauthorized access to protected endpoints. | Must |

### 3.2 Course Management
| ID | Requirement | Priority |
|---|---|---|
| FR-COURSE-01 | Students shall be able to view available courses. | Must |
| FR-COURSE-02 | Users shall be able to search and filter courses using supported criteria. | Should |
| FR-COURSE-03 | Authorized administrators shall be able to create and update courses. | Must |
| FR-COURSE-04 | Authorized administrators shall be able to set and update course capacity. | Must |
| FR-COURSE-05 | Course deletion shall respect constraints involving existing enrollments. | Must |

### 3.3 Enrollment Management
| ID | Requirement | Priority |
|---|---|---|
| FR-ENR-01 | Authenticated students shall be able to request enrollment in an available course. | Must |
| FR-ENR-02 | The system shall prevent duplicate active enrollment in the same course for the same student. | Must |
| FR-ENR-03 | The system shall prevent enrollment when course capacity is exhausted. | Must |
| FR-ENR-04 | Students shall be able to view their own enrollments. | Must |
| FR-ENR-05 | Students shall be able to drop courses when permitted by the approved enrollment policy. | Must |
| FR-ENR-06 | The system shall reject enrollment requests referencing unknown or ineligible records. | Must |

### 3.4 Grade Management
| ID | Requirement | Priority |
|---|---|---|
| FR-GRADE-01 | Authorized users shall be able to assign and update grades for valid enrollments. | Must |
| FR-GRADE-02 | Students shall be able to view only their own grades. | Must |
| FR-GRADE-03 | The system shall validate grades against an approved grading scale. | Must |
| FR-GRADE-04 | The system shall prevent unauthorized grade changes. | Must |

### 3.5 Platform and Documentation
| ID | Requirement | Priority |
|---|---|---|
| FR-PLAT-01 | Client requests shall enter backend services through the API Gateway. | Must |
| FR-PLAT-02 | Backend services shall register with or be discoverable through Eureka where applicable. | Must |
| FR-PLAT-03 | Applicable services shall expose documented REST API contracts. | Must |
| FR-PLAT-04 | The backend shall include logging and execution-time measurement aspects. | Must |

## 4. Business Rules

| ID | Proposed rule |
|---|---|
| BR-01 | A student cannot have two active enrollments in the same course. |
| BR-02 | Active enrollment count must not exceed course capacity. |
| BR-03 | Only authenticated students may initiate enrollment. |
| BR-04 | A student may access only their own private academic data unless explicitly authorized. |
| BR-05 | Grades must be within the approved grading scale. |
| BR-06 | Enrollment and grade operations must reference valid records. |

**Open policy decisions:** prerequisites, enrollment windows, maximum course load, repeat enrollments across terms, grade scale, deletion policy, and academic staff role.

## 5. Non-Functional Requirements

| ID | Category | Proposed requirement / verification |
|---|---|---|
| NFR-01 | Security | Protect credentials, JWT signing secrets, and database passwords; never commit them to Git. |
| NFR-02 | Security | Enforce authorization on backend service boundaries, not only in the frontend. |
| NFR-03 | Data integrity | Ensure capacity and duplicate-enrollment checks remain correct under concurrent requests. |
| NFR-04 | Maintainability | Separate services by business responsibility and document their API contracts. |
| NFR-05 | Testability | Add automated tests for core business rules and relevant integration paths. |
| NFR-06 | Observability | Record useful application errors and timings without exposing sensitive values. |
| NFR-07 | Usability | Provide understandable feedback for successful and rejected operations. |
| NFR-08 | Portability | Support the agreed team development environment and a planned Docker Compose setup. |
| NFR-09 | Performance | Define measurable response-time targets after establishing test data and hardware baseline. |
| NFR-10 | Reliability | Define service failure behavior and retry/time-out policies before integration testing. |

Numerical performance and availability targets are **TBD**, not approved commitments.

## 6. Proposed Use Cases

| ID | Use case | Primary actor | Expected result |
|---|---|---|---|
| UC-01 | Log in | Student / Administrator | Authenticated session/token or clear error |
| UC-02 | Browse and search courses | Student | Matching course list |
| UC-03 | Create/update course | Administrator | Valid course data saved |
| UC-04 | Enroll in course | Student | Enrollment created only if all rules pass |
| UC-05 | Drop course | Student | Enrollment updated if dropping is permitted |
| UC-06 | View enrollments | Student | Only own enrollment records returned |
| UC-07 | Assign/update grade | Authorized user | Valid grade stored for valid enrollment |
| UC-08 | View grades | Student | Only own grades returned |

### 6.1 UC-04 — Enroll in Course
**Preconditions:** Student is authenticated; course exists; enrollment policy permits registration.  
**Main flow:** Student selects a course → system validates eligibility and duplicate enrollment → checks capacity → creates enrollment → returns confirmation.  
**Alternative flows:** Course missing, duplicate enrollment, capacity full, or policy violation → no enrollment created and a suitable error returned.  
**Postcondition:** On success, exactly one new active enrollment exists and capacity is not exceeded.

## 7. External Interfaces

- **Frontend:** React + Vite web application, communicating over HTTP(S).
- **Backend:** REST APIs exposed through Spring Cloud Gateway.
- **Discovery:** Eureka Server; implemented locally on port `8761`.
- **Persistence:** PostgreSQL, with service-owned data boundaries.
- **API documentation:** OpenAPI/Swagger UI for applicable business services (planned).

Exact endpoint paths, payloads, database schemas, and production ports remain to be designed.

## 8. Verification and Traceability

- Map each `FR-*` requirement to one or more API, unit, or integration tests.
- Validate `BR-01` and `BR-02` with positive, negative, and concurrent-enrollment tests.
- Validate `FR-GRADE-02` and `FR-AUTH-04` with cross-user authorization tests.
- Keep UML use cases, class models, and OCL constraints consistent with approved requirements.
- Review changes to requirements before implementing dependent services.

## 9. Open Questions for Team Approval

1. Is there a separate Instructor/Academic Staff role, or are administrators responsible for grades?
2. What is the exact grading scale?
3. Are course offerings associated with semesters/sections, or just courses?
4. Are prerequisites, enrollment deadlines, and maximum course loads required?
5. What happens to enrollments and grades when a course is deleted?
6. What measurable performance and availability targets are realistic for the course project?
7. Does the instructor mandate specific diagram counts, OCL constraints, or architectural patterns beyond the current README assumptions?

---

**Document ownership:** Project team. **Approval status:** Draft; review against official course assignment before treating it as final.
