# Deployment Guide

## Prerequisites

- Docker and Docker Compose installed
- AWS account (for production)
- MongoDB Atlas account (optional, for cloud database)
- Node.js 16+ and npm

## Local Development

### Using Docker Compose

1. Clone the repository
```bash
git clone https://github.com/guaravsidharth-sid007/reverse-recruiting-app.git
cd reverse-recruiting-app
```

2. Set up environment variables
```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env
```

3. Update `.env` files with your configuration

4. Start the application
```bash
docker-compose up
```

5. Access the application
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- MongoDB: localhost:27017

### Manual Setup

1. Install dependencies
```bash
npm run install-all
```

2. Start MongoDB
```bash
mongod
```

3. Start backend server
```bash
cd backend
npm start
```

4. In a new terminal, start frontend
```bash
cd frontend
npm start
```

## Production Deployment

### AWS Deployment

#### Step 1: Create ECR Repository

```bash
# Create ECR repositories
aws ecr create-repository --repository-name reverse-recruiting-backend
aws ecr create-repository --repository-name reverse-recruiting-frontend
```

#### Step 2: Build and Push Docker Images

```bash
# Login to ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com

# Build backend
cd backend
docker build -t reverse-recruiting-backend:latest .
docker tag reverse-recruiting-backend:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/reverse-recruiting-backend:latest
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/reverse-recruiting-backend:latest

# Build frontend
cd ../frontend
docker build -t reverse-recruiting-frontend:latest .
docker tag reverse-recruiting-frontend:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/reverse-recruiting-frontend:latest
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/reverse-recruiting-frontend:latest
```

#### Step 3: Set up ECS Cluster

1. Create ECS cluster
2. Create task definitions for backend and frontend
3. Create services
4. Configure load balancer

#### Step 4: Set up RDS MongoDB

1. Create MongoDB instance on MongoDB Atlas or AWS DocumentDB
2. Update connection string in environment variables
3. Backup strategy and monitoring

#### Step 5: Configure Networking

1. Create VPC and subnets
2. Configure security groups
3. Set up NAT gateway for outbound traffic
4. Configure route tables

### Environment Variables (Production)

**Backend (.env)**
```
NODE_ENV=production
PORT=5000
MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/reverse-recruiting
JWT_SECRET=your-very-secure-secret-key-here
JWT_EXPIRE=7d
CORS_ORIGIN=https://yourdomain.com
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
```

**Frontend (.env)**
```
REACT_APP_API_URL=https://api.yourdomain.com
REACT_APP_WS_URL=wss://api.yourdomain.com
REACT_APP_ENV=production
```

### SSL/TLS Certificate

Use AWS Certificate Manager (ACM) for free SSL certificates:

1. Request certificate in ACM
2. Validate domain ownership
3. Use certificate in CloudFront and ALB

### Monitoring and Logging

1. CloudWatch for logs
2. CloudWatch alarms for metrics
3. X-Ray for distributed tracing
4. Application Performance Monitoring (APM)

### Backup Strategy

1. Daily automated database backups
2. Point-in-time recovery enabled
3. Backup retention: 30 days
4. Test backup restoration monthly

### CI/CD Pipeline

GitHub Actions workflow automatically deploys on push to main:

1. Run tests
2. Build Docker images
3. Push to ECR
4. Update ECS service
5. Monitor deployment health

### Domain and DNS

1. Register domain
2. Create Route53 hosted zone
3. Update nameservers
4. Create DNS records pointing to CloudFront (frontend) and ALB (backend)

### Performance Optimization

1. CloudFront CDN for frontend
2. API response caching
3. Database query optimization
4. Image compression
5. Code splitting and lazy loading

### Cost Management

1. Use AWS Cost Explorer
2. Set up billing alerts
3. Use reserved instances for predictable workloads
4. Implement auto-scaling policies
5. Regular cost optimization reviews

## Scaling

### Database Scaling

- MongoDB Atlas auto-scaling
- Read replicas for read-heavy operations
- Sharding for large collections

### Application Scaling

- ECS auto-scaling based on CPU/memory
- Load balancing with ALB
- Horizontal pod autoscaling

### Caching

- Redis for session storage
- API response caching
- Database query caching

## Troubleshooting

### Common Issues

**Database Connection Failed**
- Check MongoDB URI
- Verify security group rules
- Ensure database is running

**CORS Errors**
- Check CORS_ORIGIN environment variable
- Verify frontend domain is whitelisted

**High API Latency**
- Check database query performance
- Review CloudWatch metrics
- Consider caching strategies

**Container Crashes**
- Check ECS task logs
- Review memory allocation
- Check environment variables
