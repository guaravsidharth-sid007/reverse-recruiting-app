# Reverse Recruiting Application

A modern web application that flips the traditional recruitment model. Instead of companies finding candidates, talented professionals post their profiles and companies discover them.

## Features

- **Candidate Profiles**: Professionals create comprehensive profiles showcasing their skills, experience, and career goals
- **Opportunity Discovery**: Companies browse and discover top talent based on skills and interests
- **Smart Matching**: AI-powered algorithm matches candidates with relevant opportunities
- **Direct Communication**: In-app messaging between candidates and recruiters
- **Profile Analytics**: Track profile views, interests, and offer details
- **Offer Management**: Candidates manage multiple offers and opportunities
- **Ratings & Reviews**: Both parties can rate and review experiences

## Tech Stack

### Frontend
- React 18+
- TypeScript
- Tailwind CSS
- Redux for state management
- Axios for API calls

### Backend
- Node.js + Express
- MongoDB
- JWT Authentication
- WebSockets for real-time messaging

### Deployment
- Docker
- AWS/GCP
- CI/CD with GitHub Actions

## Project Structure

```
reverse-recruiting-app/
├── frontend/                 # React frontend application
├── backend/                  # Express API server
├── docs/                     # Documentation
├── .github/workflows/        # GitHub Actions CI/CD
├── docker-compose.yml        # Docker compose configuration
└── package.json              # Root package.json
```

## Quick Start

### Prerequisites
- Node.js 16+
- MongoDB
- Docker (optional)

### Installation

1. Clone the repository
```bash
git clone https://github.com/guaravsidharth-sid007/reverse-recruiting-app.git
cd reverse-recruiting-app
```

2. Install dependencies
```bash
npm install
cd frontend && npm install
cd ../backend && npm install
cd ..
```

3. Set up environment variables
```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env
```

4. Start the application
```bash
npm start
```

Or with Docker:
```bash
docker-compose up
```

## API Documentation

See [docs/API.md](./docs/API.md) for detailed API endpoints.

## Architecture

See [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) for system architecture and design patterns.

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines.

## License

MIT License - see LICENSE file for details

## Support

For support, email support@reverserecruiting.dev or open an issue on GitHub.