# Database Documentation

## 1. Overview

The Leave Management System uses a relational database to store and manage employee and leave-related information.

The main database entities are:

- Employee
- LeaveRequest
- LeaveType
- LeaveBalance
- ApprovalHistory

These entities are connected through primary keys and foreign keys to maintain data consistency and relationships.

---

## 2. Employee

The `Employee` entity stores information about employees using the system.

| Field | Data Type | Description |
|---|---|---|
| employee_id | Integer | Unique employee identifier |
| name | String | Employee name |
| email | String | Employee email address |
| manager_id | Integer | ID of the employee's manager |
| role | String | User role such as Employee or Manager |
| status | String | Employee account status |

Primary Key:

`employee_id`

---

## 3. LeaveRequest

The `LeaveRequest` entity stores leave applications submitted by employees.

| Field | Data Type | Description |
|---|---|---|
| leave_request_id | Integer | Unique leave request identifier |
| employee_id | Integer | Employee who submitted the request |
| leave_type_id | Integer | Type of leave requested |
| start_date | Date | Leave start date |
| end_date | Date | Leave end date |
| reason | String | Reason for leave |
| status | String | Current request status |
| created_at | DateTime | Request creation date and time |
| updated_at | DateTime | Last update date and time |

Primary Key:

`leave_request_id`

Foreign Keys:

- `employee_id` → `Employee.employee_id`
- `leave_type_id` → `LeaveType.leave_type_id`

Possible statuses:

- Pending
- Approved
- Rejected
- Cancelled

---

## 4. LeaveType

The `LeaveType` entity stores the different types of leave available in the organization.

Examples:

- Casual Leave
- Sick Leave
- Earned Leave

| Field | Data Type | Description |
|---|---|---|
| leave_type_id | Integer | Unique leave type identifier |
| name | String | Name of the leave type |
| description | String | Description of the leave type |
| annual_limit | Decimal | Maximum leave allocated |
| status | String | Leave type status |

Primary Key:

`leave_type_id`

---

## 5. LeaveBalance

The `LeaveBalance` entity stores the leave balance of each employee for each leave type.

| Field | Data Type | Description |
|---|---|---|
| balance_id | Integer | Unique balance identifier |
| employee_id | Integer | Employee associated with the balance |
| leave_type_id | Integer | Leave type associated with the balance |
| allocated_days | Decimal | Total allocated leave days |
| used_days | Decimal | Number of leave days already used |
| available_days | Decimal | Remaining available leave days |

Primary Key:

`balance_id`

Foreign Keys:

- `employee_id` → `Employee.employee_id`
- `leave_type_id` → `LeaveType.leave_type_id`

---

## 6. ApprovalHistory

The `ApprovalHistory` entity records actions performed on leave requests.

It provides a history of approvals and rejections for tracking and transparency.

| Field | Data Type | Description |
|---|---|---|
| history_id | Integer | Unique history identifier |
| leave_request_id | Integer | Leave request associated with the action |
| action_by | Integer | User who performed the action |
| action | String | Action such as Approved or Rejected |
| comment | String | Optional comment or rejection reason |
| action_at | DateTime | Date and time of the action |

Primary Key:

`history_id`

Foreign Key:

`leave_request_id` → `LeaveRequest.leave_request_id`

---

## 7. Entity Relationships

The main relationships are:

- One Employee can have many LeaveRequests.
- One Employee can have multiple LeaveBalance records.
- One LeaveType can be associated with many LeaveRequests.
- One LeaveType can be associated with many LeaveBalance records.
- One LeaveRequest can have multiple ApprovalHistory records.

Relationship overview:

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

---
![ER Diagram](./docs/images/er-diagram.png)
## 8. Data Flow

The basic database flow for a leave request is:

    Employee
       ↓
    Select Leave Type
       ↓
    Enter Leave Dates
       ↓
    Submit Leave Request
       ↓
    LeaveRequest
       ↓
    Status: Pending
       ↓
    Manager Approves / Rejects
       ↓
    ApprovalHistory
       ↓
    If Approved
       ↓
    LeaveBalance Updated

---

## 9. Data Integrity Rules

The database should maintain the following rules:

- Every employee must have a unique `employee_id`.
- Every leave type must have a unique `leave_type_id`.
- Every leave request must reference a valid employee.
- Every leave request must reference a valid leave type.
- Every leave balance must reference a valid employee and leave type.
- Every approval history record must reference a valid leave request.
- Required fields should not contain null values.
- Leave request statuses should contain only valid values.
- Foreign key relationships should maintain referential integrity.

---

## 10. Summary

The database is divided into five main entities to keep employee, leave, balance, and approval information organized.

`Employee` stores employee information.

`LeaveRequest` stores leave applications.

`LeaveType` stores available leave categories.

`LeaveBalance` stores employee leave balances.

`ApprovalHistory` stores approval and rejection activities.

This structure helps maintain data consistency, supports the leave workflow, and provides traceability of leave-related actions.
