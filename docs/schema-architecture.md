# Smart Clinic Management System - System Architecture & Request Flow

## System Overview
The Smart Clinic Management System (CMS) is built using a 3-Tier Enterprise Architecture consisting of:
1. **Presentation Layer:** Client-side HTML/CSS/JavaScript with Thymeleaf templates.
2. **Application Layer:** Java Spring Boot application implementing REST APIs, business logic services, and JWT authentication.
3. **Data Layer:** MySQL relational database for user management, appointments, and schedules; MongoDB NoSQL database for prescription medical documents.

---

## 7-Step Sequence of Request Flow

When a request is submitted by a user (e.g., booking an appointment or querying doctor availability), the application processes the request through the following sequence:

1. **HTTP Request Generation (Client):** 
   The user interacts with the frontend interface (HTML/JS). An HTTP request (GET, POST, PUT, DELETE) containing JSON payload and JWT Authorization headers is dispatched to the backend API.

2. **Security & Token Validation (Filter Layer):** 
   The request passes through Spring Security filters where `TokenService` parses and verifies the Bearer JWT token to validate the user's session and extract role-based permissions (Patient, Doctor, or Admin).

3. **Routing to Controller (Presentation Layer):** 
   The request is routed to the appropriate REST Controller (e.g., `AppointmentController` or `DoctorController`) annotated with `@RestController` and `@RequestMapping`.

4. **Payload Validation (Controller Layer):** 
   The controller validates incoming request parameters using annotations such as `@Valid`, `@NotNull`, and `@Future`. If validation passes, the request data is passed down to the service layer.

5. **Business Logic Execution (Service Layer):** 
   The Service class (e.g., `AppointmentService` or `DoctorService`) executes core business logic, such as checking doctor availability, verifying slot collisions, or processing business rules.

6. **Data Access & Persistence (Repository Layer):** 
   The Service interacts with Spring Data JPA Repositories (e.g., `PatientRepository`, `AppointmentRepository`) or MongoRepository. The repository executes SQL queries, stored procedures (e.g., `GetDailyAppointmentReportByDoctor`), or NoSQL operations against MySQL / MongoDB.

7. **HTTP Response Mapping (Client Response):** 
   The database returns the entity or result set. The service processes the result, converts entities to DTOs/Responses, and the Controller returns a structured HTTP `ResponseEntity` (e.g., HTTP 200 OK or 201 Created) back to the client.
