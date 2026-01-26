# [PROJECT NAME] Development Guidelines

**Living Document** - Evoluciona con el proyecto  
**Last Updated**: [DATE]  
**Version**: [VERSION]

---

## Propósito

Este documento captura decisiones técnicas, patrones, y conocimiento específico del proyecto actual. A diferencia de la Constitution (principios universales) y templates (estructuras genéricas), este documento es **específico de tu proyecto** y debe actualizarse manualmente conforme el proyecto evoluciona.

---

## Active Technologies

**Extraer de plan.md de las features implementadas**

### Backend
- **Language**: [e.g., Python 3.11]
- **Framework**: [e.g., FastAPI 0.104]
- **Database**: [e.g., PostgreSQL 15]
- **ORM**: [e.g., SQLAlchemy 2.0]
- **Testing**: [e.g., pytest 7.4]

### Frontend (si aplica)
- **Language**: [e.g., TypeScript 5.2]
- **Framework**: [e.g., React 18]
- **State Management**: [e.g., Zustand]
- **Testing**: [e.g., Jest + React Testing Library]

### Infrastructure
- **CI/CD**: [e.g., GitHub Actions]
- **Deployment**: [e.g., Docker + AWS ECS]
- **Monitoring**: [e.g., DataDog]

---

## Project Structure

**Actual structure from implemented features**

```text
project-root/
├── src/
│   ├── models/              # Domain models
│   │   ├── user.py
│   │   └── product.py
│   ├── services/            # Business logic
│   │   ├── auth_service.py
│   │   └── product_service.py
│   ├── api/                 # API endpoints
│   │   ├── v1/
│   │   │   ├── auth.py
│   │   │   └── products.py
│   │   └── dependencies.py
│   ├── database/            # Database setup
│   │   ├── connection.py
│   │   └── migrations/
│   └── utils/               # Utilities
│       ├── validators.py
│       └── formatters.py
├── tests/
│   ├── unit/
│   ├── contract/
│   ├── integration/
│   └── e2e/
├── docs/
├── specs/
│   ├── 001-user-auth/
│   └── 002-product-catalog/
└── [configuration files]
```

---

## Commands

**Comandos específicos para este proyecto**

### Development

```bash
# Setup inicial
[specific setup commands]

# Activar entorno
[specific activation commands]

# Instalar dependencias
[specific install commands]

# Ejecutar servidor de desarrollo
[specific run commands]
```

### Testing

```bash
# Run all tests
[specific test command]

# Run specific test types
[unit test command]
[integration test command]
[e2e test command]

# Run with coverage
[coverage command]

# Run tests for specific feature
[feature-specific test command]
```

### Linting & Formatting

```bash
# Lint
[specific lint command]

# Format
[specific format command]

# Type check (si aplica)
[specific type check command]
```

### Database

```bash
# Run migrations
[migration command]

# Create new migration
[create migration command]

# Seed test data
[seed command]
```

### Build & Deploy

```bash
# Build
[build command]

# Run in production mode
[production run command]

# Deploy
[deploy command]
```

---

## Code Style

**Language-specific conventions for this project**

### Python (Example - Adjust based on your language)

```python
# Naming conventions
class UserModel:          # PascalCase for classes
    pass

def create_user():        # snake_case for functions
    pass

CONSTANT_VALUE = 100      # UPPER_CASE for constants

# Imports order
import standard_library   # 1. Standard library
import third_party        # 2. Third party
import local_module       # 3. Local modules

# Docstrings
def function_with_docstring(param: str) -> bool:
    """
    Brief description.
    
    Args:
        param: Description of parameter
    
    Returns:
        Description of return value
    
    Raises:
        ValueError: When parameter is invalid
    """
    pass
```

### Type Hints (if applicable)

```python
# Always use type hints in function signatures
def process_data(data: dict[str, Any]) -> list[Result]:
    pass

# Use Optional for nullable values
def find_user(user_id: int) -> Optional[User]:
    pass
```

---

## Architecture Patterns

**Patterns used in this project**

### Service Layer Pattern

```
API/Controllers → Services → Repositories → Database
                     ↓
                  Business Logic
```

- **Controllers**: Handle HTTP requests, validate input, return responses
- **Services**: Contain business logic, orchestrate operations
- **Repositories**: Database access layer
- **Models**: Domain entities

### Dependency Injection

```python
# Example pattern used in this project
def get_user_service(
    repository: UserRepository = Depends(get_user_repository)
) -> UserService:
    return UserService(repository)

@router.post("/users")
def create_user(
    data: UserCreate,
    service: UserService = Depends(get_user_service)
):
    return service.create_user(data)
```

---

## Common Patterns & Solutions

**Patterns and solutions specific to this project**

### Authentication

```python
# How authentication is implemented in this project
[Actual authentication code pattern]
```

### Error Handling

```python
# Standard error response format
{
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Human readable message",
        "details": {
            "field": "email",
            "reason": "Invalid format"
        }
    }
}
```

### Logging

```python
# Logging pattern used
logger.info(
    "User created",
    extra={
        "user_id": user.id,
        "email": user.email,
        "action": "user.created"
    }
)
```

### Database Transactions

```python
# Transaction pattern
[Actual transaction code pattern]
```

---

## Environment Variables

**Required environment variables for this project**

```bash
# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname

# Security
SECRET_KEY=your-secret-key
JWT_ALGORITHM=HS256
JWT_EXPIRATION=3600

# External Services
EMAIL_API_KEY=your-email-api-key
PAYMENT_API_KEY=your-payment-api-key

# Configuration
DEBUG=false
LOG_LEVEL=INFO
```

