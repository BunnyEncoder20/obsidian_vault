# Plan: Frontend-Backend Integration Strategy

This document outlines a systematic, phased approach to connect the React frontend (`IETM_PGAD/client`) with the NestJS backend (`super-system-v1/backend`), analyzing existing API coverage, identifying gaps, and providing an implementation roadmap.

---

## Executive Summary

**Status:** Both frontend and backend are at approximately the same maturity level. The frontend has comprehensive API client methods (`api.ts`) and React Query hooks, while the backend has most core modules implemented. The primary task is to **align API contracts** and **bridge remaining gaps**.

**Key Findings:**

- ✅ **Auth Module**: Fully functional (login, logout, refresh, password change)
- ✅ **User/Admin Management**: Complete CRUD operations
- ✅ **Approvals Module**: Backend ready, frontend needs hooks
- ✅ **Access Control**: Backend complete, frontend has partial implementation
- ✅ **Systems/Content**: Backend complete, frontend partially integrated
- ✅ **Audit Logs**: Backend complete, frontend needs enhancement
- ⚠️ **Nominals Module**: Frontend expects it, backend uses generic User endpoints
- ❌ **Notifications**: Frontend ready, backend not implemented (deferred)
- ❌ **Stats/Dashboard**: Frontend ready, backend not implemented

---

## Phase 1: API Contract Verification & Alignment

### Objective

Verify that existing frontend API calls match backend endpoints exactly, document discrepancies, and create a mapping guide.

### Steps

1. **Create API Mapping Document**

   - Cross-reference all methods in `client/src/lib/api.ts` with backend controllers
   - Document URL path differences (e.g., `/auth/signin` vs `/auth/login`)
   - Identify HTTP method mismatches
   - Note authentication requirements per endpoint

2. **Align Authentication Endpoints**

   - **Frontend expects:** `POST /auth/login`, `POST /auth/logout`, `POST /auth/refresh`, `PATCH /auth/password`
   - **Backend provides:** `POST /auth/signin`, `POST /auth/signout`, `POST /auth/refresh`, `PATCH /auth/password` (needs implementation)
   - **Action:** Update frontend `api.ts` to use `/signin` and `/signout`, OR create backend aliases

3. **Align User Management Endpoints**

   - **Frontend:** Uses `/users` and `/admins` as separate namespaces
   - **Backend:** Uses `/user` (singular) for all user operations
   - **Action:** Backend should create `/users` (plural) controller or frontend should standardize on `/user`

4. **Align Access Control Endpoints**
   - **Frontend expects:** `/access/grant`, `/access/remove`, `/access/revoke`, `/access/restore`
   - **Backend provides:** `POST /access/user/:userId` (sync/replace all)
   - **Action:** Backend needs to implement granular access operations (grant adds, remove deletes specific)

### Deliverables

- [ ] `docs/API_MAPPING.md` - Complete endpoint comparison table
- [ ] `docs/API_BREAKING_CHANGES.md` - Required frontend updates
- [ ] Decision on URL conventions (plural vs singular, signin vs login)

---

## Phase 2: Backend Gaps - Critical Missing Endpoints

**Priority: HIGH** (Required for basic functionality)

### 2.1 Password Change (Self-Service)

**Current State:** Backend has password reset via admin, but not self-service change  
**Frontend:** `api.auth.changePassword(currentPassword, newPassword)`

**Implementation:**

```ts
// backend/src/auth/auth.controller.ts
@Patch('password')
@UseGuards(AccessTokenGuard)
async changePassword(
  @UserPayload() userContext: UserContext,
  @Body() dto: ChangePasswordDto,
  @HttpContext() requestContext: HttpLogContext,
) {
  return this.authService.changePassword(
    userContext.userId,
    dto.currentPassword,
    dto.newPassword,
    requestContext,
  );
}
```

### 2.2 Logout All Sessions

**Current State:** Missing  
**Frontend:** `api.auth.logoutAll()`

**Implementation:**

```ts
// backend/src/auth/auth.controller.ts
@Post('logout-all')
@UseGuards(AccessTokenGuard)
async logoutAll(
  @UserPayload() userContext: UserContext,
  @HttpContext() requestContext: HttpLogContext,
) {
  return this.authService.logoutAll(userContext.userId, requestContext);
}
```

### 2.3 Get Current User (Detailed)

**Current State:** `/user/whoami` exists but may not return full data  
**Frontend:** `api.auth.me()`

**Action:** Verify `whoami` returns all required fields or create `/auth/me` endpoint

### 2.4 Nominals Endpoints

