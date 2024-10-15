
# Atlantic League Analytics Platform API Documentation

## Overview
The Atlantic League Analytics Platform provides a robust API to access Trackman data and manage widgets. This API enables authorized users to interact with data and resources securely, facilitating the development and integration of widgets for analytical purposes.

## Base URL
```
https://api.atlanticleagueanalytics.com/v1
```

## Authentication
All API requests require authentication via AWS Cognito. Users must register and authenticate to receive a JSON Web Token (JWT), which should be included in the Authorization header of each request.

```
Authorization: Bearer <token>
```

## API Endpoints

### 1. Ballpark Management

#### Get Ballpark Details
```
GET /ballparks
```
**Query Parameters:**
```json
{
  "ballpark_name": "string",
  "city": "string",
  "state": "string"
}
```

### 2. Game Management

#### Get Game Details
```
GET /games
```
**Query Parameters:**
```json
{
  "ballpark_name": "string",
  "date": "date",
  "home_team_name": "string",
  "visiting_team_name": "string"
}
```

### 3. Pitch Management

#### Get Pitch Data
```
GET /pitches
```
**Query Parameters:**
```json
{
  "auto_pitch_type": "string",
  "balls": "int",
  "batter_id": "int",
  "catcher_id": "int",
  "date": "date",
  "date_range_end": "string",
  "date_range_start": "string",
  "game_id": "UUID",
  "inning": "int",
  "outs": "int"
}
```

### 4. Player Management

#### Get Player Details
```
GET /players
```
**Query Parameters:**
```json
{
  "player_batting_handedness": "string",
  "player_name": "string",
  "player_pitching_handedness": "string",
  "player_position": "string",
  "team_id": "UUID",
  "team_name": "string"
}
```

### 5. Team Management

#### Retrieve All Teams
```
GET /teams
```
**Query Parameters:**
```json
{
  "home_ballpark_name": "string",
  "league": "string",
  "team_name": "string"
}
```

### 6. User Management

#### Register a User
```
POST /users/register
```
**Request Body:**
```json
{
  "username": "string",
  "password": "string",
  "email": "string",
  "role": "string" 
}
```
**Response:**
- **201 Created:** User registered successfully.
- **400 Bad Request:** Validation error.

#### Login
```
POST /users/login
```
**Request Body:**
```json
{
  "username": "string",
  "password": "string"
}
```
**Response:**
- **200 OK:** Returns JWT token.
- **401 Unauthorized:** Invalid credentials.

#### Get User Details
```
GET /users/me
```
**Response:**
```json
{
  "userId": "string",
  "username": "string",
  "role": "string",
  "teams": ["string"]
}
```

## Error Handling
Common error responses include:
- **400 Bad Request:** The request was invalid or cannot be otherwise served.
- **401 Unauthorized:** Authentication failed or user does not have permissions.
- **403 Forbidden:** The request was valid, but the server is refusing action.
- **404 Not Found:** The requested resource could not be found.
- **500 Internal Server Error:** An error occurred on the server.

## Rate Limiting
To ensure fair use of the API, the following rate limits apply:
**100 requests per minute per user.**

## Database Structure Notes
The ER diagram outlines the relationships between key entities such as `PLAYER`, `PITCH`, `GAME`, and `TEAM`.

Each `PITCH` record is associated with a specific `PLAYER` (pitcher) and can also reference a batter. The `TEAM` entity includes members (players), and games are mapped between two teams.

## Conclusion
This API documentation provides a comprehensive overview of the Atlantic League Analytics Platform's API functionalities. Users are encouraged to review the authentication methods, endpoints, and response structures to effectively integrate with the platform.

For further inquiries or support, please contact our development team.
