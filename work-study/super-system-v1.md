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


## Improvements (Next Sprint)

### Most Critical Missing Components (Must Haves)

#### Security

1. Missing **Rate Limiting**:
	- Without rate limiting, any internal or external client can spam endpoints—even accidentally.
	- Add:
		- @nestjs/throttler
		- Different limits for public vs. authenticated routes.

2. Missing Global CSRF Protection (even for intranet apps)
	- But HttpOnly cookies **are still CSRF-vulnerable unless you add:**
		- SameSite=Strict
		- CSRF token via Double Submit or Synchronizer token
		- Optional: CSRF guard for browser routes

3. Missing: String Secret Management
	- we are using .env file but:
		- Rotation?
		- Storage in vault?
		- Encryption at rest?
		- Different secrets for dev/prod ?
	- Ideally integrate:
		- HashiCorp Vault/Doppler
		- Or at least SOPS/GPG-encrypted env files

4. Missing: Password Hashing Standardization
	- Ensure:
		- argon2 (recommended)
		- Proper defaults: parallelism, memory cost, time cost
		- Rehashing logic on login when config changes

5. Missing: Role + Permissions Granularity:
	- RBAC is there but LMS systems needs:
		- Permissions table 
		- Role-permission many-to-many
		- Fine-grained route + provider guards
	- We currently only have a role enum, which is limiting for future scale.

6. Missing: Central Request logging + Correlation IDs:
	- We need: 
		- RequestID middleware 
		- Structured logs (json)
		- logging to file + stdout
		- log aggregation (Loki / ElasticSearch)
	- Nest default logger is not enough

### Important Architectural Missing Pieces

#### Error Handling

1. Global Exception Filter
	- To Ensure
		- Cleanly formatted errors 
		- no interanal leaks 
		- mapped DB errors -> readable output
		- (Especially prisma's know errors)

2. Domain-Level Error Mapping
	- avoid letting Nest throw 500s for validation/logic mismatched

3. Caching Layer
	- A real LMS backend needs:
		- caching for heavy queries 
		- caching for users sessions or permissions 
		- caching of course metadata
	- We don't have:
		- Redis integration 
		- nest cache module with custom invalidation

4. File upload/Storage Module
	- LMS apps always need:
		- Pdfs
		- Videos
		- assignments 
		- user avatars
	- Your project has no file storage module.
	- even if you start local FS, late move to:
		- MinIO
		- S3
		- GCS
		- Azure Blob
	- **Important: design an abstract StorageService**

5. Background Jobs/Queues 
	- Critical for:
		- Sending emails
		- generating reports 
		- video processing
		- course completion analytics
	- We DO NOT have:
		- BullMQ
		- RabbitMQ (even lightweight queues)
		- Cron jobs

6. No Testing


## **Recommended Next Steps (Prioritized)**

### **Priority 1 – Security**

1. Add CSRF protection for cookie-based auth
    
2. Add rate limiting
    
3. Add Helmet + strict CORS
    
4. Add a Prisma error filter
    
5. Add proper secret rotation + argon2 hashing
    
### **Priority 2 – Architecture**

1. Implement audit logging
    
2. Implement caching (Redis)
    
3. Create a Storage module
    
4. Add BullMQ for async jobs
    
### **Priority 3 – Operations**

1. Add CI/CD
    
2. Add monitoring (Prometheus + Grafana)
    
3. Add a migration/deployment pipeline