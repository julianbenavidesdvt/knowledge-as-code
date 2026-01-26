# Testing Standards & Best Practices

**Purpose**: Guía detallada de estándares de testing para el proyecto

**Last Updated**: 2026-01-24  
**Version**: 1.0.0

---

## Tabla de Contenidos

1. [Pirámide de Testing](#pirámide-de-testing)
2. [Unit Tests](#unit-tests)
3. [Contract Tests](#contract-tests)
4. [Integration Tests](#integration-tests)
5. [E2E Tests](#e2e-tests)
6. [Test Structure](#test-structure)
7. [Naming Conventions](#naming-conventions)
8. [Mocks y Fixtures](#mocks-y-fixtures)
9. [Coverage Guidelines](#coverage-guidelines)
10. [Common Patterns](#common-patterns)
11. [Anti-Patterns](#anti-patterns)

---

## Pirámide de Testing

### Distribution Target

```
        /\
       /E2E\        5-10%  - End-to-End (Critical flows only)
      /____\
     /Integ.\      15-25% - Integration (Multi-component)
    /________\
   /Contract\      10-15% - Contract (API boundaries)
  /__________\  
 /   Unit     \    50-70% - Unit (Functions/Methods)
/______________\
```

### Why This Distribution?

| Type | Speed | Cost | Maintenance | Feedback |
|------|-------|------|-------------|----------|
| **Unit** | ⚡ Fast | 💰 Low | ✅ Easy | 🎯 Precise |
| **Contract** | ⚡ Fast | 💰 Low | ✅ Easy | 🎯 API-specific |
| **Integration** | 🐢 Medium | 💰💰 Medium | ⚠️ Medium | 🎯 Component-level |
| **E2E** | 🐌 Slow | 💰💰💰 High | ❌ Hard | 🌫️ System-level |

**Principle**: More tests at the bottom (fast, cheap, reliable), fewer at the top (slow, expensive, flaky)

---

## Unit Tests

### Purpose

Test **individual functions/methods** in isolation, with all dependencies mocked.

### When to Write Unit Tests

✅ **ALWAYS write unit tests for**:
- Every public function/method
- Every logical branch (if/else/switch/case)
- Edge cases and boundary conditions
- Error handling paths
- Pure functions (no side effects)
- Business logic
- Data transformations
- Validation functions

❌ **DON'T write unit tests for**:
- Simple getters/setters (unless they have logic)
- Trivial one-liners
- Framework boilerplate
- Configuration files

### Unit Test Structure (AAA Pattern)

```python
def test_function_name_scenario_expected_result():
    """Brief description of what this test validates."""
    
    # Arrange - Setup test data and mocks
    input_data = create_test_data()
    mock_dependency = Mock()
    mock_dependency.method.return_value = expected_value
    
    # Act - Execute the function under test
    result = function_under_test(input_data, mock_dependency)
    
    # Assert - Verify the result
    assert result == expected_result
    mock_dependency.method.assert_called_once_with(input_data)
```

### Examples by Language

#### Python (pytest)

```python
# tests/unit/models/test_user.py

import pytest
from src.models.user import User
from src.exceptions import ValidationError


class TestUserModel:
    """Unit tests for User model."""
    
    def test_create_user_with_valid_data_succeeds(self):
        """User creation should succeed with valid email and password."""
        # Arrange
        email = "test@example.com"
        password = "SecurePass123!"
        
        # Act
        user = User(email=email, password=password)
        
        # Assert
        assert user.email == email
        assert user.password != password  # Should be hashed
        assert user.is_verified is False
    
    def test_create_user_with_invalid_email_raises_error(self):
        """User creation should raise ValidationError for invalid email."""
        # Arrange
        invalid_email = "not-an-email"
        password = "SecurePass123!"
        
        # Act & Assert
        with pytest.raises(ValidationError) as exc_info:
            User(email=invalid_email, password=password)
        
        assert "email" in str(exc_info.value).lower()
    
    @pytest.mark.parametrize("password", [
        "short",           # Too short
        "nouppercasenum",  # No uppercase
        "NOLOWERCASENUM",  # No lowercase
        "NoNumbersHere!",  # No numbers
    ])
    def test_create_user_with_weak_password_raises_error(self, password):
        """User creation should reject weak passwords."""
        # Arrange
        email = "test@example.com"
        
        # Act & Assert
        with pytest.raises(ValidationError) as exc_info:
            User(email=email, password=password)
        
        assert "password" in str(exc_info.value).lower()


# tests/unit/services/test_auth_service.py

from unittest.mock import Mock, patch
import pytest
from src.services.auth_service import AuthService
from src.models.user import User
from src.exceptions import AuthenticationError


class TestAuthService:
    """Unit tests for AuthService."""
    
    @pytest.fixture
    def mock_user_repository(self):
        """Fixture providing a mocked user repository."""
        return Mock()
    
    @pytest.fixture
    def auth_service(self, mock_user_repository):
        """Fixture providing an AuthService instance."""
        return AuthService(user_repository=mock_user_repository)
    
    def test_authenticate_with_valid_credentials_returns_token(
        self, auth_service, mock_user_repository
    ):
        """Authentication should return JWT token for valid credentials."""
        # Arrange
        email = "test@example.com"
        password = "ValidPass123!"
        user = User(email=email, password=password)
        mock_user_repository.find_by_email.return_value = user
        
        # Act
        token = auth_service.authenticate(email, password)
        
        # Assert
        assert token is not None
        assert len(token) > 0
        mock_user_repository.find_by_email.assert_called_once_with(email)
    
    def test_authenticate_with_invalid_password_raises_error(
        self, auth_service, mock_user_repository
    ):
        """Authentication should raise error for invalid password."""
        # Arrange
        email = "test@example.com"
        correct_password = "ValidPass123!"
        wrong_password = "WrongPass123!"
        user = User(email=email, password=correct_password)
        mock_user_repository.find_by_email.return_value = user
        
        # Act & Assert
        with pytest.raises(AuthenticationError):
            auth_service.authenticate(email, wrong_password)
    
    def test_authenticate_with_nonexistent_user_raises_error(
        self, auth_service, mock_user_repository
    ):
        """Authentication should raise error for non-existent user."""
        # Arrange
        email = "nonexistent@example.com"
        password = "AnyPass123!"
        mock_user_repository.find_by_email.return_value = None
        
        # Act & Assert
        with pytest.raises(AuthenticationError):
            auth_service.authenticate(email, password)
```

#### JavaScript/TypeScript (Jest)

```typescript
// tests/unit/models/user.test.ts

import { User } from '@/models/user';
import { ValidationError } from '@/exceptions';

describe('User Model', () => {
  describe('creation', () => {
    it('should create user with valid data', () => {
      // Arrange
      const email = 'test@example.com';
      const password = 'SecurePass123!';
      
      // Act
      const user = new User(email, password);
      
      // Assert
      expect(user.email).toBe(email);
      expect(user.password).not.toBe(password); // Should be hashed
      expect(user.isVerified).toBe(false);
    });
    
    it('should throw error for invalid email', () => {
      // Arrange
      const invalidEmail = 'not-an-email';
      const password = 'SecurePass123!';
      
      // Act & Assert
      expect(() => new User(invalidEmail, password))
        .toThrow(ValidationError);
    });
    
    it.each([
      ['short'],
      ['nouppercasenum'],
      ['NOLOWERCASENUM'],
      ['NoNumbersHere!'],
    ])('should reject weak password: %s', (password) => {
      // Arrange
      const email = 'test@example.com';
      
      // Act & Assert
      expect(() => new User(email, password))
        .toThrow(ValidationError);
    });
  });
});

// tests/unit/services/authService.test.ts

import { AuthService } from '@/services/authService';
import { User } from '@/models/user';
import { AuthenticationError } from '@/exceptions';

jest.mock('@/repositories/userRepository');

describe('AuthService', () => {
  let authService: AuthService;
  let mockUserRepository: any;
  
  beforeEach(() => {
    mockUserRepository = {
      findByEmail: jest.fn(),
    };
    authService = new AuthService(mockUserRepository);
  });
  
  describe('authenticate', () => {
    it('should return token for valid credentials', async () => {
      // Arrange
      const email = 'test@example.com';
      const password = 'ValidPass123!';
      const user = new User(email, password);
      mockUserRepository.findByEmail.mockResolvedValue(user);
      
      // Act
      const token = await authService.authenticate(email, password);
      
      // Assert
      expect(token).toBeTruthy();
      expect(mockUserRepository.findByEmail).toHaveBeenCalledWith(email);
    });
    
    it('should throw error for invalid password', async () => {
      // Arrange
      const email = 'test@example.com';
      const correctPassword = 'ValidPass123!';
      const wrongPassword = 'WrongPass123!';
      const user = new User(email, correctPassword);
      mockUserRepository.findByEmail.mockResolvedValue(user);
      
      // Act & Assert
      await expect(
        authService.authenticate(email, wrongPassword)
      ).rejects.toThrow(AuthenticationError);
    });
  });
});
```

### Coverage Target for Unit Tests

- **Models/Entities**: 90%
- **Services/Business Logic**: 85%
- **Utils/Helpers**: 95%

---

## Contract Tests

### Purpose

Test **API contracts** (request/response schemas) to ensure:
- Endpoints accept correct request format
- Endpoints return correct response format
- Breaking changes are detected early

### When to Write Contract Tests

✅ **ALWAYS write contract tests for**:
- Every REST/GraphQL endpoint
- Request validation (required fields, types, formats)
- Response schema (structure, types)
- Error responses (status codes, format)

### Contract Test Structure

```python
def test_endpoint_method_contract_validation():
    """Test that endpoint validates/returns correct schema."""
    
    # Arrange - Prepare request payload
    payload = create_valid_payload()
    
    # Act - Make API request
    response = client.post("/endpoint", json=payload)
    
    # Assert - Validate response schema
    assert response.status_code == expected_code
    assert response_matches_schema(response.json(), expected_schema)
```

### Examples

#### Python (pytest + FastAPI)

```python
# tests/contract/test_user_endpoints.py

import pytest
from fastapi.testclient import TestClient
from src.main import app

client = TestClient(app)


class TestUserEndpointContracts:
    """Contract tests for /users endpoints."""
    
    def test_post_users_requires_email_field(self):
        """POST /users should require email field."""
        # Arrange - Missing email
        payload = {"password": "ValidPass123!"}
        
        # Act
        response = client.post("/users", json=payload)
        
        # Assert
        assert response.status_code == 422  # Unprocessable Entity
        errors = response.json()["detail"]
        assert any(error["loc"][-1] == "email" for error in errors)
    
    def test_post_users_requires_password_field(self):
        """POST /users should require password field."""
        # Arrange - Missing password
        payload = {"email": "test@example.com"}
        
        # Act
        response = client.post("/users", json=payload)
        
        # Assert
        assert response.status_code == 422
        errors = response.json()["detail"]
        assert any(error["loc"][-1] == "password" for error in errors)
    
    def test_post_users_validates_email_format(self):
        """POST /users should validate email format."""
        # Arrange - Invalid email format
        payload = {
            "email": "not-an-email",
            "password": "ValidPass123!"
        }
        
        # Act
        response = client.post("/users", json=payload)
        
        # Assert
        assert response.status_code == 422
        errors = response.json()["detail"]
        email_error = next(e for e in errors if e["loc"][-1] == "email")
        assert "email" in email_error["msg"].lower()
    
    def test_post_users_returns_correct_schema_on_success(self):
        """POST /users should return user object with correct schema."""
        # Arrange
        payload = {
            "email": "newuser@example.com",
            "password": "ValidPass123!"
        }
        
        # Act
        response = client.post("/users", json=payload)
        
        # Assert
        assert response.status_code == 201
        data = response.json()
        
        # Validate response schema
        assert "id" in data
        assert "email" in data
        assert "created_at" in data
        assert "is_verified" in data
        
        # Validate types
        assert isinstance(data["id"], int)
        assert isinstance(data["email"], str)
        assert isinstance(data["created_at"], str)
        assert isinstance(data["is_verified"], bool)
        
        # Should NOT return password
        assert "password" not in data
    
    def test_post_users_returns_correct_error_schema(self):
        """POST /users should return consistent error format."""
        # Arrange - Invalid payload
        payload = {}
        
        # Act
        response = client.post("/users", json=payload)
        
        # Assert
        assert response.status_code == 422
        error_response = response.json()
        
        # Validate error schema
        assert "detail" in error_response
        assert isinstance(error_response["detail"], list)
        
        for error in error_response["detail"]:
            assert "loc" in error
            assert "msg" in error
            assert "type" in error
```

#### JavaScript/TypeScript (Jest + Express)

```typescript
// tests/contract/userEndpoints.test.ts

import request from 'supertest';
import { app } from '@/app';

describe('User Endpoints - Contract Tests', () => {
  describe('POST /users', () => {
    it('should require email field', async () => {
      // Arrange
      const payload = { password: 'ValidPass123!' };
      
      // Act
      const response = await request(app)
        .post('/users')
        .send(payload);
      
      // Assert
      expect(response.status).toBe(422);
      expect(response.body.errors).toEqual(
        expect.arrayContaining([
          expect.objectContaining({ field: 'email' })
        ])
      );
    });
    
    it('should return correct schema on success', async () => {
      // Arrange
      const payload = {
        email: 'newuser@example.com',
        password: 'ValidPass123!'
      };
      
      // Act
      const response = await request(app)
        .post('/users')
        .send(payload);
      
      // Assert
      expect(response.status).toBe(201);
      expect(response.body).toMatchObject({
        id: expect.any(Number),
        email: expect.any(String),
        createdAt: expect.any(String),
        isVerified: expect.any(Boolean),
      });
      expect(response.body).not.toHaveProperty('password');
    });
  });
});
```

### Coverage Target for Contract Tests

- **All public API endpoints**: 100%
- **Each endpoint method**: 100% (GET, POST, PUT, DELETE)

---

## Integration Tests

### Purpose

Test **interaction between multiple components** with real implementations (no mocks for internal components).

### When to Write Integration Tests

✅ **ALWAYS write integration tests for**:
- User journeys (complete flows)
- Database operations (with test DB)
- Authentication/Authorization flows
- Multi-step business processes
- Service-to-service communication

❌ **Mock only**:
- External services (APIs, payment gateways)
- Time-dependent functions
- File system (sometimes)

### Integration Test Structure

```python
def test_user_story_flow():
    """Test complete user journey from start to finish."""
    
    # Arrange - Setup test environment
    test_db = create_test_database()
    test_user = create_test_user()
    
    # Act - Execute complete flow
    step1_result = execute_step_1()
    step2_result = execute_step_2(step1_result)
    final_result = execute_step_3(step2_result)
    
    # Assert - Verify end state
    assert final_result.status == "success"
    assert database_state_is_correct()
    
    # Cleanup
    cleanup_test_data()
```

### Examples

#### Python (pytest + real database)

```python
# tests/integration/test_user_registration_flow.py

import pytest
from fastapi.testclient import TestClient
from src.main import app
from src.database import get_db, User
from tests.conftest import TestDatabase

client = TestClient(app)


@pytest.mark.integration
class TestUserRegistrationFlow:
    """Integration tests for complete user registration journey."""
    
    @pytest.fixture
    def test_db(self):
        """Provide a clean test database for each test."""
        db = TestDatabase()
        db.create_all()
        yield db
        db.drop_all()
    
    def test_us1_user_can_complete_registration_flow(self, test_db):
        """User Story 1: User can register, verify email, and login."""
        
        # Step 1: User registers
        registration_payload = {
            "email": "newuser@example.com",
            "password": "SecurePass123!",
            "name": "Test User"
        }
        register_response = client.post("/auth/register", json=registration_payload)
        
        assert register_response.status_code == 201
        user_data = register_response.json()
        user_id = user_data["id"]
        assert user_data["is_verified"] is False
        
        # Verify user exists in database
        db_user = test_db.query(User).filter_by(id=user_id).first()
        assert db_user is not None
        assert db_user.email == "newuser@example.com"
        assert db_user.is_verified is False
        
        # Step 2: User receives verification email (mocked)
        # In real app, email service would be mocked
        verification_token = db_user.verification_token
        
        # Step 3: User clicks verification link
        verify_response = client.get(f"/auth/verify/{verification_token}")
        
        assert verify_response.status_code == 200
        assert verify_response.json()["message"] == "Email verified successfully"
        
        # Verify user is now verified in database
        db_user = test_db.query(User).filter_by(id=user_id).first()
        assert db_user.is_verified is True
        
        # Step 4: User logs in
        login_payload = {
            "email": "newuser@example.com",
            "password": "SecurePass123!"
        }
        login_response = client.post("/auth/login", json=login_payload)
        
        assert login_response.status_code == 200
        assert "access_token" in login_response.json()
        assert "refresh_token" in login_response.json()
    
    def test_us1_unverified_user_cannot_login(self, test_db):
        """User cannot login without verifying email."""
        
        # Step 1: User registers
        registration_payload = {
            "email": "unverified@example.com",
            "password": "SecurePass123!"
        }
        register_response = client.post("/auth/register", json=registration_payload)
        assert register_response.status_code == 201
        
        # Step 2: User tries to login without verifying
        login_payload = {
            "email": "unverified@example.com",
            "password": "SecurePass123!"
        }
        login_response = client.post("/auth/login", json=login_payload)
        
        # Should fail
        assert login_response.status_code == 403
        assert "email not verified" in login_response.json()["detail"].lower()
```

#### JavaScript/TypeScript (Jest + real database)

```typescript
// tests/integration/userRegistrationFlow.test.ts

import request from 'supertest';
import { app } from '@/app';
import { db, User } from '@/database';

describe('User Registration Flow - Integration', () => {
  beforeEach(async () => {
    await db.sync({ force: true }); // Clean database
  });
  
  afterEach(async () => {
    await db.close();
  });
  
  it('should complete full registration and login flow', async () => {
    // Step 1: Register
    const registrationPayload = {
      email: 'newuser@example.com',
      password: 'SecurePass123!',
      name: 'Test User'
    };
    
    const registerResponse = await request(app)
      .post('/auth/register')
      .send(registrationPayload);
    
    expect(registerResponse.status).toBe(201);
    const userId = registerResponse.body.id;
    expect(registerResponse.body.isVerified).toBe(false);
    
    // Verify in database
    const dbUser = await User.findByPk(userId);
    expect(dbUser).toBeTruthy();
    expect(dbUser!.email).toBe('newuser@example.com');
    
    // Step 2: Verify email
    const verificationToken = dbUser!.verificationToken;
    const verifyResponse = await request(app)
      .get(`/auth/verify/${verificationToken}`);
    
    expect(verifyResponse.status).toBe(200);
    
    // Check database updated
    await dbUser!.reload();
    expect(dbUser!.isVerified).toBe(true);
    
    // Step 3: Login
    const loginPayload = {
      email: 'newuser@example.com',
      password: 'SecurePass123!'
    };
    
    const loginResponse = await request(app)
      .post('/auth/login')
      .send(loginPayload);
    
    expect(loginResponse.status).toBe(200);
    expect(loginResponse.body.accessToken).toBeTruthy();
  });
});
```

### Coverage Target for Integration Tests

- **Critical user journeys**: 100%
- **Multi-step business processes**: 100%

---

## E2E Tests

### Purpose

Test **complete system** from user perspective (UI → Backend → Database).

### When to Write E2E Tests

✅ **ONLY write E2E tests for**:
- Critical business flows (e.g., payment, checkout)
- High-value user journeys
- Smoke tests for production
- Regulatory/compliance requirements

❌ **DON'T write E2E tests for**:
- Edge cases (use unit tests)
- Every user journey (too slow/expensive)
- Internal implementation details

### Examples

#### Playwright (Web)

```typescript
// tests/e2e/criticalCheckoutFlow.spec.ts

import { test, expect } from '@playwright/test';

test.describe('Critical Checkout Flow', () => {
  test('user can complete purchase from product to payment', async ({ page }) => {
    // Step 1: Navigate to product
    await page.goto('/products/bestseller');
    await expect(page.locator('h1')).toContainText('Product Name');
    
    // Step 2: Add to cart
    await page.click('button:has-text("Add to Cart")');
    await expect(page.locator('.cart-count')).toHaveText('1');
    
    // Step 3: Go to cart
    await page.click('.cart-icon');
    await expect(page).toHaveURL(/.*cart/);
    
    // Step 4: Proceed to checkout
    await page.click('button:has-text("Checkout")');
    await expect(page).toHaveURL(/.*checkout/);
    
    // Step 5: Fill shipping info
    await page.fill('[name="address"]', '123 Test St');
    await page.fill('[name="city"]', 'Test City');
    await page.fill('[name="zipcode"]', '12345');
    await page.click('button:has-text("Continue to Payment")');
    
    // Step 6: Enter payment (test mode)
    await page.fill('[name="cardNumber"]', '4242424242424242');
    await page.fill('[name="expiry"]', '12/25');
    await page.fill('[name="cvc"]', '123');
    
    // Step 7: Complete purchase
    await page.click('button:has-text("Place Order")');
    
    // Step 8: Verify success
    await expect(page).toHaveURL(/.*order-confirmation/);
    await expect(page.locator('.success-message'))
      .toContainText('Order placed successfully');
  });
});
```

### Coverage Target for E2E Tests

- **Critical flows**: 100%
- **Overall E2E coverage**: 5-10% of total tests

---

## Test Structure

### Directory Structure

```
tests/
├── conftest.py              # Shared fixtures (pytest)
├── fixtures/                # Test data
│   ├── users.json
│   ├── products.json
│   └── sample_requests.json
├── mocks/                   # Reusable mocks
│   ├── external_api.py
│   ├── email_service.py
│   └── payment_gateway.py
├── unit/                    # Unit tests (50-70%)
│   ├── __init__.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── test_user.py
│   │   └── test_product.py
│   ├── services/
│   │   ├── __init__.py
│   │   ├── test_auth_service.py
│   │   └── test_product_service.py
│   └── utils/
│       ├── __init__.py
│       ├── test_validators.py
│       └── test_formatters.py
├── contract/                # Contract tests (10-15%)
│   ├── __init__.py
│   ├── test_user_endpoints.py
│   └── test_product_endpoints.py
├── integration/             # Integration tests (15-25%)
│   ├── __init__.py
│   ├── test_user_registration_flow.py
│   └── test_product_purchase_flow.py
└── e2e/                     # E2E tests (5-10%)
    ├── test_critical_checkout.py
    └── test_admin_workflow.py
```

---

## Naming Conventions

### Unit Tests

**Format**: `test_<function>_<scenario>_<expected_result>`

```python
# ✅ Good
def test_calculate_discount_with_valid_coupon_returns_reduced_price():
def test_validate_email_with_invalid_format_raises_error():
def test_get_user_when_not_found_returns_none():

# ❌ Bad
def test_discount():  # Too vague
def test_email_validation_1():  # Unclear what's being tested
def test_user_get():  # Unclear scenario and result
```

### Integration Tests

**Format**: `test_<user_story>_<flow>`

```python
# ✅ Good
def test_us1_user_completes_registration_successfully():
def test_us2_admin_creates_product_and_publishes():
def test_payment_flow_with_discount_code_applies_correctly():

# ❌ Bad
def test_registration():  # Missing user story context
def test_integration_1():  # Too vague
```

### Contract Tests

**Format**: `test_<endpoint>_<method>_<validation>`

```python
# ✅ Good
def test_post_users_requires_email_field():
def test_get_products_returns_correct_schema():
def test_put_users_id_validates_permission():

# ❌ Bad
def test_users_endpoint():  # Missing method and validation
def test_api_contract():  # Too vague
```

---

## Mocks y Fixtures

### When to Mock

✅ **Mock these**:
- External APIs (payment, email, SMS)
- Time-dependent functions (`datetime.now()`)
- Random functions (`random.randint()`)
- File system operations (sometimes)
- Database (only in unit tests)

❌ **DON'T mock these** (use real implementation):
- Your own business logic
- Internal services (in integration tests)
- Database (in integration tests - use test DB)
- Simple utility functions

### Fixture Examples

#### Python (pytest)

```python
# tests/conftest.py

import pytest
from unittest.mock import Mock
from src.database import Base, engine, SessionLocal
from src.models import User, Product


@pytest.fixture
def test_db():
    """Provide a clean test database."""
    Base.metadata.create_all(bind=engine)
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
        Base.metadata.drop_all(bind=engine)


@pytest.fixture
def sample_user():
    """Provide a sample user for testing."""
    return User(
        email="test@example.com",
        password="HashedPassword123!",
        name="Test User"
    )


@pytest.fixture
def mock_email_service():
    """Provide a mocked email service."""
    mock = Mock()
    mock.send_verification_email.return_value = True
    mock.send_password_reset.return_value = True
    return mock


@pytest.fixture
def mock_payment_gateway():
    """Provide a mocked payment gateway."""
    mock = Mock()
    mock.charge.return_value = {
        "success": True,
        "transaction_id": "tx_12345"
    }
    return mock
```

#### JavaScript (Jest)

```typescript
// tests/setup.ts

import { db } from '@/database';

export async function setupTestDb() {
  await db.sync({ force: true });
}

export async function teardownTestDb() {
  await db.close();
}

export const mockEmailService = {
  sendVerificationEmail: jest.fn().mockResolvedValue(true),
  sendPasswordReset: jest.fn().mockResolvedValue(true),
};

export const mockPaymentGateway = {
  charge: jest.fn().mockResolvedValue({
    success: true,
    transactionId: 'tx_12345',
  }),
};

export const sampleUser = {
  email: 'test@example.com',
  password: 'HashedPassword123!',
  name: 'Test User',
};
```

---

## Coverage Guidelines

### Minimum Targets

| Component Type | Target | Priority |
|----------------|--------|----------|
| **Overall Project** | **80%** | **REQUIRED** |
| Models/Entities | 90% | High |
| Services/Business Logic | 85% | High |
| Utils/Helpers | 95% | High |
| Controllers/Handlers | 75% | Medium |
| Config/Setup | 50% | Low |

### How to Measure

```bash
# Python (pytest)
pytest --cov=src --cov-report=html --cov-report=term
pytest --cov=src --cov-fail-under=80

# JavaScript (Jest)
jest --coverage
jest --coverage --coverageThreshold='{"global":{"branches":80,"functions":80,"lines":80,"statements":80}}'
```

### What to Do if Coverage is Low

1. **Identify uncovered lines**:
   ```bash
   pytest --cov=src --cov-report=term-missing
   ```

2. **Prioritize**:
   - Critical business logic first
   - Public APIs second
   - Internal utilities third

3. **Write missing tests**:
   - Unit tests for uncovered functions
   - Integration tests for uncovered flows

4. **Don't cheat**:
   - ❌ Don't write meaningless tests just for coverage
   - ❌ Don't exclude files without justification
   - ✅ Write meaningful tests that would catch bugs

---

## Common Patterns

### Testing Async Functions

```python
# Python (pytest-asyncio)
import pytest

@pytest.mark.asyncio
async def test_async_function_returns_expected_result():
    # Arrange
    data = "test"
    
    # Act
    result = await async_function(data)
    
    # Assert
    assert result == expected_result
```

```typescript
// TypeScript (Jest)
it('should handle async operation', async () => {
  // Arrange
  const data = 'test';
  
  // Act
  const result = await asyncFunction(data);
  
  // Assert
  expect(result).toBe(expectedResult);
});
```

### Testing Exceptions

```python
# Python
import pytest

def test_function_raises_expected_error():
    with pytest.raises(ValidationError) as exc_info:
        function_that_should_raise(invalid_input)
    
    assert "expected message" in str(exc_info.value)
```

```typescript
// TypeScript
it('should throw expected error', () => {
  expect(() => functionThatShouldThrow(invalidInput))
    .toThrow(ValidationError);
});
```

### Parametrized Tests

```python
# Python
import pytest

@pytest.mark.parametrize("input,expected", [
    ("valid@email.com", True),
    ("invalid-email", False),
    ("another@valid.com", True),
])
def test_email_validation_with_multiple_inputs(input, expected):
    result = validate_email(input)
    assert result == expected
```

```typescript
// TypeScript
it.each([
  ['valid@email.com', true],
  ['invalid-email', false],
  ['another@valid.com', true],
])('should validate email: %s -> %s', (input, expected) => {
  const result = validateEmail(input);
  expect(result).toBe(expected);
});
```

---

## Anti-Patterns

### ❌ DON'T DO THESE

#### 1. Testing Implementation Details

```python
# ❌ Bad - Testing internal implementation
def test_user_service_calls_repository_method():
    user_service.create_user(data)
    mock_repository.save.assert_called_once()  # Testing HOW, not WHAT

# ✅ Good - Testing behavior
def test_user_service_creates_user_successfully():
    user = user_service.create_user(data)
    assert user.email == data["email"]  # Testing WHAT happens
```

#### 2. Dependent Tests

```python
# ❌ Bad - Tests depend on each other
def test_create_user():
    global created_user
    created_user = create_user(data)

def test_update_user():
    update_user(created_user.id, new_data)  # Depends on previous test

# ✅ Good - Independent tests
def test_create_user():
    user = create_user(data)
    assert user.id is not None

def test_update_user():
    user = create_user(data)  # Own setup
    updated = update_user(user.id, new_data)
    assert updated.name == new_data["name"]
```

#### 3. Meaningless Tests

```python
# ❌ Bad - Test that doesn't test anything
def test_user_exists():
    user = User(email="test@example.com")
    assert user is not None  # Of course it's not None, we just created it!

# ✅ Good - Test that validates behavior
def test_user_creation_sets_email_correctly():
    email = "test@example.com"
    user = User(email=email)
    assert user.email == email
```

#### 4. Too Many Assertions

```python
# ❌ Bad - Testing multiple things in one test
def test_user_and_product_and_order():
    user = create_user()
    assert user.id is not None
    
    product = create_product()
    assert product.price > 0
    
    order = create_order(user, product)
    assert order.total == product.price
    # If any assertion fails, we don't know which one!

# ✅ Good - One concept per test
def test_user_creation_assigns_id():
    user = create_user()
    assert user.id is not None

def test_product_has_positive_price():
    product = create_product()
    assert product.price > 0

def test_order_total_matches_product_price():
    user = create_user()
    product = create_product()
    order = create_order(user, product)
    assert order.total == product.price
```

#### 5. Sleeping in Tests

```python
# ❌ Bad - Using sleep
def test_async_operation():
    trigger_async_operation()
    time.sleep(5)  # Hope it's done by now...
    assert operation_completed()

# ✅ Good - Proper async handling or polling
async def test_async_operation():
    await trigger_async_operation()
    assert await operation_completed()
```

---

## Continuous Improvement

### Review Test Quality Regularly

- **Coverage trends**: Is it going up or down?
- **Test execution time**: Are tests getting slower?
- **Flaky tests**: Track and fix tests that fail intermittently
- **Test maintenance**: Are tests easy to update when code changes?

### Refactor Tests Too

- **DRY**: Extract common setup to fixtures
- **Clarity**: Tests should be as readable as production code
- **Speed**: Optimize slow tests (use mocks, smaller datasets)

---

**Remember**: Tests are documentation. Future developers (including yourself) will read tests to understand how the system works. Make them clear, meaningful, and maintainable.

---

**Version**: 1.0.0  
**Last Updated**: 2026-01-24

