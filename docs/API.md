# API Documentation

## Base URL
```
http://localhost:5000/api
```

## Authentication
All protected routes require a JWT token in the Authorization header:
```
Authorization: Bearer <token>
```

## Endpoints

### Authentication

#### Register
- **POST** `/auth/register`
- **Body**: `{ email, password, name, userType }` (userType: 'candidate' or 'recruiter')
- **Response**: `{ token, user }`

#### Login
- **POST** `/auth/login`
- **Body**: `{ email, password }`
- **Response**: `{ token, user }`

#### Get Current User
- **GET** `/auth/me`
- **Headers**: Authorization required
- **Response**: `{ user }`

### Profiles

#### Get All Profiles
- **GET** `/profiles`
- **Query**: `?skip=0&limit=10&skills=javascript,react`
- **Response**: `{ profiles: [], total, hasMore }`

#### Get Profile by ID
- **GET** `/profiles/:id`
- **Response**: `{ profile }`

#### Create Profile
- **POST** `/profiles`
- **Headers**: Authorization required
- **Body**: `{ title, bio, skills, experience, location, availability }`
- **Response**: `{ profile }`

#### Update Profile
- **PUT** `/profiles/:id`
- **Headers**: Authorization required
- **Body**: `{ title, bio, skills, experience, location, availability }`
- **Response**: `{ profile }`

#### Delete Profile
- **DELETE** `/profiles/:id`
- **Headers**: Authorization required
- **Response**: `{ success: true }`

### Messages

#### Get Conversations
- **GET** `/messages/conversations`
- **Headers**: Authorization required
- **Response**: `{ conversations: [] }`

#### Get Messages
- **GET** `/messages/:conversationId`
- **Headers**: Authorization required
- **Response**: `{ messages: [] }`

#### Send Message
- **POST** `/messages`
- **Headers**: Authorization required
- **Body**: `{ recipientId, content, conversationId }`
- **Response**: `{ message }`

### Offers

#### Get Offers
- **GET** `/offers`
- **Headers**: Authorization required
- **Query**: `?status=pending`
- **Response**: `{ offers: [] }`

#### Create Offer
- **POST** `/offers`
- **Headers**: Authorization required
- **Body**: `{ candidateId, title, description, salary, position }`
- **Response**: `{ offer }`

#### Update Offer Status
- **PATCH** `/offers/:id`
- **Headers**: Authorization required
- **Body**: `{ status }` (status: 'pending', 'accepted', 'rejected')
- **Response**: `{ offer }`

## Error Responses

### 400 Bad Request
```json
{
  "error": "Invalid request parameters"
}
```

### 401 Unauthorized
```json
{
  "error": "Authentication required"
}
```

### 403 Forbidden
```json
{
  "error": "Access denied"
}
```

### 404 Not Found
```json
{
  "error": "Resource not found"
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal server error"
}
```

## Rate Limiting

API requests are rate-limited to 100 requests per 15 minutes per IP address.

## WebSocket Events

### Connect
```javascript
const socket = io('http://localhost:5000');
socket.on('connect', () => {
  console.log('Connected');
});
```

### New Message
```javascript
socket.on('message:new', (message) => {
  console.log('New message:', message);
});
```

### User Status
```javascript
socket.emit('user:status', { status: 'online' });
socket.on('user:statusChanged', (user) => {
  console.log('User status changed:', user);
});
```