**Current State:** Frontend expects `/nominals/*` but backend uses generic `/user/*`  
**Frontend:** Complete CRUD at `api.nominals.*`

**Options:**

- **Option A:** Create separate `NominalsController` that wraps `UserService` with different filters
- **Option B:** Update frontend to use `/users` with `assignedOnly=false` filter
- **Recommendation:** Option A for clarity (nominals = admin candidates, users = end users)

### Deliverables

- [ ] Implement `PATCH /auth/password`
- [ ] Implement `POST /auth/logout-all`
- [ ] Create `NominalsModule` or update frontend conventions
- [ ] Test all auth flows end-to-end

---

## Phase 3: Backend Gaps - Admin Dashboard Features

**Priority: MEDIUM-HIGH** (Required for admin functionality)

### 3.1 Statistics/Dashboard Endpoints

**Frontend Expects:**

- `GET /stats/overview`
- `GET /stats/superadmin`
- `GET /stats/unitadmin`

**Implementation Plan:**

```ts
// backend/src/stats/stats.module.ts
@Module({
  controllers: [StatsController],
  providers: [StatsService],
})
export class StatsModule {}

// backend/src/stats/stats.service.ts
async getOverview(): Promise<OverviewStats> {
  return {
    totalUsers: await this.prisma.user.count(),
    totalSystems: await this.prisma.system.count(),
    activeRequests: await this.prisma.approvalRequests.count({ where: { status: 'PENDING' } }),
    recentActivity: await this.getRecentActivity(),
  };
}

async getSuperAdminStats(): Promise<SuperAdminStats> {
  return {
    totalAdmins: await this.prisma.user.count({ where: { adminRole: { not: null } } }),
    totalUnits: await this.getUniqueUnits(),
    systemUsage: await this.getSystemUsageStats(),
    approvalsPending: await this.getPendingApprovals(),
  };
}

async getUnitAdminStats(unit: string): Promise<UnitAdminStats> {
  return {
    unitUsers: await this.prisma.user.count({ where: { unit } }),
    activeUsers: await this.getActiveUsersInUnit(unit),
    accessRequests: await this.getUnitAccessRequests(unit),
  };
}
```

### 3.2 Audit Log Statistics

**Frontend Expects:** `GET /access/audit/statistics`

**Implementation:**

```ts
// backend/src/logging/logging.controller.ts
@Get('statistics')
@Roles(...ALL_ADMINS)
async getStatistics() {
  return this.loggingService.getStatistics();
}

// backend/src/logging/logging.service.ts
async getStatistics() {
  return {
    totalLogs: await this.prisma.auditLog.count(),
    logsByType: await this.prisma.auditLog.groupBy({
      by: ['activityType'],
      _count: true,
    }),
    recentActivity: await this.getRecentActivity(),
    topUsers: await this.getMostActiveUsers(),
  };
}
```

### Deliverables

- [ ] Create `StatsModule` with overview, superadmin, unitadmin endpoints
- [ ] Add statistics endpoint to `LoggingController`
- [ ] Test admin dashboards with real data

---

## Phase 4: Access Control Enhancement

**Priority: HIGH** (Core security feature)

### Current State Analysis

**Backend:** `POST /access/user/:userId` does a **sync/replace** operation  
**Frontend:** Expects granular operations:

- `grant` - Add new access paths (append)
- `remove` - Delete specific access paths
- `revoke` - Soft delete for Officers (preserves default access)
- `restore` - Restore revoked access for Officers

### Implementation Strategy

```ts
// backend/src/access/access.controller.ts

@Post('grant')
@Roles(...ALL_ADMINS)
async grantAccess(
  @Body() dto: GrantAccessDto,
  @UserPayload() adminContext: UserContext,
  @HttpContext() requestContext: HttpLogContext,
) {
  return this.accessService.grantAccess(
    dto.userId,
    dto.accessPaths,
    adminContext,
    requestContext,
    dto.reason,
  );
}

@Post('remove')
@Roles(...ALL_ADMINS)
async removeAccess(
  @Body() dto: RemoveAccessDto,
  @UserPayload() adminContext: UserContext,
  @HttpContext() requestContext: HttpLogContext,
) {
  return this.accessService.removeAccess(
    dto.userId,
    dto.accessPaths,
    adminContext,
    requestContext,
    dto.reason,
  );
}

@Post('revoke')
@Roles(...ALL_ADMINS)
async revokeAccess(
  @Body() dto: RevokeAccessDto,
  @UserPayload() adminContext: UserContext,
  @HttpContext() requestContext: HttpLogContext,
) {
  // For Officers: Soft delete (mark as revoked, keep record)
  return this.accessService.revokeAccess(
    dto.userId,
    dto.accessPaths,
    adminContext,
    requestContext,
    dto.reason,
  );
}

@Post('restore')
@Roles(...ALL_ADMINS)
async restoreAccess(
  @Body() dto: RestoreAccessDto,
  @UserPayload() adminContext: UserContext,
  @HttpContext() requestContext: HttpLogContext,
) {
  // For Officers: Restore previously revoked access
  return this.accessService.restoreAccess(
    dto.userId,
    dto.accessPaths,
    adminContext,
    requestContext,
    dto.reason,
  );
}

@Post('check')
@UseGuards(AccessTokenGuard)
async checkAccess(@Body() dto: CheckAccessDto) {
  return this.accessService.checkAccess(dto.userId, dto.path);
}
```

