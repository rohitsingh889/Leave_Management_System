# Technical Documentation

## 1. Proposed Architecture

The Leave Management System follows a layered architecture where each layer has a specific responsibility.

### Architecture Flow

    User
      ↓
    Frontend
      ↓
    REST API
      ↓
    Backend / Business Logic
      ↓
    Data Access Layer
      ↓
    Relational Database

### Frontend Layer

The frontend provides the user interface for employees and managers.

Main responsibilities:

- Display leave application forms.
- Display leave requests and their status.
- Display leave balances.
- Provide manager approval and rejection options.
- Send requests to the backend through REST APIs.
- Display API responses and errors.

### REST API Layer

The REST API acts as the communication layer between the frontend and backend.

Main responsibilities:

- Receive HTTP requests.
- Validate request format.
- Authenticate users.
- Check authorization.
- Route requests to the appropriate backend functionality.
- Return appropriate HTTP responses.

### Backend / Business Logic Layer

The backend contains the main application logic and business rules.

Main responsibilities:

- Validate leave requests.
- Check leave balance.
- Validate leave dates.
- Process approval and rejection.
- Update leave request status.
- Apply authorization rules.
- Maintain approval history.

### Data Access Layer

The data access layer manages communication between the application and the database.

Main responsibilities:

- Create leave requests.
- Retrieve employee and leave information.
- Update leave request status.
- Read and update leave balances.
- Store approval history.
- Maintain database operations separately from business logic.

### Database Layer

A relational database is used to store application data.

The database stores:

- Employee information
- Leave types
- Leave balances
- Leave requests
- Approval history

### Request Flow

The leave application flow is:

    Employee
       ↓
    Frontend
       ↓
    POST /leave-requests
       ↓
    REST API
       ↓
    Authentication & Validation
       ↓
    Business Logic
       ↓
    Data Access Layer
       ↓
    Database
       ↓
    Response
       ↓
    Frontend
       ↓
    Employee

---

## 2. Data Entities

The proposed system contains the following main data entities.

### Employee

Stores information about employees who use the system.

| Field | Data Type | Description |
|---|---|---|
| employee_id | Integer | Unique employee identifier |
| name | String | Employee name |
| email | String | Employee email address |
| manager_id | Integer | Manager responsible for the employee |
| role | String | User role |
| status | String | Employee account status |

### LeaveType

Stores the different types of leave available in the organization.

| Field | Data Type | Description |
|---|---|---|
| leave_type_id | Integer | Unique leave type identifier |
| name | String | Leave type name |
| description | String | Description of the leave type |
| annual_limit | Decimal | Maximum allocated leave |
| status | String | Leave type status |

### LeaveBalance

Stores the leave balance of an employee for each leave type.

| Field | Data Type | Description |
|---|---|---|
| balance_id | Integer | Unique balance identifier |
| employee_id | Integer | Reference to employee |
| leave_type_id | Integer | Reference to leave type |
| allocated_days | Decimal | Total allocated leave |
| used_days | Decimal | Leave already used |
| available_days | Decimal | Remaining leave balance |

### LeaveRequest

Stores employee leave requests.

| Field | Data Type | Description |
|---|---|---|
| leave_request_id | Integer | Unique leave request identifier |
| employee_id | Integer | Reference to employee |
| leave_type_id | Integer | Reference to leave type |
| start_date | Date | Leave start date |
| end_date | Date | Leave end date |
| reason | String | Reason for leave |
| status | String | Current request status |
| created_at | DateTime | Request creation time |
| updated_at | DateTime | Last update time |

### ApprovalHistory

Stores the history of actions performed on leave requests.

| Field | Data Type | Description |
|---|---|---|
| history_id | Integer | Unique history identifier |
| leave_request_id | Integer | Reference to leave request |
| action_by | Integer | Employee or manager who performed the action |
| action | String | Action performed |
| comment | String | Optional comment or reason |
| action_at | DateTime | Date and time of action |

---

## 3. Entity Relationships

The main relationships between the entities are:

    Employee
       │
       ├──────────< LeaveRequest
       │
       └──────────< LeaveBalance

    LeaveType
       │
       ├──────────< LeaveRequest
       │
       └──────────< LeaveBalance

    LeaveRequest
       │
       └──────────< ApprovalHistory

### Relationship Description

- One Employee can have many LeaveRequests.
- One Employee can have multiple LeaveBalance records.
- One LeaveType can be associated with many LeaveRequests.
- One LeaveType can be associated with many LeaveBalance records.
- One LeaveRequest can have multiple ApprovalHistory records.

### Primary Keys

- `Employee.employee_id`
- `LeaveType.leave_type_id`
- `LeaveBalance.balance_id`
- `LeaveRequest.leave_request_id`
- `ApprovalHistory.history_id`

### Foreign Keys

- `LeaveRequest.employee_id` → `Employee.employee_id`
- `LeaveRequest.leave_type_id` → `LeaveType.leave_type_id`
- `LeaveBalance.employee_id` → `Employee.employee_id`
- `LeaveBalance.leave_type_id` → `LeaveType.leave_type_id`
- `ApprovalHistory.leave_request_id` → `LeaveRequest.leave_request_id`

---

## 4. API List

The system will expose REST APIs for communication between the frontend and backend.

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/leave-requests` | Create a new leave request |
| GET | `/leave-requests` | Retrieve leave requests |
| GET | `/leave-requests/{id}` | Retrieve a specific leave request |
| PATCH | `/leave-requests/{id}/approve` | Approve a pending leave request |
| PATCH | `/leave-requests/{id}/reject` | Reject a pending leave request |

### POST `/leave-requests`

Creates a new leave request for an authenticated employee.

Purpose:

- Submit a leave request.
- Validate leave details.
- Check available leave balance.
- Create the request with `Pending` status.

### GET `/leave-requests`

Retrieves leave requests based on the user's authorization.

Purpose:

- Allow employees to view their requests.
- Allow authorized managers to view applicable employee requests.

### GET `/leave-requests/{id}`

Retrieves details of a specific leave request.

Purpose:

- View leave type.
- View leave dates.
- View reason.
- View current status.
- View relevant approval information.

### PATCH `/leave-requests/{id}/approve`

Approves a pending leave request.

Purpose:

- Verify manager authorization.
- Verify that the request is still pending.
- Update the request status to `Approved`.
- Update the applicable leave balance.
- Record the approval in approval history.

### PATCH `/leave-requests/{id}/reject`

Rejects a pending leave request.

Purpose:

- Verify manager authorization.
- Verify that the request is still pending.
- Update the request status to `Rejected`.
- Record the rejection reason.
- Add the rejection action to approval history.

---

## 5. Technical Requirements

The proposed architecture should follow these technical requirements:

- REST APIs should use standard HTTP methods.
- API requests and responses should use JSON.
- Protected APIs should require authentication.
- Role-based authorization should be applied.
- User input should be validated on the server.
- Database relationships should maintain data integrity.
- Business logic should remain separate from database operations.
- Sensitive configuration should be stored using environment variables.
- API and database documentation should be updated when the design changes.
- Automated testing should be included for important business rules.

---

## 6. Summary

The proposed system uses a layered architecture consisting of the frontend, REST API, backend business logic, data access layer, and relational database.

The main data entities are Employee, LeaveType, LeaveBalance, LeaveRequest, and ApprovalHistory.

REST APIs provide the communication between the frontend and backend for creating, viewing, approving, and rejecting leave requests.