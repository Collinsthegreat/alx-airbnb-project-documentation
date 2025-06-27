# Airbnb Clone Backend – Requirements Documentation

This document outlines the technical and functional requirements for key backend features of the Airbnb Clone system. Each feature specification includes API endpoints, input/output formats, validation rules, and performance expectation

# 1. User Authentication

# Functional Requirements
- Allow users (guests, hosts, and admins) to register and log in securely.
- Support password hashing and token-based authentication.

# API Endpoints

# POST `/api/v1/auth/register`
Registers a new user.

- Input (JSON):
  ```json
  {
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com",
    "password": "Secure123!"
  }

Validations:

Email must be unique and properly formatted.

Password must be at least 8 characters and hashed.

Response (201 Created):
json
Copy
Edit
{
  "message": "User registered successfully.",
  "user_id": "uuid"
}
POST /api/v1/auth/login
Authenticates user and returns a JWT token.

Input:
json
Copy
Edit
{
  "email": "john@example.com",
  "password": "Secure123!"
}
Response (200 OK):
json
Copy
Edit
{
  "token": "jwt-token",
  "role": "host"
}
 Performance
Password hashing should complete in <500ms.
Token generation within 200ms.

 2. Property Management
 Functional Requirements
Enable hosts to list, update, and delete properties.

Allow users to view and search available properties.

API Endpoints
POST /api/v1/properties
Creates a new property (host only).

Input:
json
Copy
Edit
{
  "name": "Ocean View Apartment",
  "description": "Near the beach, 2 beds, 1 bath",
  "location": "Cape Town",
  "price_per_night": 150.00
}
Validations:
All fields are required.
price_per_night must be positive.

GET /api/v1/properties
Fetches list of all properties (supports filters).

Query params:
location=Cape Town
min_price=100&max_price=200

Response (200 OK):
json
Copy
Edit

[
  {
    "property_id": "uuid",
    "name": "Ocean View Apartment",
    "location": "Cape Town",
    "price_per_night": 150.00
  }
]

PUT /api/v1/properties/:id
Updates a property (host only).

Fields: name, description, location, price_per_night
DELETE /api/v1/properties/:id
Deletes a property (host only).

 Performance
Search queries must respond in <1s for up to 10,000 properties.

3. Booking System
 Functional Requirements
Allow guests to book available properties.

Hosts can view upcoming bookings.

Bookings must be validated to avoid double-booking.

 API Endpoints
POST /api/v1/bookings
Creates a booking for a property.

Input:
json
Copy
Edit
{
  "property_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-05"
}
Validations:

Property must exist and be available.
Dates must be in the future and valid.
Total price calculated as (end_date - start_date) * price_per_night.

Response:
json
Copy
Edit
{
  "booking_id": "uuid",
  "status": "pending",
  "total_price": 600.00
}
GET /api/v1/bookings?user_id=...
Returns all bookings made by a guest.

GET /api/v1/host/bookings
Returns all bookings for a host's properties.

# Performance
Booking operations must complete under 1 second.

Prevent race conditions using DB-level locks or transactions.

 Notes
All endpoints are versioned under /api/v1/.
Authentication required for protected routes.
JWT-based authorization middleware handles access control.
All dates must be in YYYY-MM-DD format.

Author: Abuchi Collins
Project: ALX Airbnb Clone
