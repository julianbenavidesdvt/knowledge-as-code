---

description: "Task list template for feature implementation"
---

# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Tests are **OBLIGATORIOS** según Constitution. Seguir la pirámide de testing:
- **Unit Tests** (50-70%): Funciones/métodos individuales
- **Contract Tests** (10-15%): Interfaces API
- **Integration Tests** (15-25%): Flujos multi-componente
- **E2E Tests** (5-10%): Casos críticos completos

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/`, `tests/` at repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` or `android/src/`
- Paths shown below assume single project - adjust based on plan.md structure

<!-- 
  ============================================================================
  IMPORTANT: The tasks below are SAMPLE TASKS for illustration purposes only.
  
  The /speckit.tasks command MUST replace these with actual tasks based on:
  - User stories from spec.md (with their priorities P1, P2, P3...)
  - Feature requirements from plan.md
  - Entities from data-model.md
  - Endpoints from contracts/
  
  Tasks MUST be organized by user story so each story can be:
  - Implemented independently
  - Tested independently
  - Delivered as an MVP increment
  
  DO NOT keep these sample tasks in the generated tasks.md file.
  ============================================================================
-->

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create project structure per implementation plan
- [ ] T002 Initialize [language] project with [framework] dependencies
- [ ] T003 [P] Configure linting and formatting tools
- [ ] T004 [P] Setup test infrastructure (test runner, coverage tools)
- [ ] T005 [P] Configure pre-commit hooks
- [ ] T006 [P] Setup CI/CD pipeline configuration

---

## Phase 1.5: Test Infrastructure Setup (OBLIGATORIO)

**Purpose**: Shared test infrastructure and utilities

**⚠️ CRITICAL**: Must be complete before writing ANY tests

### Test Structure

```
tests/
├── conftest.py              # Fixtures compartidos
├── fixtures/                # Datos de prueba
│   └── sample_data.json
├── mocks/                   # Mocks reutilizables
│   └── external_services.py
├── unit/                    # Unit tests (50-70%)
│   ├── models/
│   ├── services/
│   └── utils/
├── contract/                # Contract tests (10-15%)
├── integration/             # Integration tests (15-25%)
└── e2e/                     # E2E tests (5-10%)
```

### Tareas

- [ ] T007 Create tests/ directory structure
- [ ] T008 [P] Configure conftest.py with base fixtures
- [ ] T009 [P] Create tests/fixtures/ directory with sample data
- [ ] T010 [P] Create tests/mocks/ with common mocks
- [ ] T011 Setup coverage configuration (target: 80% overall)
- [ ] T012 [P] Create unit test utilities in tests/unit/test_utils.py
- [ ] T013 Document testing standards in tests/README.md

**Checkpoint**: Test infrastructure ready - test writing can now begin

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

Examples of foundational tasks (adjust based on your project):

- [ ] T014 Setup database schema and migrations framework
- [ ] T015 [P] Implement authentication/authorization framework
- [ ] T016 [P] Setup API routing and middleware structure
- [ ] T017 Create base models/entities that all stories depend on
- [ ] T018 Configure error handling and logging infrastructure
- [ ] T019 Setup environment configuration management

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - [Title] (Priority: P1) 🎯 MVP

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Unit Tests for User Story 1 (OBLIGATORIOS) 🧪

> **⚠️ TDD: Write these tests FIRST, ensure they FAIL, then implement**

**Coverage Target**: 85% para esta user story

#### Model Tests
- [ ] T020 [P] [US1] Unit test for [Entity1] model validation in tests/unit/models/test_[entity1].py
- [ ] T021 [P] [US1] Unit test for [Entity1] edge cases in tests/unit/models/test_[entity1].py
- [ ] T022 [P] [US1] Unit test for [Entity2] model in tests/unit/models/test_[entity2].py

#### Service Tests
- [ ] T023 [P] [US1] Unit test for [Service] happy path in tests/unit/services/test_[service].py
- [ ] T024 [P] [US1] Unit test for [Service] error handling in tests/unit/services/test_[service].py
- [ ] T025 [P] [US1] Unit test for [Service] edge cases in tests/unit/services/test_[service].py

