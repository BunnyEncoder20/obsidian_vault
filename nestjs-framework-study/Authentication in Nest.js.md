# JWT Authentication and Authorization Flow in Nest.js
This architecture separates the concerns of Authentication (Is the token valid?), Authorization (Is the user allowed?), and Business Logic (What data is requested?).

## Phase 1: Authentication (Token Validation)
This phase uses Passport to verify the access token on every protected request.

| **Component**       | **File(s)**                                | **Role & Process**                                                                                                                                                                                                                                                                                                           |
| ------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Gatekeeper**      | `passport.guard.ts` (e.g., `JwtAuthGuard`) | Runs first. It extends `AuthGuard('jwt')`, delegating the entire validation process to the associated Strategy. If it fails, the request stops with a **401 Unauthorized** error.                                                                                                                                            |
| **The Interpreter** | `jwt.strategy.ts` (`JwtStrategy`)          | **1. Extraction:** Extracts the `access_token` from the HTTP-only cookie. **2. Validation:** Verifies the token's signature against the `JWT_SECRET` and checks for expiration. **3. Attachment:** If valid, it returns the decoded payload (`userId`, `email`, `role`) which Passport automatically attaches to `req.user`. |
## Phase 2: Authorization (Role Checking)
This phase runs after successful authentication to enforce access control based on the user's role.

| **Component**     | **File(s)**                       | **Role & Process**                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rule Setter**   | `roles.decorator.ts` (`@Roles()`) | Uses `SetMetadata` to attach an array of required roles (e.g., `['ADMIN']`) to the specific controller handler method.                                                                                                                                                                                                                                                                                                        |
| **Rule Enforcer** | `roles.guard.ts` (`RolesGuard`)   | Implements the `CanActivate` interface. **1. Get Rule:** Uses the `Reflector` to retrieve the roles metadata attached by `@Roles()`. **2. Get User:** Accesses the validated user data (`user.role`) from `req.user` (which was populated by the `JwtStrategy`). **3. Check Match:** Returns `true` only if the user's role matches one of the required roles. If it fails, the request stops with a **403 Forbidden** error. |

## Phase 3: Data Consumption (Controller Logic)
If both Guards pass, the request executes the business logic.

| **Component**      | **Role & Process**                                      |
| ------------------ | ------------------------------------------------------- |
| **Data Extractor** | Custom Decorator (e.g., `@User()`)                      |
| **The Endpoint**   | `passport-auth.controller.ts` (or any other controller) |
This entire sequence ensures that **every protected request is validated and authorized** before any business code runs. 