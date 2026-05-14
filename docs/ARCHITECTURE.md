# System Architecture

## Overview

The Reverse Recruiting Application follows a modern three-tier architecture with clear separation of concerns.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                      Frontend (React)                        │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ Pages: Home, Profile, Browse, Messages, Dashboard   │   │
│  │ Components: Navbar, Cards, Forms, Chat              │   │
│  │ State: Redux Store with slices                      │   │
│  └──────────────────────────────────────────────────────┘   │
└──────────────────────────┬──────────────────────────────────┘
                           │
                     HTTP/WebSocket
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                    API Gateway (Express)                    │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ Routes, Middleware, Auth, Error Handling            │   │
│  └──────────────────────────────────────────────────────┘   │
└──────────────────────────┬──────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
   Services           Controllers        WebSocket
   - Auth             - Auth              - Messages
   - Profile          - Profile           - Status
   - Matching         - Messages          - Offers
   - Notifications    - Offers
        │                  │
        └──────────────────┼──────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                    Data Layer (MongoDB)                      │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ Collections: Users, Profiles, Messages, Offers      │   │
│  └──────────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

## Component Structure

### Frontend

#### Pages
- **Home**: Landing page with introduction
- **Profile**: User profile creation and editing
- **Browse**: Discover candidates/opportunities
- **Messages**: In-app messaging interface
- **Dashboard**: Analytics and management

#### Components
- **Navbar**: Navigation and user menu
- **ProfileCard**: Display profile information
- **MessagePanel**: Chat interface
- **OfferModal**: Create and manage offers
- **FilterBar**: Search and filter options

#### Store (Redux)
- **auth slice**: Authentication state
- **profile slice**: User profile state
- **messages slice**: Message state
- **ui slice**: UI/UX state

### Backend

#### Controllers
- **authController**: Login, register, token refresh
- **profileController**: CRUD operations for profiles
- **messageController**: Message operations
- **offerController**: Offer management

#### Models
- **User**: User credentials and basic info
- **Profile**: Detailed profile information
- **Message**: Chat messages
- **Offer**: Job offers and opportunities
- **Review**: Ratings and reviews

#### Services
- **AuthService**: JWT token management
- **ProfileService**: Profile matching and search
- **NotificationService**: Email and push notifications
- **AnalyticsService**: Track engagement metrics

#### Middleware
- **authMiddleware**: JWT verification
- **errorHandler**: Centralized error handling
- **logger**: Request logging
- **rateLimiter**: API rate limiting

## Data Flow

### Authentication Flow
1. User enters credentials
2. Frontend sends POST request to `/auth/login`
3. Backend validates credentials against MongoDB
4. Backend generates JWT token
5. Frontend stores token in localStorage
6. Token included in subsequent API requests

### Messaging Flow
1. User sends message via chat interface
2. Frontend emits WebSocket event
3. Backend receives and stores message in MongoDB
4. Backend broadcasts message to recipient via WebSocket
5. Both clients update UI in real-time

### Profile Discovery Flow
1. Recruiter accesses browse page
2. Frontend requests `/profiles?skills=javascript`
3. Backend queries MongoDB with filters
4. Results returned with pagination
5. Frontend displays profile cards
6. Clicks trigger `/profiles/:id` for detailed view

## Security Considerations

- **JWT Tokens**: Secure authentication with expiration
- **Password Hashing**: bcrypt for password security
- **HTTPS**: Required in production
- **CORS**: Configured for frontend domain
- **Rate Limiting**: Prevent brute force attacks
- **Input Validation**: All user inputs validated
- **Authorization**: Role-based access control (RBAC)

## Scalability

### Horizontal Scaling
- Load balancer (nginx/HAProxy)
- Multiple API server instances
- MongoDB replica set

### Database Optimization
- Indexing on frequently queried fields
- Denormalization where appropriate
- Caching layer (Redis)

### Performance
- CDN for static assets
- API response caching
- Image optimization
- Database query optimization

## Deployment

### Local Development
```bash
docker-compose up
```

### Production
- AWS ECS/EKS for container orchestration
- RDS for MongoDB
- CloudFront for CDN
- Route53 for DNS

## CI/CD Pipeline

1. Code push to GitHub
2. GitHub Actions workflow triggered
3. Lint and test frontend
4. Lint and test backend
5. Build Docker images
6. Push to Docker registry
7. Deploy to production