### Database Schema Enhancement

```prisma
// Add soft delete support for Officer access revocation
model UserManualAccess {
  // ...existing fields...
  revokedAt     DateTime? @map("revoked_at")
  revokedBy     String?   @map("revoked_by")
  revokedByUser User?     @relation("RevokedAccess", fields: [revokedBy], references: [id])
}
```

### Deliverables

- [ ] Implement granular access control endpoints
- [ ] Add soft delete columns to access tables
- [ ] Update `AccessService` with new methods
- [ ] Create new DTOs (GrantAccessDto, RemoveAccessDto, etc.)
- [ ] Update frontend hooks to use new endpoints

---

## Phase 5: Systems & Content Integration

**Priority: MEDIUM** (Core IETM functionality)

### Current State

- **Backend:** `SystemsController` exists with tree, ToC, and content endpoints
- **Frontend:** Has comprehensive hooks in `use-systems.ts`
- **Gap:** Need to verify CRUD operations for SuperAdmin

### Verification & Enhancement

1. **Verify Existing Endpoints:**

   - [x] `GET /systems` - List all systems
   - [x] `GET /systems/tree` - Hierarchical structure
   - [x] `GET /systems/:id` - Single system
   - [x] `GET /systems/:id/toc` - Table of Contents
   - [ ] `POST /systems` - Create system (verify admin-only)
   - [ ] `PATCH /systems/:id` - Update system
   - [ ] `DELETE /systems/:id` - Delete system

2. **Content Serving:**

```ts
// backend/src/systems/systems.controller.ts
@Get('manuals/:manualId/toc')
@ApiOperation({ summary: 'Get Table of Contents for a manual' })
async getManualToc(@Param('manualId', ParseUUIDPipe) manualId: string) {
  return this.systemsService.getManualToc(manualId);
}

@Get('datamodules/:dmId/content')
@ApiOperation({ summary: 'Get DataModule HTML content' })
async getDataModuleContent(@Param('dmId', ParseUUIDPipe) dmId: string) {
  return this.systemsService.getDataModuleContent(dmId);
}
```

3. **Media Serving (Already Implemented):**
   - `GET /media/:manualId/:category/:filename` - With ACL check

### Deliverables

- [ ] Verify all CRUD operations for systems/subsystems/manuals
- [ ] Test content serving with frontend viewer
- [ ] Ensure ACL checks on all content endpoints

---

## Phase 6: Bookmarks & Annotations

**Priority: LOW-MEDIUM** (User features)

### Current State

**Frontend:** Complete hooks in `use-bookmarks.ts` and `use-annotations.ts`  
**Backend:** Likely missing - needs verification

### Implementation Plan

```ts
// backend/src/bookmarks/bookmarks.module.ts
@Module({
  controllers: [BookmarksController],
  providers: [BookmarksService],
  imports: [PrismaModule],
})
export class BookmarksModule {}

// Schema addition
model Bookmark {
  id           String   @id @default(uuid())
  userId       String   @map("user_id")
  resourceType String   @map("resource_type") // 'system', 'manual', 'datamodule'
  resourceId   String   @map("resource_id")
  title        String?
  note         String?
  createdAt    DateTime @default(now()) @map("created_at")

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([userId, resourceType, resourceId])
  @@index([userId])
  @@map("bookmarks")
}

model Annotation {
  id           String   @id @default(uuid())
  userId       String   @map("user_id")
  dataModuleId String   @map("data_module_id")
  content      String
  visibility   String   @default("private") // 'private', 'public'
  highlightText String? @map("highlight_text")
  position     Json?    // Store DOM position/range
  createdAt    DateTime @default(now()) @map("created_at")
  updatedAt    DateTime @updatedAt @map("updated_at")

  user       User       @relation(fields: [userId], references: [id], onDelete: Cascade)
  dataModule DataModule @relation(fields: [dataModuleId], references: [id], onDelete: Cascade)

  @@index([userId])
  @@index([dataModuleId])
  @@index([visibility])
  @@map("annotations")
}
```

