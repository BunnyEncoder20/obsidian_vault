# Project Overview

This is a monorepo containing a backend and a frontend application.

## Backend

The backend is a NestJS application using TypeScript, Prisma for ORM, and PostgreSQL for the database. It handles user authentication with JWT.

### Key Technologies

- **Framework:** NestJS
- **Language:** TypeScript
- **Database:** PostgreSQL
- **ORM:** Prisma
- **Authentication:** JWT

### Building and Running

- **Install dependencies:** `npm install`
- **Run in development mode:** `npm run start:dev`
- **Run in production mode:** `npm run start:prod`
- **Run unit tests:** `npm run test`
- **Run end-to-end tests:** `npm run test:e2e`

### Development Conventions

- **Formatting:** `npm run format`
- **Linting:** `npm run lint`

## Frontend

The frontend is a Next.js application with TypeScript and Tailwind CSS.

### Key Technologies

- **Framework:** Next.js
- **Language:** TypeScript
- **Styling:** Tailwind CSS

### Building and Running

- **Install dependencies:** `npm install`
- **Run in development mode:** `npm run dev`
- **Build for production:** `npm run build`
- **Run in production mode:** `npm run start`
- **Linting:** `npm run lint`
- **Type checking:** `npm run typecheck`
