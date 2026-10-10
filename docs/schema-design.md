# Smart Clinic Management System - MySQL Relational Database Design

## 1. Overview
The MySQL database for the Smart Clinic Management System stores core relational data, including user credentials, doctor profiles, patient demographics, and appointment schedules.

---

## 2. Table Schemas

### 2.1 Users Table (`users`)
Stores authentication and role-based access information.
| Column Name | Data Type | Constraints | Description |
|---|---|---|---|
| `id` | BIGINT | PRIMARY KEY, AUTO_INCREMENT | Unique user identifier |
| `username` | VARCHAR(50) | NOT NULL, UNIQUE | User login username |
| `password` | VARCHAR(255) | NOT NULL | Hashed user password |
| `role` | VARCHAR(20) | NOT NULL | User role (`ADMIN`, `DOCTOR`, `PATIENT`) |
| `email` | VARCHAR(100) | NOT NULL, UNIQUE | User email address |

---

### 2.2 Doctors Table (`doctors`)
Stores doctor profile and specialization details.
| Column Name | Data Type | Constraints | Description |
|---|---|---|---|
| `id` | BIGINT | PRIMARY KEY, AUTO_INCREMENT | Unique doctor identifier |
| `user_id` | BIGINT | FOREIGN KEY (`users.id`), NOT NULL | Reference to associated user |
| `name` | VARCHAR(100) | NOT NULL | Full name of doctor |
| `specialization` | VARCHAR(100) | NOT NULL | Field of specialization (e.g., Cardiology) |
| `phone` | VARCHAR(15) | NOT NULL | Contact number |

---

### 2.3 Patients Table (`patients`)
Stores patient details and medical history lookup keys.
| Column Name | Data Type | Constraints | Description |
|---|---|---|---|
| `id` | BIGINT | PRIMARY KEY, AUTO_INCREMENT | Unique patient identifier |
| `user_id` | BIGINT | FOREIGN KEY (`users.id`), NOT NULL | Reference to associated user |
| `name` | VARCHAR(100) | NOT NULL | Patient full name |
| `email` | VARCHAR(100) | NOT NULL, UNIQUE | Patient contact email |
| `phone` | VARCHAR(15) | NOT NULL | Patient contact phone number |
| `date_of_birth` | DATE | NOT NULL | Date of birth |

---

### 2.4 Appointments Table (`appointments`)
Manages appointment bookings and scheduling relationships.
| Column Name | Data Type | Constraints | Description |
|---|---|---|---|
| `id` | BIGINT | PRIMARY KEY, AUTO_INCREMENT | Unique appointment identifier |
| `doctor_id` | BIGINT | FOREIGN KEY (`doctors.id`), NOT NULL | Scheduled doctor |
| `patient_id` | BIGINT | FOREIGN KEY (`patients.id`), NOT NULL | Booking patient |
| `appointment_time` | DATETIME | NOT NULL | Scheduled date and time |
| `status` | VARCHAR(20) | NOT NULL | Status (`SCHEDULED`, `COMPLETED`, `CANCELLED`) |

---

## 3. Relationships & Foreign Keys
- `doctors.user_id` $\rightarrow$ `users.id` (1-to-1 relationship)
- `patients.user_id` $\rightarrow$ `users.id` (1-to-1 relationship)
- `appointments.doctor_id` $\rightarrow$ `doctors.id` (Many-to-1 relationship)
- `appointments.patient_id` $\rightarrow$ `patients.id` (Many-to-1 relationship)