### Deliverables

- [ ] Create `BookmarksModule` with CRUD operations
- [ ] Create `AnnotationsModule` with visibility controls
- [ ] Add database models
- [ ] Test with frontend IETM viewer

---

## Phase 7: Frontend Hook Updates

**Priority: HIGH** (Required for full integration)

### Tasks

1. **Create Missing Hooks:**

   - [ ] `use-approvals.ts` - For password reset requests
   - [ ] `use-stats.ts` - For dashboard statistics (already exists, verify coverage)
   - [ ] `use-notifications.ts` - For notification management (already exists)

2. **Update API Error Handling:**

```ts
// client/src/lib/api.ts - Enhance error responses
apiClient.interceptors.response.use(
  (response) => response,
  async (error: AxiosError<ApiResponse>) => {
    // Handle specific backend error codes
    if (error.response?.data?.error?.code === "FORBIDDEN") {
      // Show appropriate error message
    }

    // Handle token refresh errors
    if (error.response?.status === 401) {
      // Attempt refresh, redirect to login on failure
    }

    throw new ApiError(
      error.response?.data?.error?.message || "Unknown error",
      error.response?.data?.error?.code || "UNKNOWN_ERROR",
      error.response?.data?.error?.details,
      error.response?.status
    );
  }
);
```

3. **Standardize Response Types:**

```typescript
// client/src/lib/types.ts - Align with backend DTOs
export interface PaginatedResponse<T> {
  data: T[];
  meta: {
    page: number;
    pageSize: number;
    total: number;
    totalPages: number;
  };
}
```

### Deliverables

- [ ] Create/update all React Query hooks
- [ ] Align TypeScript types with backend DTOs
- [ ] Test all CRUD operations from UI
- [ ] Update error handling across all hooks

---

## Phase 8: Testing & Validation

**Priority: HIGH** (Quality assurance)

### Integration Testing Strategy

1. **Auth Flow Testing:**

   ```typescript
   // Test scenarios
   - Login with valid credentials
   - Login with invalid credentials
   - Token refresh on expiry
   - Logout and session cleanup
   - Password change flow
   - Logout all sessions
   ```

2. **RBAC Testing:**

   ```typescript
   // Test role-based access
   - SuperAdmin can access all endpoints
   - UnitAdmin can only access their unit's users
   - Regular users can only access assigned systems
   - Verify 403 responses for unauthorized access
   ```

3. **Access Control Testing:**

   ```typescript
   // Test granular permissions
   - Grant access to system/subsystem/manual
   - Remove specific access
   - Revoke Officer access (soft delete)
   - Restore revoked access
   - Verify access checks work correctly
   ```

4. **E2E User Journeys:**
   - SuperAdmin creates UnitAdmin
   - UnitAdmin creates users and assigns access
   - User logs in and views accessible systems
   - User bookmarks content and creates annotations
   - Admin approves password reset request

### Deliverables

- [ ] Create integration test suite (Playwright/Cypress)
- [ ] Write API integration tests (Supertest)
- [ ] Document test scenarios and expected results
- [ ] Create test data seed scripts

---

## Phase 9: Performance & Production Readiness

**Priority: MEDIUM** (Pre-deployment)

### Optimization Tasks

1. **API Response Optimization:**

   ```typescript
   // Implement pagination everywhere
   - Default pageSize: 10
   - Max pageSize: 100
   - Always return total count
   ```

2. **Database Query Optimizations:**

   ```typescript
   // Use Prisma select to fetch only required fields
   await prisma.user.findMany({
     select: {
       id: true,
       icNumber: true,
       firstName: true,
       lastName: true,
       rank: true,
       // Exclude password hash, refreshTokens, etc.
     },
   });
   ```

3. **Caching Strategy:**

   ```typescript
   // Backend caching for frequently accessed data
   - System tree (cache for 5 minutes)
   - User permissions (cache for 1 minute)
   - Static content metadata (cache for 1 hour)

   // Frontend caching with React Query
   - staleTime: 60000 (1 minute)
   - cacheTime: 300000 (5 minutes)
   - refetchOnWindowFocus: true for critical data
   ```

