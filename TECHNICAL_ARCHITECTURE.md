# Technical Architecture

Complete technical architecture documentation for the Instagram UGC Platform.

---

## System Overview

### Architecture Pattern
**Monolithic Backend + Microservices-Ready Frontend**

- **Backend**: Single FastAPI application (can be split into services later)
- **Frontend**: React SPA with modular component architecture
- **Database**: PostgreSQL (single database, can be sharded later)
- **Storage**: AWS S3 for media files
- **Queue**: Redis + Celery for background jobs

---

## Technology Stack

### Frontend
- **Framework**: React 18+ with TypeScript
- **Styling**: Tailwind CSS
- **State Management**: Zustand + React Query
- **Routing**: React Router v6
- **Forms**: React Hook Form + Zod validation
- **HTTP Client**: Axios
- **Build Tool**: Vite
- **Testing**: Vitest + React Testing Library

### Backend
- **Framework**: Python 3.11+ with FastAPI
- **ORM**: SQLAlchemy 2.0
- **Database**: PostgreSQL 15+
- **Cache/Queue**: Redis 7+
- **Task Queue**: Celery
- **Authentication**: JWT tokens
- **API Docs**: OpenAPI/Swagger (auto-generated)
- **Testing**: Pytest

### Infrastructure
- **Hosting**: AWS
  - **Compute**: ECS (Fargate) or EC2
  - **Database**: RDS PostgreSQL
  - **Storage**: S3 + CloudFront CDN
  - **Cache**: ElastiCache Redis
  - **Load Balancer**: ALB
- **CI/CD**: GitHub Actions
- **Monitoring**: CloudWatch + Sentry
- **Logging**: CloudWatch Logs

### External Services
- **Instagram API**: Instagram Graph API (via Facebook)
- **Email**: SendGrid or AWS SES
- **Analytics**: PostHog or Mixpanel (optional)

---

## System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        Users (Browser)                       │
└───────────────────────┬─────────────────────────────────────┘
                        │ HTTPS
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                    CloudFront CDN                            │
│              (Static assets, cached content)                 │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────┐
│                  Application Load Balancer                   │
│                        (ALB)                                 │
└───────┬───────────────────────────────┬─────────────────────┘
        │                               │
        ▼                               ▼
┌──────────────────┐          ┌──────────────────┐
│   Frontend       │          │    Backend        │
│   (React SPA)    │          │   (FastAPI)       │
│                  │          │                   │
│  - Static files  │          │  - REST API       │
│  - Served via    │          │  - WebSocket      │
│    CloudFront    │          │  - Background     │
│                  │          │    jobs           │
└──────────────────┘          └────────┬──────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │                  │                  │
                    ▼                  ▼                  ▼
        ┌──────────────┐    ┌──────────────┐   ┌──────────────┐
        │  PostgreSQL  │    │    Redis     │   │  AWS S3       │
        │   (RDS)      │    │  (ElastiCache)│   │  (Storage)    │
        │              │    │              │   │              │
        │ - Users      │    │ - Cache      │   │ - Images     │
        │ - Content    │    │ - Sessions   │   │ - Videos     │
        │ - Permissions│    │ - Rate limit │   │ - Exports    │
        └──────────────┘    └──────────────┘   └──────────────┘
                    │
                    ▼
        ┌──────────────────────────┐
        │      Celery Workers      │
        │                          │
        │ - Content discovery      │
        │ - Export processing      │
        │ - Email sending          │
        │ - Permission reminders   │
        └──────────────────────────┘
                    │
                    ▼
        ┌──────────────────────────┐
        │   Instagram Graph API    │
        │   (External Service)     │
        └──────────────────────────┘