#### Utils/Helpers Tests
- [ ] T026 [P] [US1] Unit test for [helper function] in tests/unit/utils/test_[util].py

**Checkpoint Unit Tests**: All unit tests written and FAILING ❌

### Contract Tests for User Story 1 (Si aplica API) 📋

- [ ] T027 [P] [US1] Contract test for [endpoint] request schema in tests/contract/test_[name].py
- [ ] T028 [P] [US1] Contract test for [endpoint] response schema in tests/contract/test_[name].py
- [ ] T029 [P] [US1] Contract test for [endpoint] error responses in tests/contract/test_[name].py

### Integration Tests for User Story 1 🔗

> **NOTE: Write these BEFORE implementation, ensure they FAIL**

- [ ] T030 [P] [US1] Integration test for [user journey] happy path in tests/integration/test_[name].py
- [ ] T031 [P] [US1] Integration test for [user journey] error scenarios in tests/integration/test_[name].py

**Checkpoint Tests**: All tests (unit + contract + integration) written and FAILING ❌

### Implementation for User Story 1

> **⚠️ IMPLEMENTATION ONLY STARTS AFTER ALL TESTS ARE WRITTEN AND FAILING**

- [ ] T032 [P] [US1] Create [Entity1] model in src/models/[entity1].py
- [ ] T033 [P] [US1] Create [Entity2] model in src/models/[entity2].py
- [ ] T034 [US1] Implement [Service] in src/services/[service].py (depends on T032, T033)
- [ ] T035 [US1] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T036 [US1] Add validation and error handling
- [ ] T037 [US1] Add logging for user story 1 operations

**Checkpoint Implementation**: Code complete - now verify tests pass ✅

### Verification for User Story 1

- [ ] T038 Run all unit tests for US1 - should be GREEN ✅
- [ ] T039 Run all contract tests for US1 - should be GREEN ✅
- [ ] T040 Run all integration tests for US1 - should be GREEN ✅
- [ ] T041 Verify coverage >= 85% for US1 code
- [ ] T042 Verify no lint errors

**Checkpoint**: At this point, User Story 1 should be fully functional, tested, and ready

---

## Phase 4: User Story 2 - [Title] (Priority: P2)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Unit Tests for User Story 2 (OBLIGATORIOS) 🧪

> **⚠️ TDD: Write these tests FIRST, ensure they FAIL, then implement**

- [ ] T043 [P] [US2] Unit tests for [Entity] model in tests/unit/models/test_[entity].py
- [ ] T044 [P] [US2] Unit tests for [Service] in tests/unit/services/test_[service].py
- [ ] T045 [P] [US2] Unit tests for helpers/utils in tests/unit/utils/test_[util].py

### Contract Tests for User Story 2 (Si aplica API) 📋

- [ ] T046 [P] [US2] Contract test for [endpoint] in tests/contract/test_[name].py

### Integration Tests for User Story 2 🔗

- [ ] T047 [P] [US2] Integration test for [user journey] in tests/integration/test_[name].py

**Checkpoint Tests**: All US2 tests written and FAILING ❌

### Implementation for User Story 2

- [ ] T048 [P] [US2] Create [Entity] model in src/models/[entity].py
- [ ] T049 [US2] Implement [Service] in src/services/[service].py
- [ ] T050 [US2] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T051 [US2] Integrate with User Story 1 components (if needed)

### Verification for User Story 2

- [ ] T052 Run all tests for US2 - should be GREEN ✅
- [ ] T053 Verify coverage >= 85% for US2 code
- [ ] T054 Verify US1 still passes (no regression)

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - [Title] (Priority: P3)

**Goal**: [Brief description of what this story delivers]

**Independent Test**: [How to verify this story works on its own]

### Unit Tests for User Story 3 (OBLIGATORIOS) 🧪

- [ ] T055 [P] [US3] Unit tests for [Entity] model in tests/unit/models/test_[entity].py
- [ ] T056 [P] [US3] Unit tests for [Service] in tests/unit/services/test_[service].py

### Contract Tests for User Story 3 (Si aplica API) 📋

- [ ] T057 [P] [US3] Contract test for [endpoint] in tests/contract/test_[name].py

### Integration Tests for User Story 3 🔗

