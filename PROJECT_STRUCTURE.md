# Project Structure Template

Starter project structure for developers to begin implementation.

---

## Repository Structure

```
instagram-ugc-platform/
├── README.md
├── prd.md
├── DECISIONS.md
├── WIREFRAMES.md
├── USER_FLOWS.md
├── API_SPECIFICATION.md
├── DATABASE_SCHEMA.md
├── COMPONENT_SPECIFICATIONS.md
├── USER_STORIES.md
├── TECHNICAL_ARCHITECTURE.md
│
├── backend/                    # Backend application
│   ├── app/
│   ├── tests/
│   ├── alembic.ini
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/                   # Frontend application
│   ├── src/
│   ├── public/
│   ├── package.json
│   ├── vite.config.ts
│   └── Dockerfile
│
├── infrastructure/             # Infrastructure as code
│   ├── terraform/              # Terraform configs
│   ├── docker-compose.yml      # Local development
│   └── scripts/                # Deployment scripts
│
└── docs/                       # Additional documentation
    ├── api/                    # API documentation
    ├── deployment/             # Deployment guides
    └── development/           # Development setup
```

---

## Quick Start Commands

### Backend Setup

```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# Edit .env with your configuration
alembic upgrade head
uvicorn app.main:app --reload
```

### Frontend Setup

```bash
cd frontend
npm install
cp .env.example .env
# Edit .env with your configuration
npm run dev
```

### Docker Setup (Full Stack)

```bash
docker-compose up -d
```

---

## Environment Variables

### Backend (.env)

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/instagram_ugc

# Redis
REDIS_URL=redis://localhost:6379

# Instagram API
INSTAGRAM_APP_ID=your_app_id
INSTAGRAM_APP_SECRET=your_app_secret
INSTAGRAM_REDIRECT_URI=http://localhost:8000/auth/callback

# JWT
JWT_SECRET_KEY=your_secret_key
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24

# AWS
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_S3_BUCKET=your_bucket_name
AWS_REGION=us-east-1

# Email
SENDGRID_API_KEY=your_sendgrid_key
EMAIL_FROM=noreply@yourdomain.com

# Environment
ENVIRONMENT=development
DEBUG=True
```

### Frontend (.env)

```env
VITE_API_URL=http://localhost:8000/api/v1
VITE_INSTAGRAM_APP_ID=your_app_id
VITE_ENVIRONMENT=development
```

---

## Development Workflow

### 1. Feature Development

```bash
# Create feature branch
git checkout -b feature/content-discovery

# Make changes
# ...

# Run tests
cd backend && pytest
cd frontend && npm test

# Commit
git add .
git commit -m "feat: add content discovery"

# Push and create PR
git push origin feature/content-discovery
```

### 2. Code Standards

**Backend:**
- Follow PEP 8
- Use type hints
- Write docstrings
- 80% test coverage minimum

**Frontend:**
- Use TypeScript
- Follow ESLint rules
- Use Prettier for formatting
- Write component tests

### 3. Git Workflow

- **main**: Production-ready code
- **develop**: Integration branch
- **feature/**: New features
- **fix/**: Bug fixes
- **docs/**: Documentation updates

---

## Testing Strategy

### Backend Tests

```bash
# Unit tests
pytest tests/unit/

# Integration tests
pytest tests/integration/

# All tests with coverage
pytest --cov=app tests/
```

### Frontend Tests

```bash
# Unit tests
npm test

# E2E tests (when added)
npm run test:e2e
```

---

## Deployment Checklist

### Pre-Deployment

- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Environment variables configured
- [ ] Database migrations tested
- [ ] Security scan completed

### Deployment Steps

1. Merge to main branch
2. CI/CD pipeline runs automatically
3. Build Docker images
4. Deploy to staging
5. Run smoke tests
6. Manual approval for production
7. Deploy to production
8. Monitor for errors

### Post-Deployment

- [ ] Verify health checks
- [ ] Check error logs
- [ ] Monitor metrics
- [ ] Test critical user flows
- [ ] Notify team

---

## Useful Commands

### Backend

```bash
# Run development server
uvicorn app.main:app --reload

# Run migrations
alembic upgrade head
alembic revision --autogenerate -m "description"

# Run tests
pytest
pytest -v  # Verbose
pytest tests/test_content.py  # Specific test

# Format code
black app/
isort app/

# Type checking
mypy app/
```

### Frontend

```bash
# Development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run tests
npm test
npm test -- --watch

# Lint
npm run lint
npm run lint -- --fix

# Type check
npm run type-check
```

### Docker

```bash
# Build images
docker-compose build

# Start services
docker-compose up -d

# View logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Stop services
docker-compose down

# Rebuild and restart
docker-compose up -d --build
```

---

## Next Steps for Developers

1. **Set up development environment**
   - Install dependencies
   - Configure environment variables
   - Set up database

2. **Review documentation**
   - Read PRD and wireframes
   - Understand API specification
   - Review component specifications

3. **Start with MVP features**
   - Authentication
   - Content discovery
   - Save to library
   - Basic collections

4. **Follow user stories**
   - Implement one story at a time
   - Write tests as you go
   - Get code review before merging

5. **Iterate and improve**
   - Gather user feedback
   - Refine features
   - Add enhancements

---

## Resources

- **API Documentation**: `/docs/api` (Swagger UI when running)
- **Component Library**: Storybook (when set up)
- **Design System**: Tailwind CSS docs
- **Instagram API**: Facebook Developer Docs
- **FastAPI Docs**: https://fastapi.tiangolo.com
- **React Docs**: https://react.dev

---

## Getting Help

- **Technical Questions**: Check documentation first
- **Architecture Questions**: Review TECHNICAL_ARCHITECTURE.md
- **API Questions**: Check API_SPECIFICATION.md
- **Component Questions**: Check COMPONENT_SPECIFICATIONS.md
- **Bug Reports**: Create GitHub issue
- **Feature Requests**: Discuss with product owner

---

This structure provides a solid foundation for development. Start with the MVP features and iterate based on user feedback.

