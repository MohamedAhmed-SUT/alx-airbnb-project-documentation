# Airbnb Clone – Backend Requirement Specifications

## 1. Introduction

This document provides detailed technical and functional specifications for the key backend features of the Airbnb Clone project. It is intended to guide the development, testing, and implementation of the API, serving as a contract between the frontend and backend teams.

---

## 2. Feature: User Authentication

### 2.1 Functional Requirements

- Users must be able to register for a new account using their email and a password.
- Registered users must be able to log in and receive a secure authentication token (JWT).
- Sensitive routes must be protected and only accessible to authenticated users.

### 2.2 API Endpoint Specifications

#### A. User Registration

- **Method:** `POST`  
- **Endpoint:** `/api/v1/auth/register`  
- **Description:** Creates a new user account.

**Request Body:**

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "strongpassword123",
  "role": "guest"
}
Success Response (201 Created):

json
Copy
Edit
{
  "userId": "a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11",
  "message": "User registered successfully."
}
Error Responses:

400 Bad Request: If required fields are missing or invalid.

409 Conflict: If a user with the same email already exists.

B. User Login
Method: POST

Endpoint: /api/v1/auth/login

Description: Authenticates a user and returns a JWT.

Request Body:

json
Copy
Edit
{
  "email": "john.doe@example.com",
  "password": "strongpassword123"
}
Success Response (200 OK):

json
Copy
Edit
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
Error Response:

401 Unauthorized: If credentials are invalid.

2.3 Validation & Security
Validation:

email must be unique and follow a valid email format.

password must be at least 8 characters.

Security:

Passwords must be hashed using a secure algorithm (e.g., bcrypt).

JWT tokens must be securely signed and verified.

3. Feature: Property Management
3.1 Functional Requirements
Only authenticated users with the role of host can create a property.

Each property must include:

Name

Description

Location

Price per night

Each property must be linked to a host (user).

3.2 API Endpoint Specifications
A. Create a New Property
Method: POST

Endpoint: /api/v1/properties

Authentication: Required (JWT Bearer Token)

Description: Creates a new property listing.

Request Body:

json
Copy
Edit
{
  "name": "Cozy Beachfront Cottage",
  "description": "A beautiful cottage right on the beach.",
  "location": "Malibu, California",
  "pricePerNight": 300.00
}
Success Response (201 Created):

json
Copy
Edit
{
  "propertyId": "b0eebc99-9c0b-4ef8-bb6d-6bb9bd380b21",
  "hostId": "a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11",
  "name": "Cozy Beachfront Cottage",
  "location": "Malibu, California",
  "pricePerNight": 300.00,
  "createdAt": "2025-06-28T10:00:00Z"
}
Error Responses:

401 Unauthorized: If the user is not authenticated.

403 Forbidden: If the authenticated user's role is not host.

400 Bad Request: If required fields are missing or invalid.

3.3 Validation & Performance
name, location, and pricePerNight are required.

pricePerNight must be a positive number.

Use database indexing on hostId and location for efficient querying.

4. Feature: Booking System
4.1 Functional Requirements
Authenticated users must be able to create a booking for an available property.

The system must check availability (no date overlaps).

Total price must be calculated based on the number of nights.

Booking must include status (e.g., pending, confirmed).

4.2 API Endpoint Specifications
A. Create a Booking
Method: POST

Endpoint: /api/v1/bookings

Authentication: Required (JWT Bearer Token)

Description: Creates a new booking request.

Request Body:

json
Copy
Edit
{
  "propertyId": "b0eebc99-9c0b-4ef8-bb6d-6bb9bd380b21",
  "startDate": "2025-08-01",
  "endDate": "2025-08-05"
}
Success Response (201 Created):

json
Copy
Edit
{
  "bookingId": "c0eebc99-9c0b-4ef8-bb6d-6bb9bd380c31",
  "userId": "a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a12",
  "propertyId": "b0eebc99-9c0b-4ef8-bb6d-6bb9bd380b21",
  "totalPrice": 1200.00,
  "status": "pending",
  "startDate": "2025-08-01",
  "endDate": "2025-08-05"
}
Error Responses:

401 Unauthorized: If the user is not authenticated.

404 Not Found: If the property ID does not exist.

409 Conflict: If the property is already booked for the selected dates.

400 Bad Request: If endDate is before startDate.

4.3 Validation & Performance
Dates must not overlap existing bookings.

startDate must be before endDate.

Total price is calculated as (endDate - startDate) * pricePerNight.

Booking creation and availability checks should respond within 300ms for optimal UX.

5. General Notes
Use consistent status codes and error messages across endpoints.

Follow RESTful design principles.

Secure all endpoints with proper authentication and authorization.

Maintain clear documentation for frontend integration.

