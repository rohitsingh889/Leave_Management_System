# API Documentation

## 1. Overview

The Leave Management System uses REST APIs to allow the frontend to communicate with the backend.

The APIs handle creating, viewing, approving, and rejecting employee leave requests.

All protected endpoints require authentication and appropriate authorization.

---

## 2. API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/leave-requests` | Create a new leave request |
| GET | `/leave-requests` | Get leave requests |
| GET | `/leave-requests/{id}` | Get a specific leave request |
| PATCH | `/leave-requests/{id}/approve` | Approve a leave request |
| PATCH | `/leave-requests/{id}/reject` | Reject a leave request |

---

## 3. POST /leave-requests

Creates a new leave request for the authenticated employee.

### Request

POST `/leave-requests`

### Request Body

    {
      "leave_type_id": 2,
      "start_date": "2026-09-10",
      "end_date": "2026-09-12",
      "reason": "Personal work"
    }

### Validation

The system should verify:

- Employee is authenticated.
- Leave type is valid and active.
- Start date is not after end date.
- Required fields are provided.
- Employee has sufficient leave balance.

### Success Response

    {
      "id": 101,
      "status": "Pending",
      "message": "Leave request submitted successfully"
    }

### HTTP Status

`201 Created`

### Possible Errors

- `400 Bad Request`
- `401 Unauthorized`
- `409 Conflict`
- `422 Unprocessable Entity`

---

## 4. GET /leave-requests

Retrieves leave requests that the authenticated user is authorized to view.

### Request

GET `/leave-requests`

### Success Response

    {
      "leave_requests": [
        {
          "id": 101,
          "employee_id": 15,
          "leave_type": "Casual Leave",
          "start_date": "2026-09-10",
          "end_date": "2026-09-12",
          "reason": "Personal work",
          "status": "Pending"
        }
      ]
    }

### HTTP Status

`200 OK`

### Possible Errors

- `401 Unauthorized`
- `403 Forbidden`

---

## 5. GET /leave-requests/{id}

Retrieves details of a specific leave request.

### Request

GET `/leave-requests/101`

### Success Response

    {
      "id": 101,
      "employee_id": 15,
      "leave_type": "Casual Leave",
      "start_date": "2026-09-10",
      "end_date": "2026-09-12",
      "reason": "Personal work",
      "status": "Pending"
    }

### HTTP Status

`200 OK`

### Possible Errors

- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`

---

## 6. PATCH /leave-requests/{id}/approve

Approves a pending leave request.

Only an authorized manager can perform this action.

### Request

PATCH `/leave-requests/101/approve`

### Processing

The system should:

1. Verify that the manager is authenticated.
2. Verify that the manager has permission to approve the request.
3. Check that the leave request exists.
4. Check that the request is in `Pending` status.
5. Update the request status to `Approved`.
6. Update the applicable leave balance.
7. Record the approval in the approval history.

### Success Response

    {
      "id": 101,
      "status": "Approved",
      "message": "Leave request approved successfully"
    }

### HTTP Status

`200 OK`

### Possible Errors

- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `409 Conflict`

---

## 7. PATCH /leave-requests/{id}/reject

Rejects a pending leave request.

Only an authorized manager can perform this action.

### Request

PATCH `/leave-requests/101/reject`

### Request Body

    {
      "reason": "Leave cannot be approved for the requested period."
    }

### Processing

The system should:

1. Verify that the manager is authenticated.
2. Verify that the manager has permission to reject the request.
3. Check that the leave request exists.
4. Check that the request is in `Pending` status.
5. Update the request status to `Rejected`.
6. Record the rejection reason.
7. Record the rejection in the approval history.
8. Do not reduce the employee's leave balance.

### Success Response

    {
      "id": 101,
      "status": "Rejected",
      "message": "Leave request rejected successfully"
    }

### HTTP Status

`200 OK`

### Possible Errors

- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `409 Conflict`

---

## 8. Authentication and Authorization

The API should use authentication to identify the user making the request.

Authorization should be applied based on the user's role.

| Action | Employee | Manager |
|---|---|---|
| Create leave request | Yes | Yes, if allowed by policy |
| View own requests | Yes | Yes |
| View applicable employee requests | No | Yes |
| Approve request | No | Yes |
| Reject request | No | Yes |

---

## 9. Common HTTP Status Codes

| Status Code | Meaning |
|---|---|
| `200` | Request completed successfully |
| `201` | Resource created successfully |
| `400` | Bad request |
| `401` | Authentication required |
| `403` | User is not authorized |
| `404` | Resource not found |
| `409` | Request conflicts with current state |
| `422` | Validation failed |
| `500` | Internal server error |

---

## 10. API Flow

The general API flow is:

    Frontend
       ↓
    HTTP Request
       ↓
    REST API
       ↓
    Authentication
       ↓
    Authorization
       ↓
    Input Validation
       ↓
    Business Logic
       ↓
    Database
       ↓
    API Response
       ↓
    Frontend

The API acts as the communication layer between the frontend and backend while enforcing authentication, authorization, validation, and business rules.