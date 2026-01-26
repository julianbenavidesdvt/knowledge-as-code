# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]  
**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]  
**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]  
**Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]  
**Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]
**Project Type**: [single/web/mobile - determines source structure]  
**Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]  
**Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]  
**Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

[Gates determined based on constitution file]

## CI/CD Configuration

### Pipeline Architecture

**Required Stages** (must be configured before implementation):

```yaml
Pipeline:
  1. Pre-Commit:
     - Lint check (fast fail)
     - Format validation
     - Type checking (if applicable)
  
  2. Commit/Push:
     - Unit tests (< 30s target)
     - Coverage check (>= 80%)
  
  3. Pull Request:
     - Full lint & format check
     - Unit tests
     - Contract tests
     - Integration tests
     - Coverage report (>= 80% gate)
     - Security scan
     - Build verification
  
  4. Pre-Merge:
     - All tests passing
     - Code review approved
     - Branch up-to-date with main
  
  5. Post-Merge (optional):
     - E2E tests
     - Performance benchmarks
     - Deploy to staging
```

### Quality Gates (Blocking)

Pipeline will **FAIL** and **BLOCK** merge if:

- [ ] Any test fails (unit, contract, integration)
- [ ] Coverage drops below 80%
- [ ] Linter errors or warnings exist
- [ ] Critical security vulnerabilities found
- [ ] Build fails
- [ ] Code review not approved

### Configuration Files Required

**Select based on your stack (from Technical Context above)**:

#### Python
```
- .pre-commit-config.yaml   # Pre-commit hooks
- pytest.ini                # Test configuration
- .coveragerc               # Coverage config
- pyproject.toml            # Project config (optional)
- requirements-dev.txt      # Dev dependencies
```

#### Node.js/TypeScript
```
- .pre-commit-config.yaml   # Pre-commit hooks
- jest.config.js            # Jest config
- .eslintrc.js              # Lint config
- .prettierrc               # Format config
- package.json              # Scripts: test, lint, coverage
```

#### Other Languages
```
[Document specific config files for your stack]
```

### GitHub Actions Example (Adjust for your platform)

**File**: `.github/workflows/ci.yml`

```yaml
name: CI

on:
  pull_request:
  push:
    branches: [main, master]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup [Language]
        # Setup specific to your language/version
        
      - name: Install dependencies
        run: [install command]
        
      - name: Lint
        run: [lint command]
        
      - name: Run unit tests
        run: [test command] tests/unit/
        
      - name: Run contract tests
        run: [test command] tests/contract/
        
      - name: Run integration tests
        run: [test command] tests/integration/
        
      - name: Coverage report
        run: [coverage command] --fail-under=80
        
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        
      - name: Security scan
        run: [security scan command]
```

### Pre-Commit Hooks Configuration

**File**: `.pre-commit-config.yaml`

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      
  - repo: [language-specific linter repo]
    rev: [version]
    hooks:
      - id: [linter-hook]
      
  - repo: [language-specific formatter repo]
    rev: [version]
    hooks:
      - id: [formatter-hook]
```

### Testing Commands (Document in plan.md)

```bash
# Run all tests
[command to run all tests]

# Run specific test types
[command] tests/unit/
[command] tests/contract/
[command] tests/integration/
[command] tests/e2e/

# Run with coverage
[command] --cov=src --cov-report=html --cov-report=term

# Run and enforce coverage threshold
[command] --cov=src --cov-fail-under=80

# Run tests for specific user story
[command] -k "us1"

# Lint
[lint command]

# Format
[format command]

# Security scan
[security scan command]
```

### Coverage Targets

| Component Type | Coverage Target | Rationale |
|----------------|----------------|-----------|
| Models/Entities | 90% | Core data logic, high criticality |
| Services/Business Logic | 85% | Main business rules |
| Utils/Helpers | 95% | Reusable, should be rock solid |
| Controllers/Handlers | 75% | Thinner layer, more integration tested |
| **Overall Project** | **80%** | **Minimum required** |

### Continuous Monitoring

**After implementation, track**:
- Test execution time trends
- Coverage trends over time
- Pipeline failure rates
- Time to merge (lead time)

**Tools to consider**:
- Codecov / Coveralls (coverage tracking)
- SonarQube (code quality)
- Snyk / Dependabot (security)
- [Platform-specific monitoring]

### Local Development Workflow

Developers should run before pushing:

```bash
# 1. Pre-commit hooks (automatic)
git commit -m "message"  # Hooks run automatically

# 2. Run tests locally
make test                # Or equivalent

# 3. Check coverage
make coverage            # Or equivalent

# 4. Verify lint
make lint                # Or equivalent

# 5. Push
git push
```

### Branch Protection Rules

Configure in repository settings:

- [ ] Require pull request reviews (min 1 reviewer)
- [ ] Require status checks to pass before merging
- [ ] Require branches to be up to date before merging
- [ ] Require conversation resolution before merging
- [ ] Do not allow bypassing the above settings

### Deployment Strategy (If applicable)

**Environments**:
1. **Development**: Auto-deploy from feature branches (optional)
2. **Staging**: Auto-deploy from main after all tests pass
3. **Production**: Manual approval after staging validation

**Deployment Gates**:
- [ ] All tests passing in staging
- [ ] E2E tests passing
- [ ] Performance benchmarks acceptable
- [ ] Security scan clean
- [ ] Manual QA approval (if required)

---

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)
<!--
  ACTION REQUIRED: Replace the placeholder tree below with the concrete layout
  for this feature. Delete unused options and expand the chosen structure with
  real paths (e.g., apps/admin, packages/something). The delivered plan must
  not include Option labels.
-->

```text
# [REMOVE IF UNUSED] Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVE IF UNUSED] Option 2: Web application (when "frontend" + "backend" detected)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVE IF UNUSED] Option 3: Mobile + API (when "iOS/Android" detected)
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure: feature modules, UI flows, platform tests]
```

**Structure Decision**: [Document the selected structure and reference the real
directories captured above]

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
