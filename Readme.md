**# Car Pooling App Backend - API Documentation**

**### Base URL**

`http://localhost:3000/api`

**### Authentication**

Most APIs require authentication using a JWT (JSON Web Token).

To obtain a token, use the `/users/login` endpoint.

Include the token in the Authorization header:

`Authorization: Bearer <token>`

**### API Endpoints**

**#### 1. User Management**

**##### 1.1 Register a New User**

**Endpoint:** POST `/users/register`

**Description:** Register a new user (driver or passenger).

**Request Body:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "role": "passenger",
  "phone": "1234567890"
}
```
**Response:**

```json
{
  "message": "User registered successfully"
}
```

**Error Responses:**

- `400 Bad Request`: If the request body is invalid or the email is already registered.

- `500 Internal Server Error`: If there is a server-side error.

**##### 1.2 Login User**

**Endpoint:** POST `/users/login`

**Description:** Authenticate a user and return a JWT token.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```
**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```
**Error Responses:**

- `400 Bad Request`: If the email or password is incorrect.

- `500 Internal Server Error`: If there is a server-side error.

**#### 2. Ride Management**

**##### 2.1 Create a New Ride**

**Endpoint:** POST `/rides`

**Description:** Create a new ride (only for drivers).

**Request Body:**

```json
{
  "driverId": "64f1c1a1e4b0f2b3c4d5e6f7",
  "pickupLocation": "New York",
  "dropLocation": "Boston",
  "seatsAvailable": 3,
  "fare": 50,
  "dateTime": "2023-10-15T10:00:00Z"
}
```

**Response:**
```json
{
  "message": "Ride created successfully",
  "ride": {
    "_id": "64f1c1a1e4b0f2b3c4d5e6f7",
    "driverId": "64f1c1a1e4b0f2b3c4d5e6f7",
    "pickupLocation": "New York",
    "dropLocation": "Boston",
    "seatsAvailable": 3,
    "fare": 50,
    "dateTime": "2023-10-15T10:00:00Z"
  }
}
```

**Error Responses:**

- `400 Bad Request`: If the request body is invalid or the driver does not exist.

- `500 Internal Server Error`: If there is a server-side error.

**##### 2.2 Get Available Rides**

**Endpoint:** GET `/rides`

**Description:** Get a list of all available rides.

**Response:**

```json

[
  {
    "_id": "64f1c1a1e4b0f2b3c4d5e6f7",
    "driverId": "64f1c1a1e4b0f2b3c4d5e6f7",
    "pickupLocation": "New York",
    "dropLocation": "Boston",
    "seatsAvailable": 3,
    "fare": 50,
    "dateTime": "2023-10-15T10:00:00Z"
  }
]
```

**Error Responses:**

- `500 Internal Server Error`: If there is a server-side error.

**##### 2.3 Book a Ride**

**Endpoint:** POST `/rides/book`

**Description:** Book a ride (only for passengers).

**Request Body:**

```json

{
  "rideId": "64f1c1a1e4b0f2b3c4d5e6f7",
  "passengerId": "64f1c1a1e4b0f2b3c4d5e6f8"
}
```

**Response:**

```json

{
  "message": "Ride booked successfully",
  "ride": {
    "_id": "64f1c1a1e4b0f2b3c4d5e6f7",
    "driverId": "64f1c1a1e4b0f2b3c4d5e6f7",
    "pickupLocation": "New York",
    "dropLocation": "Boston",
    "seatsAvailable": 2,
    "fare": 50,
    "dateTime": "2023-10-15T10:00:00Z",
    "passengers": [
      {
        "userId": "64f1c1a1e4b0f2b3c4d5e6f8",
        "name": "Jane Doe",
        "phone": "9876543210"
      }
    ]
  }
}

```

**Error Responses:**

- `400 Bad Request`: If the ride or passenger does not exist or no seats are available.

- `500 Internal Server Error`: If there is a server-side error.

**#### 3. Rating Management**

**##### 3.1 Add a Rating**

**Endpoint:** POST `/ratings`

**Description:** Add a rating and review for a user.

**Request Body:**
```json
{
  "ratedUserId": "64f1c1a1e4b0f2b3c4d5e6f7",
  "rating": 5,
  "review": "Great ride!"
}
```

**Response:**

```json

{
  "message": "Rating added successfully",
  "rating": {
    "_id": "64f1c1a1e4b0f2b3c4d5e6f9",
    "ratedUserId": "64f1c1a1e4b0f2b3c4d5e6f7",
    "rating": 5,
    "review": "Great ride!"
  }
}
```
**Error Responses:**

- `400 Bad Request`: If the request body is invalid or the rated user does not exist.

- `500 Internal Server Error`: If there is a server-side error.

**##### 3.2 Get Ratings for a User**

**Endpoint:** GET `/ratings/:userId`

**Description:** Get all ratings for a specific user.

**Response:**

```json

[
  {
    "_id": "64f1c1a1e4b0f2b3c4d5e6f9",
    "ratedUserId": "64f1c1a1e4b0f2b3c4d5e6f7",
    "rating": 5,
    "review": "Great ride!"
  }
]
```

**Error Responses:**

- `404 Not Found`: If the user does not exist.

- `500 Internal Server Error`: If there is a server-side error.

