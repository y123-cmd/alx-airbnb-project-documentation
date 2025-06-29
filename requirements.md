# Backend Requirements Specification

This document outlines the technical and functional requirements for three core backend features of the Airbnb Clone project: **User Authentication**, **Property Management**, and **Booking System**.

---

## 1. User Authentication

### Functional Requirements:
- Users can register as either a host or guest.
- Users can log in using email and password.
- JWT-based session authentication.
- OAuth integration (optional, e.g., Google).

### API Endpoints:
- `POST /api/auth/register` → Register new user.
- `POST /api/auth/login` → Authenticate user and return JWT.
- `GET /api/user/profile` → Fetch logged-in user's details.

### Input/Output:
- **Register Input**: `first_name`, `last_name`, `email`, `password`, `role`
- **Login Input**: `email`, `password`
- **Output**: JSON with success status and JWT token.

### Validation Rules:
- Email must be unique and valid.
- Password must be at least 6 characters.
- Role must be either `guest` or `host`.

### Performance Criteria:
- Token issued within < 500ms.
- Passwords securely hashed using bcrypt.
- Rate limiting on login endpoint to prevent brute-force attacks.

---

## 2. Property Management

### Functional Requirements:
- Hosts can create, update, delete, and view their property listings.
- Guests can view available properties with filters.

### API Endpoints:
- `POST /api/properties` → Create new property.
- `GET /api/properties` → List all properties.
- `PUT /api/properties/:id` → Update existing property.
- `DELETE /api/properties/:id` → Remove property listing.

### Input/Output:
- **Input**: `title`, `description`, `location`, `price`, `amenities`, `availability_dates`
- **Output**: JSON object with property details.

### Validation Rules:
- Title, description, and location are required.
- Price must be a positive number.
- Only authenticated hosts can create/update/delete properties.

### Performance Criteria:
- Searchable and paginated listings.
- Queries optimized with indexes on location and price.

---

## 3. Booking System

### Functional Requirements:
- Guests can book available properties.
- Hosts and guests can view and cancel bookings.
- Bookings are tied to availability dates.

### API Endpoints:
- `POST /api/bookings` → Create new booking.
- `GET /api/bookings` → Retrieve all bookings for a user.
- `DELETE /api/bookings/:id` → Cancel a booking.

### Input/Output:
- **Input**: `property_id`, `start_date`, `end_date`
- **Output**: Booking confirmation with total cost and status.

### Validation Rules:
- Ensure property is available for selected dates.
- `end_date` must be after `start_date`.
- Prevent overlapping bookings for the same property.

### Performance Criteria:
- Concurrent booking checks must respond in < 300ms.
- Booking queries indexed on `property_id`, `user_id`, and `status`.

---

## Notes:
- All endpoints must return proper HTTP status codes.
- Use RESTful API conventions.
- Logs and error handling to be included in middleware layer.
