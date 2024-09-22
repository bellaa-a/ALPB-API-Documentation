# Atlantic League Analytics Platform API Documentation

## Overview

The Atlantic League Analytics Platform provides a robust API to access Trackman data and manage widgets. This API enables authorized users to interact with data and resources securely, facilitating the development and integration of widgets for analytical purposes.

## Base URL

https://api.atlanticleagueanalytics.com/v1


## Authentication

### Amazon Cognito

All API requests require authentication via AWS Cognito. Users must register and authenticate to receive a JSON Web Token (JWT), which should be included in the `Authorization` header of each request.

Authorization: Bearer <token>


## API Endpoints

### 1. User Management

#### Register a User

- **POST** `/users/register`
  
**Request Body:**

{ "username": "string", "password": "string", "email": "string", "role": "string" // e.g., "coach", "player", "developer" }


**Response:**

- **201 Created**: User registered successfully.
- **400 Bad Request**: Validation error.

---

#### Login

- **POST** `/users/login`

**Request Body:**

{ "username": "string", "password": "string" }


**Response:**

- **200 OK**: Returns JWT token.
- **401 Unauthorized**: Invalid credentials.

---

#### Get User Details

- **GET** `/users/me`

**Response:**

- **200 OK**: Returns user details.

{ "userId": "string", "username": "string", "role": "string", "teams": ["string"] }


---

### 2. Team Management

#### Create Team

- **POST** `/teams`

**Request Body:**


{ "teamName": "string", "members": ["userId1", "userId2"] }


**Response:**

- **201 Created**: Team created successfully.
- **403 Forbidden**: User not authorized.

---

#### Invite User to Team

- **POST** `/teams/{teamId}/invite`

**Request Body:**

{ "userId": "string" }


**Response:**

- **200 OK**: User invited successfully.
- **404 Not Found**: Team not found.

---

#### Remove User from Team

- **DELETE** `/teams/{teamId}/members/{userId}`

**Response:**

- **204 No Content**: User removed successfully.
- **403 Forbidden**: User not authorized.

---

### 3. Widget Management

#### List Widgets

- **GET** `/widgets`

**Response:**

- **200 OK**: Returns a list of available widgets.

[ { "widgetId": "string", "name": "string", "description": "string", "createdBy": "string" } ]


---

#### Search Widgets

- **GET** `/widgets/search?query={query}`

**Response:**

- **200 OK**: Returns matching widgets.


[ { "widgetId": "string", "name": "string", "description": "string" } ]

---

### 4. Data Access

#### Get Trackman Data

- **GET** `/data/trackman`

**Response:**

- **200 OK**: Returns Trackman data relevant to the user’s access.

[ { "playerId": "string", "stats": { "pitchType": "string", "velocity": "number", "spinRate": "number" } } ]


---

### 5. Access Control

#### Set Access Level for Team

- **POST** `/teams/{teamId}/access`

**Request Body:**

{ "accessLevel": "string" // e.g., "public", "private" }


**Response:**

- **200 OK**: Access level updated successfully.

---

### Database Structure Notes

* The ER diagram outlines the relationships between key entities such as `PLAYER`, `PITCH`, `GAME`, and `TEAM`.
* Each `PITCH` involves three players: the pitcher, batter, and catcher. Ensure APIs return details for all involved players when querying pitch data.
* The `daily_game_number` in the `GAME` entity helps identify doubleheaders. API endpoints should allow filtering by this field.
* Some fields, such as `pitcher_set` and `tagged_pitch_type`, currently have undefined values. Future updates should focus on clarifying their data types and existence.
* All unspecified measurement data types in the PITCH diagram are assumed to be `double precision`.
* Further investigation is needed for fields like `dimensions` in the `BALLPARK` table.

---

## Error Handling

Common error responses include:

- **400 Bad Request**: The request was invalid or cannot be otherwise served.
- **401 Unauthorized**: Authentication failed or user does not have permissions.
- **403 Forbidden**: The request was valid, but the server is refusing action.
- **404 Not Found**: The requested resource could not be found.
- **500 Internal Server Error**: An error occurred on the server.

## Rate Limiting

To ensure fair use of the API, the following rate limits apply:

- **100 requests per minute** per user.

---

## Conclusion

This API documentation provides a comprehensive overview of the Atlantic League Analytics Platform's API functionalities. Users are encouraged to review the authentication methods, endpoints, and response structures to effectively integrate with the platform.

For further inquiries or support, please contact our development team.