```

---

## Backend Architecture

### Application Structure

```
backend/
├── app/
│   ├── __init__.py
│   ├── main.py                 # FastAPI app entry point
│   ├── config.py               # Configuration
│   ├── dependencies.py         # Dependency injection
│   │
│   ├── api/
│   │   ├── __init__.py
│   │   ├── v1/
│   │   │   ├── __init__.py
│   │   │   ├── auth.py          # Authentication endpoints
│   │   │   ├── content.py       # Content endpoints
│   │   │   ├── collections.py   # Collection endpoints
│   │   │   ├── permissions.py   # Permission endpoints
│   │   │   ├── export.py        # Export endpoints
│   │   │   ├── dashboard.py     # Dashboard endpoints
│   │   │   └── tags.py          # Tag endpoints
│   │
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── content.py
│   │   ├── collection.py
│   │   ├── permission.py
│   │   └── export.py
│   │
│   ├── schemas/
│   │   ├── __init__.py
│   │   ├── user.py              # Pydantic schemas
│   │   ├── content.py
│   │   ├── collection.py
│   │   └── permission.py
│   │
│   ├── services/
│   │   ├── __init__.py
│   │   ├── instagram_service.py # Instagram API client
│   │   ├── content_service.py   # Content business logic
│   │   ├── permission_service.py
│   │   ├── export_service.py
│   │   └── email_service.py
│   │
│   ├── tasks/
│   │   ├── __init__.py
│   │   ├── content_discovery.py # Celery tasks
│   │   ├── export_processing.py
│   │   └── permission_reminders.py
│   │
│   ├── db/
│   │   ├── __init__.py
│   │   ├── base.py              # Database base
│   │   ├── session.py           # DB session management
│   │   └── migrations/          # Alembic migrations
│   │
│   └── utils/
│       ├── __init__.py
│       ├── auth.py              # JWT utilities
│       ├── cache.py             # Redis utilities
│       └── storage.py           # S3 utilities
│
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   ├── test_api/
│   ├── test_services/
│   └── test_models/
│
├── alembic.ini
├── requirements.txt
└── Dockerfile
```

### Key Components

**1. API Layer (FastAPI)**
- RESTful endpoints
- Request/response validation with Pydantic
- Automatic OpenAPI documentation
- Dependency injection for auth, DB sessions

**2. Service Layer**
- Business logic separation
- Instagram API integration
- Content processing
- Permission management
- Export generation

**3. Task Layer (Celery)**
- Async background jobs
- Content discovery (batch processing)
- Export processing
- Email sending
- Permission reminders

**4. Data Layer**
- SQLAlchemy ORM models
- Database migrations (Alembic)
- Query optimization
- Connection pooling

---

## Frontend Architecture

### Application Structure

```
frontend/
├── public/
│   ├── index.html
│   └── assets/
│
├── src/
│   ├── main.tsx                # App entry point
│   ├── App.tsx                 # Root component
│   ├── index.css               # Global styles
│   │
│   ├── components/
│   │   ├── common/            # Reusable components
│   │   │   ├── Button.tsx
│   │   │   ├── Card.tsx
│   │   │   ├── Modal.tsx
│   │   │   ├── Input.tsx
│   │   │   └── LoadingSpinner.tsx
│   │   │
│   │   ├── layout/            # Layout components
│   │   │   ├── Header.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── PageLayout.tsx
│   │   │
│   │   ├── content/           # Content components
│   │   │   ├── ContentGrid.tsx
│   │   │   ├── ContentCard.tsx
│   │   │   └── ContentDetailModal.tsx
│   │   │
│   │   ├── collections/       # Collection components
│   │   │   ├── CollectionCard.tsx
│   │   │   └── CollectionFormModal.tsx
│   │   │
│   │   ├── permissions/      # Permission components
│   │   │   ├── PermissionDashboard.tsx
│   │   │   └── PermissionRequestModal.tsx
│   │   │
│   │   └── export/           # Export components
│   │       ├── ExportModal.tsx
│   │       └── ExportProgress.tsx
│   │
│   ├── pages/                 # Page components
│   │   ├── DashboardPage.tsx
│   │   ├── ContentDiscoveryPage.tsx
│   │   ├── ContentLibraryPage.tsx
│   │   ├── CollectionsPage.tsx
│   │   ├── PermissionsPage.tsx
│   │   └── SettingsPage.tsx
│   │
│   ├── hooks/                 # Custom hooks
│   │   ├── useContent.ts
│   │   ├── useCollections.ts
│   │   ├── usePermissions.ts
│   │   ├── useDebounce.ts
│   │   └── useInfiniteScroll.ts
│   │
│   ├── services/              # API services
│   │   ├── api.ts             # Axios instance
│   │   ├── contentService.ts
│   │   ├── collectionService.ts
│   │   └── permissionService.ts
│   │
│   ├── store/                 # State management
│   │   ├── authStore.ts       # Zustand stores
│   │   ├── uiStore.ts
│   │   └── selectionStore.ts
│   │
│   ├── types/                 # TypeScript types
│   │   ├── content.ts
│   │   ├── collection.ts
│   │   └── permission.ts
│   │
│   └── utils/                 # Utilities
│       ├── constants.ts
│       ├── helpers.ts
│       └── validation.ts
│
├── vite.config.ts
├── tsconfig.json
├── tailwind.config.js
└── package.json
```

### State Management Strategy

**1. Server State (React Query)**
- API data (content, collections, permissions)
- Automatic caching and refetching
- Optimistic updates
- Background sync

**2. Client State (Zustand)**
- UI state (modals, sidebar, selections)
- User preferences
- Form state (local)

**3. URL State (React Router)**
- Current page/route
- Query parameters (filters, search)
- Deep linking support

---

## Data Flow

### Content Discovery Flow

```
User enters search query
    ↓
