# Web Application README Template

Use this template for web applications (frontend, backend, full-stack).

## Structure

```markdown
# <app-name>

Brief description of what this web application does.

[![Build Status][build-badge]][build-url]
[![License][license-badge]][license-url]

## Features

- Feature 1
- Feature 2
- Feature 3

## Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Getting Started

### Prerequisites

- Node.js 18+
- npm 9+ or pnpm 8+

### Installation

1. Clone the repository
   ```bash
   git clone https://github.com/username/app-name.git
   cd app-name
   ```

2. Install dependencies
   ```bash
   npm install
   ```

3. Set up environment variables
   ```bash
   cp .env.example .env
   ```

4. Start the development server
   ```bash
   npm run dev
   ```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | Database connection string | Yes |
| `API_KEY` | External API key | No |
| `NODE_ENV` | Environment (development/production) | Yes |

> [!NOTE]
> See `.env.example` for all available variables.

## Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run start` | Start production server |
| `npm run lint` | Run linter |
| `npm run test` | Run tests |

## Project Structure

```
src/
├── components/     # Reusable UI components
├── pages/        # Route pages
├── api/          # API routes/endpoints
├── lib/          # Utilities and helpers
├── styles/       # Global styles
└── ...
```

## Deployment

### Vercel (Recommended)

```bash
npm i -g vercel
vercel
```

### Docker

```bash
docker build -t app-name .
docker run -p 3000:3000 app-name
```

### Manual

```bash
npm run build
npm run start
```

## API Documentation

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/users` | List users |
| `POST` | `/api/users` | Create user |
| `GET` | `/api/users/:id` | Get user |
| `PUT` | `/api/users/:id` | Update user |
| `DELETE` | `/api/users/:id` | Delete user |

### Authentication

This API uses JWT authentication.

```bash
# Get token
curl -X POST /api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "secret"}'
```

Use the token:

```bash
curl -H "Authorization: Bearer <token>" /api/users
```

## Tech Stack

- **Frontend:** React 18
- **Backend:** Node.js, Express
- **Database:** PostgreSQL
- **Authentication:** JWT
- **Styling:** Tailwind CSS

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT - see [LICENSE](LICENSE)
```

## Key Elements

1. **Features** - Key capabilities
2. **Quick Start** - 30-second start
3. **Prerequisites** - What's needed
4. **Installation** - Step-by-step
5. **Environment Variables** - Configuration
6. **Scripts** - npm/yarn commands
7. **Project Structure** - Overview
8. **Deployment** - Multiple options
9. **API Docs** - For backends
10. **Tech Stack** - Technologies used

## Variations by Type

### Frontend Only

- Remove API documentation section
- Focus on installation + build
- Add screenshot if applicable
- Deployment: Vercel, Netlify

### Backend/API

- Focus on API endpoints
- Authentication details
- Database schema if applicable
- Deployment: Railway, Render, Fly.io

### Full-Stack

- Include both frontend and backend sections
- Show frontend-backend communication
- Database configuration
