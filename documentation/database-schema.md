# My Database Schema

This document outlines the schema for the MongoDB database used in the vehicle rental system.

## Collections

The database consists of the following collections:

- **cars**: Stores information about the vehicles available for rent.
- **users**: Stores user information, synced from Clerk.
- **rentals**: Stores information about active and past vehicle rentals.
- **audit_logs**: Stores a log of significant events for auditing purposes.

---

### `cars` Collection

Stores details for each car in the rental fleet.

**Fields:**

- `_id` (ObjectId): The unique identifier for the car.
- `name` (String): The name of the car model (e.g., "Lexus ES").
- `description` (String): A brief description of the car.
- `price` (Number): The rental price per day.
- `image` (String): The URL of an image of the car.
- `available` (Boolean): A flag indicating whether the car is currently available for rent.

**Example:**

```json
{
  "_id": "60d5f1b3e6e6a4b7f0a3b3a0",
  "name": "Lexus ES",
  "description": "A great business car with a luxurious and comfortable ride.",
  "price": 150,
  "image": "https://example.com/images/lexus-es.jpg",
  "available": true
}
```

---

### `users` Collection

Stores user data, which is synchronized from the Clerk authentication service.

**Fields:**

- `_id` (ObjectId): The unique identifier for the user in the database.
- `clerk_id` (String): The unique identifier for the user from Clerk. This is used to link the user's authentication data with their application data.
- `email` (String): The user's email address.
- `name` (String): The user's full name.
- `rental_history` (Array of ObjectId): An array of `_id`s from the `rentals` collection, representing the user's past rentals.

**Example:**

```json
{
  "_id": "60d5f1b3e6e6a4b7f0a3b3a1",
  "clerk_id": "user_123456789",
  "email": "user@example.com",
  "name": "John Doe",
  "rental_history": [
    "60d5f1b3e6e6a4b7f0a3b3a2"
  ]
}
```

---

### `rentals` Collection

Stores information about each rental transaction.

**Fields:**

- `_id` (ObjectId): The unique identifier for the rental.
- `user_id` (ObjectId): The `_id` of the user who rented the car, referencing the `users` collection.
- `car_id` (ObjectId): The `_id` of the car that was rented, referencing the `cars` collection.
- `start_date` (Date): The date and time when the rental period begins.
- `end_date` (Date): The date and time when the rental period ends.
- `total_price` (Number): The total cost of the rental.
- `status` (String): The current status of the rental (e.g., "active", "completed", "cancelled").

**Example:**

```json
{
  "_id": "60d5f1b3e6e6a4b7f0a3b3a2",
  "user_id": "60d5f1b3e6e6a4b7f0a3b3a1",
  "car_id": "60d5f1b3e6e6a4b7f0a3b3a0",
  "start_date": "2025-12-10T14:00:00Z",
  "end_date": "2025-12-15T14:00:00Z",
  "total_price": 750,
  "status": "active"
}
```

---

### `audit_logs` Collection

Stores a log of important actions performed within the system, such as user registrations, new rentals, and administrative changes.

**Fields:**

- `_id` (ObjectId): The unique identifier for the log entry.
- `timestamp` (Date): The date and time when the event occurred.
- `user_id` (ObjectId): The `_id` of the user who performed the action, referencing the `users` collection. Can be null if the action was performed by the system.
- `action` (String): A description of the action that was performed (e.g., "user_registered", "car_rented", "car_added").
- `details` (Object): An object containing additional details about the event.

**Example:**

```json
{
  "_id": "60d5f1b3e6e6a4b7f0a3b3a3",
  "timestamp": "2025-12-10T14:00:00Z",
  "user_id": "60d5f1b3e6e6a4b7f0a3b3a1",
  "action": "car_rented",
  "details": {
    "car_id": "60d5f1b3e6e6a4b7f0a3b3a0",
    "rental_id": "60d5f1b3e6e6a4b7f0a3b3a2"
  }
}
```