Frontend: SearchBar component
    ↓
useDebounce hook (300ms)
    ↓
contentService.search(query, filters)
    ↓
API: GET /api/v1/content/discover
    ↓
Backend: ContentService.search()
    ↓
InstagramService.searchInstagram()
    ↓
Instagram Graph API
    ↓
Process & cache results
    ↓
Return to frontend
    ↓
React Query caches response
    ↓
ContentGrid displays results
```

### Permission Request Flow

```
User clicks "Request Permission"
    ↓
PermissionRequestModal opens
    ↓
User fills form and submits
    ↓
permissionService.requestPermission(data)
    ↓
API: POST /api/v1/permissions
    ↓
Backend: PermissionService.create()
    ↓
Save to database
    ↓
Queue email task (Celery)
    ↓
Return success to frontend
    ↓
React Query invalidates cache
    ↓
UI updates (status → pending)
    ↓
Celery worker sends email
```

---

## Security Architecture

### Authentication Flow

```
1. User clicks "Login with Instagram"
2. Redirect to Instagram OAuth
3. User grants permissions
4. Instagram redirects with code
5. Backend exchanges code for access token
6. Backend creates/updates user
7. Backend generates JWT token
8. JWT stored in httpOnly cookie
9. Frontend receives user data
10. User authenticated
```

### Authorization

- **Role-Based Access Control (RBAC)**
  - Admin: Full access
  - Member: Content management
  - Viewer: Read-only

- **Resource-Level Permissions**
  - Users can only access their team's content
  - Collections are team-scoped
  - Permissions are user-scoped

### Data Protection

- **Encryption**
  - TLS 1.3 for data in transit
  - Encryption at rest (RDS, S3)
  - JWT tokens signed with RS256

- **Sensitive Data**
  - Instagram tokens encrypted in database
  - Passwords hashed with bcrypt
  - API keys in environment variables

---

## Scalability Considerations

### Horizontal Scaling

**Backend:**
- Stateless API servers
- Load balancer distributes traffic
- Database connection pooling
- Redis for session storage

**Frontend:**
- Static files via CDN
- Browser caching
- Code splitting
- Lazy loading

### Database Scaling

**Current:**
- Single PostgreSQL instance
- Read replicas for analytics queries
- Connection pooling

**Future:**
- Database sharding by team_id
- Read replicas per region
- Caching layer (Redis)

### API Rate Limiting

- **Per User**: 100 requests/minute
- **Per Endpoint**: Varies by endpoint
- **Instagram API**: Queue-based batch processing
- **Redis**: Track rate limits

---

## Monitoring & Observability

### Logging

- **Structured Logging**: JSON format
- **Log Levels**: DEBUG, INFO, WARNING, ERROR
- **Log Aggregation**: CloudWatch Logs
- **Log Retention**: 30 days

### Metrics

- **Application Metrics**:
  - Request rate
  - Response times
  - Error rates
  - Active users

- **Business Metrics**:
  - Content discovered
  - Permissions requested/approved
  - Exports generated

### Error Tracking

- **Sentry**: Error tracking and alerting
- **Error Types**: Exceptions, API errors, frontend errors
- **Alerting**: Slack/PagerDuty integration

### Health Checks

- **Endpoint**: `/health`
- **Checks**: Database, Redis, S3 connectivity
- **Load Balancer**: Health check every 30s

---

## Deployment Architecture

### Development

```
Local Development
├── Frontend: Vite dev server (localhost:5173)
├── Backend: FastAPI dev server (localhost:8000)
├── Database: Docker PostgreSQL
└── Redis: Docker Redis
```

### Staging

```
AWS Staging Environment
├── Frontend: S3 + CloudFront
├── Backend: ECS Fargate (1 task)
├── Database: RDS PostgreSQL (db.t3.micro)
├── Redis: ElastiCache (cache.t3.micro)
└── Celery: ECS Fargate (1 task)
```

### Production

```
AWS Production Environment
├── Frontend: S3 + CloudFront (CDN)
├── Backend: ECS Fargate (2+ tasks, auto-scaling)
├── Database: RDS PostgreSQL (Multi-AZ, backups)
├── Redis: ElastiCache (Multi-AZ)
├── Celery: ECS Fargate (2+ workers)
└── Monitoring: CloudWatch + Sentry
```

---

## CI/CD Pipeline

### GitHub Actions Workflow

```
1. Push to branch
   ↓
