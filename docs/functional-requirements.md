# Functional Documentation

## 1. Roles

### Employee

The Employee is the person who applies for and manages their own leave requests.

- Login to the system.
- View available leave balance.
- Apply for leave.
- Select leave type.
- Enter start and end dates.
- Provide a reason for leave.
- View submitted leave requests.
- Track leave request status.

### Manager

The Manager is responsible for reviewing employee leave requests.

- View employee leave requests.
- Review pending leave requests.
- Approve leave requests.
- Reject leave requests.
- Provide a reason when rejecting a request.
- View the status of processed requests.

### System / Administrator

The System or Administrator maintains the required data and enforces system rules.

- Maintain employee information.
- Maintain leave types.
- Maintain leave balances.
- Validate leave requests.
- Maintain approval history.
- Enforce authentication and authorization rules.
- Maintain data consistency.

---

## 2. Features

### Leave Application

Employees can apply for leave by selecting a leave type, providing the start and end dates, and entering a reason.

After successful validation, the leave request is created with a `Pending` status.

### Leave Request Tracking

Employees can view their submitted leave requests and track their current status.

Possible statuses include:

- Pending
- Approved
- Rejected
- Cancelled

### Leave Balance

Employees can view their available leave balance for different leave types.

The system checks the available balance before accepting a leave request.

### Leave Approval

Managers can review pending leave requests and approve valid requests.

After approval, the request status is changed to `Approved` and the approval action is recorded.

### Leave Rejection

Managers can reject pending leave requests when they cannot be approved.

A rejection reason can be recorded for future reference.

### Approval History

The system records approval and rejection actions for tracking and transparency.

The history includes:

- Leave request
- Action performed
- Person who performed the action
- Comment or reason
- Date and time

### Authentication and Authorization

Users must be authenticated to access protected features.

Role-based authorization ensures that users can only perform actions allowed for their role.

---

## 3. Business Rules

### Leave Application Rules

1. An employee must be authenticated before applying for leave.
2. A valid leave type must be selected.
3. The start date cannot be after the end date.
4. Required leave information must be provided.
5. The employee must have sufficient leave balance.
6. The employee must exist in the system.
7. The selected leave type must be valid and active.
8. Invalid leave requests must not be created.

### Leave Approval Rules

1. Only authorized managers can approve leave requests.
2. Only pending leave requests can be approved.
3. A request that has already been approved or rejected cannot be approved again.
4. Approval actions must be recorded in the approval history.
5. When a request is approved, the applicable leave balance is updated.

### Leave Rejection Rules

1. Only authorized managers can reject leave requests.
2. Only pending leave requests can be rejected.
3. A request that has already been approved or rejected cannot be rejected again.
4. Rejection actions must be recorded in the approval history.
5. Rejected requests should not reduce the employee's leave balance.

### Authorization Rules

1. Unauthenticated users cannot access protected leave features.
2. Employees cannot approve or reject leave requests.
3. Only authorized managers can approve or reject requests.
4. Users can only access information they are authorized to view.

### Data Rules

1. Every leave request must belong to a valid employee.
2. Every leave request must reference a valid leave type.
3. Approval and rejection actions must be traceable.
4. Invalid or non-existing leave request IDs must not be processed.
5. Leave balances and request statuses must remain consistent.