---

## Testing Patterns

**Testing patterns specific to this project**

### Fixture Organization

```python
# Common fixtures used across project
@pytest.fixture
def sample_user():
    """Fixture for user tests."""
    return User(email="test@example.com", name="Test")

@pytest.fixture
def authenticated_client():
    """Fixture for authenticated API client."""
    [Actual implementation]
```

### Mock Patterns

```python
# How we mock external services
@pytest.fixture
def mock_email_service():
    """Mock email service."""
    [Actual mock implementation]
```

---

## Database Conventions

**Database-specific conventions**

### Table Naming
- Use plural: `users`, `products`, `orders`
- Snake_case: `user_sessions`, `product_categories`

### Column Naming
- Use snake_case: `user_id`, `created_at`, `is_active`
- Booleans prefix with `is_`, `has_`, `can_`: `is_verified`, `has_avatar`
- Timestamps suffix with `_at`: `created_at`, `updated_at`, `deleted_at`

### Foreign Keys
- Format: `{table_singular}_id`: `user_id`, `product_id`, `category_id`

---

## API Conventions

**API-specific conventions for this project**

### Endpoint Naming
- Use plural resources: `/users`, `/products`
- Use kebab-case for multi-word: `/product-categories`
- Use HTTP methods correctly: GET (read), POST (create), PUT (full update), PATCH (partial), DELETE (remove)

### Versioning
- API version in URL: `/api/v1/users`
- Breaking changes require new version

### Pagination
```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total_pages": 5,
    "total_items": 100
  }
}
```

### Error Responses
```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  }
}
```

---

## Security Practices

**Security patterns used in this project**

### Password Hashing
- Algorithm: [e.g., bcrypt]
- Salt rounds: [e.g., 12]

### JWT Tokens
- Algorithm: [e.g., RS256]
- Expiration: [e.g., 1 hour for access, 7 days for refresh]

### Input Validation
- All user inputs validated using: [e.g., Pydantic models]
- SQL injection prevented by: [e.g., ORM parameterized queries]
- XSS prevented by: [e.g., auto-escaping in templates]

---

## Performance Considerations

**Performance patterns and optimizations**

### Database
- Indexes on: [list indexed columns]
- Connection pooling: [configuration]
- Query optimization: [patterns used]

### Caching
- Cache layer: [e.g., Redis]
- Cached resources: [list what's cached]
- TTL: [time-to-live for cached items]

### Rate Limiting
- Limits: [e.g., 100 requests per minute]
- Implementation: [how it's implemented]

---

## Recent Features & Changes

**Track last 3-5 major features implemented**

### Feature 001: User Authentication (2026-01-15)
- Implemented JWT-based auth
- Email verification flow
- Password reset functionality
- **New files**: `src/services/auth_service.py`, `src/api/v1/auth.py`
- **Dependencies added**: `pyjwt`, `passlib`

### Feature 002: Product Catalog (2026-01-20)
- Product CRUD operations
- Category management
- Search and filtering
- **New files**: `src/models/product.py`, `src/services/product_service.py`
- **Database changes**: Added `products` and `categories` tables

### Feature 003: [Next Feature]
- [Description]
- [Changes]

---

## Known Issues & Workarounds

**Document known issues and temporary solutions**

### Issue 1: [Description]
- **Problem**: [Detailed description]
- **Workaround**: [Temporary solution]
- **Issue tracker**: [Link to issue]

---

## Deployment Notes

**Deployment-specific information**

### Environment Setup
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Pre-deployment Checklist
- [ ] All tests passing
- [ ] Database migrations ready
- [ ] Environment variables configured
- [ ] Monitoring alerts configured

### Post-deployment Verification
- [ ] Health check endpoint responds
- [ ] Critical flows tested in production
- [ ] Monitoring dashboards showing metrics

---

## Useful Resources

**Project-specific resources**

### Documentation
- [API Documentation]: [URL]
- [Architecture Diagrams]: [URL]
- [Database Schema]: [URL]

### Tools
- [Monitoring Dashboard]: [URL]
- [CI/CD Pipeline]: [URL]
- [Error Tracking]: [URL]

### Team
- [Team Wiki]: [URL]
- [Slack Channel]: [Link]
- [Code Review Guidelines]: See `templates/code-review-checklist.md`

---

## Onboarding Checklist

**For new developers joining the project**

- [ ] Read Constitution (`memory/constitution.md`)
- [ ] Read this Development Guidelines document
- [ ] Setup development environment
- [ ] Run all tests successfully
- [ ] Review recent features in `specs/` folder
- [ ] Read Testing Standards (`templates/testing-standards.md`)
- [ ] Deploy to local/staging environment
- [ ] Complete sample task from backlog

---

## Maintenance

**How to keep this document updated**

### When to Update
- After implementing a new feature
- When adding new dependencies
- When changing architecture patterns
- When discovering new conventions
- At least monthly review

### What to Update
1. **Active Technologies**: Add/update versions
2. **Project Structure**: Reflect new directories/modules
3. **Commands**: Document new scripts/commands
4. **Recent Features**: Add latest feature, remove oldest
5. **Known Issues**: Add new, remove resolved

---

<!-- MANUAL ADDITIONS START -->

## Custom Project-Specific Sections

**Add any additional sections specific to your project here**

<!-- MANUAL ADDITIONS END -->

---

**Note**: This is a living document. Update it regularly to keep it useful. If information becomes outdated, it's worse than having no documentation at all.

**Template Version**: 1.0.0  
**Last Updated**: 2026-01-24