2. Run tests (unit + integration)
   ↓
3. Build Docker images
   ↓
4. Push to ECR
   ↓
5. Deploy to staging
   ↓
6. Run E2E tests
   ↓
7. (If main branch) Deploy to production
   ↓
8. Run smoke tests
```

### Deployment Strategy

- **Staging**: Automatic on merge to `develop`
- **Production**: Manual approval required
- **Rollback**: Previous image in ECR
- **Blue/Green**: Future consideration

---

## Performance Optimization

### Frontend

- **Code Splitting**: Route-based and component-based
- **Lazy Loading**: Images, components, routes
- **Caching**: Service worker, browser cache
- **Bundle Size**: Tree shaking, minification

### Backend

- **Database**: Indexes, query optimization
- **Caching**: Redis for API responses (1 hour)
- **Connection Pooling**: SQLAlchemy pool
- **Async Processing**: Celery for heavy tasks

### API

- **Rate Limiting**: Per user, per endpoint
- **Pagination**: Limit results (24-100 items)
- **Compression**: Gzip responses
- **CDN**: CloudFront for static assets

---

## Disaster Recovery

### Backup Strategy

- **Database**: Daily automated backups (RDS)
- **S3**: Versioning enabled
- **Retention**: 30 days backups
- **Testing**: Monthly restore tests

### High Availability

- **Database**: Multi-AZ deployment
- **Application**: Multiple instances behind ALB
- **Redis**: Multi-AZ ElastiCache
- **S3**: 99.99% durability

### Recovery Procedures

1. **Database Failure**: Restore from latest backup
2. **Application Failure**: Auto-scaling replaces instances
3. **Region Failure**: Multi-region deployment (future)

---

## Future Architecture Considerations

### Microservices Migration

When to split:
- Team size > 10 developers
- Traffic > 10k requests/minute
- Independent scaling needs

Potential services:
- Content Service
- Permission Service
- Export Service
- Notification Service

### Multi-Region

- **CDN**: CloudFront global distribution
- **Database**: Read replicas per region
- **Application**: Deploy to multiple regions
- **Data Sync**: Event-driven replication

### Caching Strategy

- **L1**: Browser cache (static assets)
- **L2**: CDN cache (API responses)
- **L3**: Redis cache (database queries)
- **L4**: Database (source of truth)

---

This architecture provides a solid foundation that can scale from MVP to enterprise while maintaining simplicity and developer productivity.

