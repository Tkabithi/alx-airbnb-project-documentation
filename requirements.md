Airbnb Clone – Backend Requirements

This document specifies the technical and functional requirements for three core backend features:

User Authentication

Property Management

Booking System

Each section includes API endpoints, input/output specifications, validation rules, and performance criteria.

1. 🔐 User Authentication
Functional Requirements

Users can register, log in, and log out.

Passwords must be hashed before storage.

Issue JWT tokens for session management.

API Endpoints

POST /api/auth/register → Register new user

POST /api/auth/login → Authenticate and return JWT

POST /api/auth/logout → Invalidate token

Input / Output

Register
Input (JSON):

{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "Secret123!"
}


Output:

{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "message": "User registered successfully"
}


Login
Input:

{
  "email": "john@example.com",
  "password": "Secret123!"
}


Output:

{
  "access_token": "jwt_token_here",
  "refresh_token": "refresh_token_here"
}

Validation Rules

Email must be unique and valid format.

Password must have at least 8 characters, one uppercase letter, one number.

Performance Criteria

Authentication response < 500ms.

Support at least 100 concurrent login requests.

2. 🏠 Property Management
Functional Requirements

Hosts can add, update, and delete properties.

Guests can search and view properties.

API Endpoints

POST /api/properties/ → Add new property

GET /api/properties/ → List/search properties

GET /api/properties/{id}/ → Retrieve property details

PUT /api/properties/{id}/ → Update property (host only)

DELETE /api/properties/{id}/ → Delete property (host only)

Input / Output

Add Property
Input:

{
  "title": "Cozy Apartment",
  "description": "2 bed, near city center",
  "price_per_night": 50,
  "location": "Nairobi, Kenya",
  "host_id": 3
}


Output:

{
  "id": 10,
  "title": "Cozy Apartment",
  "price_per_night": 50,
  "status": "available"
}

Validation Rules

Title: max 100 characters.

Price: must be positive integer.

Location: required.

Performance Criteria

Search must return results in < 1 second.

System should scale to 10,000+ listings.

3. 📅 Booking System
Functional Requirements

Guests can book available properties.

Hosts must be notified of bookings.

Prevent double booking for the same date range.

API Endpoints

POST /api/bookings/ → Create a booking

GET /api/bookings/{id}/ → View booking details

PUT /api/bookings/{id}/cancel → Cancel booking

Input / Output

Create Booking
Input:

{
  "property_id": 10,
  "guest_id": 2,
  "check_in": "2025-10-01",
  "check_out": "2025-10-05"
}


Output:

{
  "id": 101,
  "property_id": 10,
  "guest_id": 2,
  "total_price": 200,
  "status": "confirmed"
}

Validation Rules

Check-in < check-out date.

Property must be available during requested dates.

Booking duration ≤ 30 days.

Performance Criteria

Booking confirmation in < 800ms.

Handle 1000+ simultaneous booking requests.