4. **Security Hardening:**
   - [ ] Enable CORS with specific origins
   - [ ] Implement rate limiting
   - [ ] Add request validation with class-validator
   - [ ] Sanitize all user inputs
   - [ ] Implement CSRF protection

### Deliverables

- [ ] Optimize all database queries
- [ ] Implement caching layer
- [ ] Add rate limiting middleware
- [ ] Complete security audit
- [ ] Performance benchmark report

---

## Implementation Timeline

### Week 1-2: Foundation

- [ ] Complete API mapping and alignment (Phase 1)
- [ ] Implement critical missing endpoints (Phase 2)
- [ ] Update frontend API client

### Week 3-4: Core Features

- [ ] Admin dashboard statistics (Phase 3)
- [ ] Access control enhancements (Phase 4)
- [ ] Systems/content verification (Phase 5)

### Week 5-6: User Features

- [ ] Bookmarks & Annotations (Phase 6)
- [ ] Frontend hook updates (Phase 7)
- [ ] Integration testing (Phase 8)

### Week 7-8: Polish & Launch

- [ ] Performance optimization (Phase 9)
- [ ] Security hardening
- [ ] Production deployment

---

## Critical Success Factors

1. **Consistent API Contracts:** Backend and frontend must agree on URL paths, HTTP methods, and response formats
2. **Type Safety:** Share TypeScript types between frontend and backend (consider monorepo or shared package)
3. **Error Handling:** Standardized error responses with proper HTTP status codes
4. **Authentication Flow:** Seamless token refresh without user disruption
5. **Access Control:** Properly enforced at both API and UI levels
6. **Testing Coverage:** Comprehensive tests for all critical paths

---

## Next Steps

### Immediate Actions:

- [ ] Review and approve this plan
- [ ] Set up development environment with both repos
- [ ] Create API mapping spreadsheet
- [ ] Prioritize endpoint implementations

### First Sprint Tasks:

- [ ] Fix auth endpoint naming mismatches
- [ ] Implement missing auth endpoints (password change, logout-all)
- [ ] Create Nominals module or update frontend
- [ ] Test complete auth flow end-to-end

### Ongoing:

- [ ] Daily sync between frontend/backend development
- [ ] Update this plan as requirements evolve
- [ ] Document all breaking changes
- [ ] Maintain API changelog

---

## Phase 10: Real-time Notifications (Future Enhancement)

**Priority: LOW** (Deferred - Nice-to-have)

### Current State

**Frontend:** Complete hooks in `use-socket.ts` and notification UI components  
**Backend:** Not implemented

### Implementation Plan

```typescript
// backend/src/notifications/notifications.gateway.ts
@WebSocketGateway({
  cors: { origin: process.env.FRONTEND_URL, credentials: true },
})
export class NotificationsGateway {
  @WebSocketServer() server: Server;

  @SubscribeMessage('subscribe')
  handleSubscribe(@ConnectedSocket() client: Socket) {
    const userId = client.handshake.auth.userId;
    client.join(`user:${userId}`);
  }

  async sendNotification(userId: string, notification: Notification) {
    this.server.to(`user:${userId}`).emit('notification', notification);
  }
}

// backend/src/notifications/notifications.service.ts
async createNotification(userId: string, data: CreateNotificationDto) {
  const notification = await this.prisma.notification.create({
    data: {
      userId,
      type: data.type,
      title: data.title,
      message: data.message,
      isRead: false,
    },
  });

  // Emit real-time event
  this.gateway.sendNotification(userId, notification);

  return notification;
}
```

### Database Schema

```prisma
model Notification {
  id        String   @id @default(uuid())
  userId    String   @map("user_id")
  type      String   // 'info', 'warning', 'error', 'success'
  title     String
  message   String
  isRead    Boolean  @default(false) @map("is_read")
  createdAt DateTime @default(now()) @map("created_at")

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@index([userId])
  @@index([isRead])
  @@map("notifications")
}
```

### API Endpoints

- `GET /notifications` - List user's notifications (paginated)
- `GET /notifications/unread-count` - Get count of unread notifications
- `PATCH /notifications/:id/read` - Mark single notification as read
- `PATCH /notifications/read-all` - Mark all notifications as read
- `DELETE /notifications/:id` - Delete a notification

### Deliverables

- [ ] Create `NotificationsModule` with Gateway
- [ ] Add Notification model to Prisma schema
- [ ] Implement notification CRUD endpoints
- [ ] Test real-time delivery with frontend
- [ ] Add notification triggers to key events (access changes, approval decisions, etc.)

---

**Ready to begin implementation? Start with Phase 1 - API Contract Verification.**