- [ ] T058 [P] [US3] Integration test for [user journey] in tests/integration/test_[name].py

**Checkpoint Tests**: All US3 tests written and FAILING ❌

### Implementation for User Story 3

- [ ] T059 [P] [US3] Create [Entity] model in src/models/[entity].py
- [ ] T060 [US3] Implement [Service] in src/services/[service].py
- [ ] T061 [US3] Implement [endpoint/feature] in src/[location]/[file].py

### Verification for User Story 3

- [ ] T062 Run all tests for US3 - should be GREEN ✅
- [ ] T063 Verify coverage >= 85% for US3 code
- [ ] T064 Verify US1 and US2 still pass (no regression)

**Checkpoint**: All user stories should now be independently functional

---

[Add more user story phases as needed, following the same pattern]

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] TXXX [P] Documentation updates in docs/
- [ ] TXXX Code cleanup and refactoring
- [ ] TXXX Performance optimization across all stories
- [ ] TXXX Security hardening
- [ ] TXXX Run quickstart.md validation

### E2E Tests (Casos Críticos) 🎯

> **Solo para flujos críticos completos (5-10% de tests)**

- [ ] TXXX [P] E2E test for critical user flow in tests/e2e/test_[flow].py
- [ ] TXXX [P] E2E test for payment flow (if applicable) in tests/e2e/test_payment.py

### Coverage Final

- [ ] TXXX Run full test suite and verify all passing ✅
- [ ] TXXX Verify overall coverage >= 80%
- [ ] TXXX Generate coverage report
- [ ] TXXX Verify pyramid ratio (50-70% unit, 10-15% contract, 15-25% integration, 5-10% e2e)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - May integrate with US1 but should be independently testable
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - May integrate with US1/US2 but should be independently testable

### Within Each User Story

- **Tests FIRST** (TDD obligatorio):
  1. Write unit tests → FAIL ❌
  2. Write contract tests → FAIL ❌
  3. Write integration tests → FAIL ❌
  4. Implement code → Tests pass ✅
- Models before services
- Services before endpoints
- Core implementation before integration
- Verification after implementation
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Step 1: Launch all UNIT tests for User Story 1 together:
Task: "Unit test for [Entity1] model in tests/unit/models/test_[entity1].py"
Task: "Unit test for [Entity2] model in tests/unit/models/test_[entity2].py"
Task: "Unit test for [Service] in tests/unit/services/test_[service].py"

# Step 2: Launch all CONTRACT tests together:
Task: "Contract test for [endpoint] in tests/contract/test_[name].py"

# Step 3: Launch all INTEGRATION tests together:
Task: "Integration test for [user journey] in tests/integration/test_[name].py"

# Step 4: Verify all tests FAIL ❌ (Red phase of TDD)

# Step 5: Launch all models implementation together (after tests fail):
Task: "Create [Entity1] model in src/models/[entity1].py"
Task: "Create [Entity2] model in src/models/[entity2].py"

# Step 6: Verify tests now PASS ✅ (Green phase of TDD)
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Add User Story 3 → Test independently → Deploy/Demo
5. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1
   - Developer B: User Story 2
   - Developer C: User Story 3
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- **TDD OBLIGATORIO**: Tests fail → Implement → Tests pass ✅
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence
- **Coverage**: Mantener siempre >= 80% overall
- **Pyramid ratio**: Verificar distribución correcta de tipos de tests

## Test Naming Conventions

### Unit Tests

```python
def test_<función>_<escenario>_<resultado_esperado>():
    """
    Ejemplos:
    - test_calculate_total_with_discount_returns_reduced_price()
    - test_validate_email_with_invalid_format_raises_error()
    - test_get_user_when_not_found_returns_none()
    """
    pass
```

### Integration Tests

```python
def test_<user_story>_<flujo>():
    """
    Ejemplos:
    - test_us1_user_can_complete_checkout()
    - test_us2_admin_creates_product_successfully()
    """
    pass
```

### Contract Tests

```python
def test_<endpoint>_<método>_<validación>():
    """
    Ejemplos:
    - test_post_users_validates_required_fields()
    - test_get_products_returns_correct_schema()
    """
    pass
```
