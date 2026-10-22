# EngageHUB — Prototype

A social media management project bringing account connections, content scheduling, publishing, and analytics into a unified dashboard.

**React · Vite · Tailwind CSS · Node.js · Express · MongoDB · OAuth**

## Product focus

Managing multiple social accounts involves more than composing a post: connections, authorization, media, scheduling, and provider-specific behavior all need to work together. EngageHUB organizes these concerns into a React frontend and an Express backend.

This repository is the prototype codebase. The related [EngageHUB repository](https://github.com/psodage/EngageHUB) is maintained separately; check each repository rather than assuming their behavior is identical.

## Architecture

`React SPA → Express API → MongoDB`

`Express API ↔ Social platform OAuth / publishing APIs`

| Path | Responsibility |
| --- | --- |
| `frontend/` | React pages, components, hooks, context, and API services |
| `frontend/src/config/api.js` | Centralized frontend API configuration |
| `backend/index.js` | Express application and authentication routes |
| `backend/start.js` | Environment loading and server bootstrap |
| `backend/routes/`, `controllers/` | API routing and request handlers |
| `backend/services/` | Authentication, social integrations, and supporting services |
| `backend/models/` | Database models |
| `backend/jobs/` | Background job code |
| `backend/public/uploads/` | Hosted media assets |
| `backend/scripts/` | OAuth and integration debugging utilities |
| `docs/` | Supporting documentation |

## Local setup

### Prerequisites

- Node.js 18+ as documented by the project; use a version compatible with the installed frontend and backend dependencies.
- npm and a MongoDB instance.
- OAuth application credentials for each social platform you want to connect.

### Install dependencies

~~~sh
git clone https://github.com/psodage/EngageHUB-Prototype.git
cd EngageHUB-Prototype
npm run install:all
~~~

### Configure the environment

Create `backend/.env` from `backend/.env.example` and `frontend/.env` from `frontend/.env.example`.

| Variable | Location | Purpose |
| --- | --- | --- |
| `MONGODB_URI` | Backend | MongoDB connection |
| `JWT_SECRET` | Backend | Authentication signing secret |
| `TOKEN_ENCRYPTION_KEY` | Backend | Token encryption configuration |
| `APP_BASE_URL` | Backend | Public API origin used by integrations |
| `CLIENT_BASE_URL` | Backend | Frontend origin for CORS |
| `VITE_API_URL` | Frontend | API origin, such as `http://localhost:4000` |

Configure the provider-specific values listed in the backend environment template. Register matching OAuth redirect URIs with each provider. For `VITE_API_URL`, use the API origin without appending `/api`.

### Run the applications

From the repository root:

~~~sh
npm run dev
~~~

Default local addresses:

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:4000`
- API health check: `http://localhost:4000/api/health`

## Available commands

| Command | Run from | Purpose |
| --- | --- | --- |
| `npm run install:all` | Root | Install root, frontend, and backend dependencies |
| `npm run dev` | Root | Run frontend and backend development servers |
| `npm run client` | Root | Run the frontend development server |
| `npm run server:dev` | Root | Run the backend development server |
| `npm run build` | Root | Build frontend assets |
| `npm run vercel-build` | Root | Install frontend dependencies and build assets |
| `npm start` | `backend/` | Start the API |
| `npm run debug:meta-oauth` | `backend/` | Inspect the Meta OAuth integration |
| `npm run debug:google-business-post` | `backend/` | Inspect the Google Business publishing integration |

## Deployment

Deploy the frontend and API as separate services.

**Frontend:** install dependencies, run `npm run build` from the root, and publish `frontend/dist`. Set `VITE_API_URL` to the deployed API origin. The repository includes SPA routing configuration.

**Backend:** use `backend/` as the service root, install dependencies with `npm install`, and start with `npm start`. Supply the backend environment values through the hosting service.

Set `APP_BASE_URL` to the API origin and `CLIENT_BASE_URL` to the frontend origin. Update registered OAuth redirect URIs to match. The root build command produces the frontend; it does not deploy the API. Plan persistent or external media storage if your hosting environment does not preserve local uploads.

## Verification and integration scope

Start by checking the health route and frontend-to-API connectivity. Then test account authorization, content preparation, and the publishing or scheduling workflow for each configured provider with an account you control. Debug utilities may contact external provider APIs; inspect them before running.

Provider support depends on OAuth permissions, provider approval, account type, and environment configuration. Verify each configured integration separately.

## Usage rights

All rights reserved, consistent with the existing project documentation.
