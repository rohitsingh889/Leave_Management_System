# Leave Management System – Architecture

## 1. Overview

The Leave Management System follows a layered architecture to separate the user interface, API handling, business logic, data access, and database responsibilities.

The main goal of this architecture is to make the system easier to understand, develop, test, maintain, and scale.

---

## 2. Architecture Diagram

```text
+----------------------+
|       Employee       |
|       Manager        |
+----------+-----------+
           |
           v
+----------------------+
|      Frontend        |
|    Web Application   |
+----------+-----------+
           |
           | HTTPS / JSON
           v
+----------------------+
|      REST API        |
|  Routing & Validation|
+----------+-----------+
           |
           v
+----------------------+
|    Backend /         |
|   Business Logic     |
|                      |
| - Leave Validation   |
| - Balance Checking   |
| - Approval Rules     |
| - Authorization      |
+----------+-----------+
           |
           v
+----------------------+
|    Data Access Layer |
|    ORM / SQL Queries |
+----------+-----------+
           |
           v
+----------------------+
|   Relational Database|
|                      |
| - Employee           |
| - LeaveRequest       |
| - LeaveType          |
| - LeaveBalance       |
| - ApprovalHistory    |
+----------------------+
```


## 3. Architecture Layers

### Frontend

Provides the user interface for employees and managers.

- Apply and view leave
- View leave balance
- Approve or reject leave

### REST API

Handles communication between the frontend and backend.

- Receives HTTP requests
- Validates requests
- Handles authentication and authorization
- Returns responses

### Backend / Business Logic

Handles the main leave management rules.

- Validate leave dates and leave type
- Check leave balance
- Create leave requests
- Approve or reject requests
- Update leave balance

### Data Access Layer

Communicates with the database using ORM or SQL operations.

### Database

Stores the main system data:

- Employee
- LeaveRequest
- LeaveType
- LeaveBalance
- ApprovalHistory

## 4. Request Flow

    Employee
       ↓
    Frontend
       ↓
    POST /leave-requests
       ↓
    REST API
       ↓
    Validation & Business Logic
       ↓
    Database
       ↓
    API Response
       ↓
    Frontend

## 5. Security

- Authentication is required for protected operations.
- Authorization ensures users can perform permitted actions.
- APIs should use HTTPS.
- Secrets should be stored using environment variables.
- Server-side validation must be applied.

## 6. Architecture Governance

Major changes to APIs, database structure, security, or system components should be reviewed and approved. Architecture documentation should be updated after approved changes.

## 7. Summary

The layered architecture provides clear separation of responsibilities, making the system easier to develop, test, maintain, and scale.