# Git Workflow Guidelines

This document defines the **standard Git workflow** for our development, staging, and production environments.

---

## Branch Structure

- **`development`** → Active development branch, used for integrating new features and fixes.
- **`staging`** → Pre-release testing branch; mirrors production environment for validation.
- **`production`** → Stable branch; only merged after full testing and approval.

---

## Branch Naming Rules

- `feature/<name>` → New feature development.  
  Example: `feature/login-api`
- `bugfix/<name>` → Fixing bugs in development/staging.  
  Example: `bugfix/fix-timezone`
- `hotfix/<name>` → Urgent fix directly for production.  
  Example: `hotfix/payment-crash`
- `refactor/<name>` → Code improvement without changing functionality.  
  Example: `refactor/cleanup-auth`
- `docs/<name>` → Documentation updates.  
  Example: `docs/api-guidelines`

---

## Commit Message Convention

Use **Conventional Commits** format:

```
<type>(<scope>): <short summary>

[optional body]
[optional footer]
```

### Commit Types
| Type | Description | Example |
|------|--------------|----------|
| `feat` | Add new feature | `feat(auth): add JWT login` |
| `fix` | Fix a bug | `fix(ui): resolve layout issue on dashboard` |
| `refactor` | Code restructure | `refactor(api): simplify request handler` |
| `chore` | Routine tasks (build, config) | `chore: update dependencies` |
| `docs` | Documentation only | `docs: update README` |
| `test` | Add or fix tests | `test: add auth unit tests` |

---

## Git Workflow Steps

1. **Start a new branch**
   ```bash
   git checkout development
   git pull origin development
   git checkout -b feature/your-feature
   ```

2. **Commit changes**
   ```bash
   git add .
   git commit -m "feat(feature-name): add new feature"
   ```

3. **Push to remote**
   ```bash
   git push origin feature/your-feature
   ```

4. **Create Merge Request (MR)**
   - Reviewer must perform **code review**.
   - The MR triggers **CI/CD pipelines** (linting, testing, build checks).

5. **Merge Rules**
   - MR → `development`: feature integration.
   - MR → `staging`: after QA validation.
   - MR → `production`: after staging approval.

---

## CI/CD Integration

`.gitlab-ci.yml` automatically triggers jobs based on branch:

```yaml
stages:
  - test
  - build
  - deploy

deploy_development:
  stage: deploy
  script:
    - sh deploy/dev.sh
  only:
    - development

deploy_staging:
  stage: deploy
  script:
    - sh deploy/staging.sh
  only:
    - staging

deploy_production:
  stage: deploy
  script:
    - sh deploy/prod.sh
  only:
    - production
```

---

## Code Review Policy

- All MRs require **at least one code review** before merging.
- Review checks include:
  - Duplicate or unused code
  - Code readability and structure
  - Security and performance
  - Test coverage

Use tools like:
- **SonarQube** → static code analysis
- **Codacy / CodeClimate** → automated quality checks
- **GitLab Code Review** → inline comments

---

## WhatsApp Delivery Format

Use the following template when notifying the team about a new MR (Merge Request), deployment, or important delivery via WhatsApp or team chat.

### Template
```bash
<type_of_delivery> <project_name>:
- <type>: <short description>
cc: @<reviewer1> @<reviewer2> ...
note: <optional notes or testing instructions>
```

### Example — Merge Request
```bash
MR gateway_services:
- feat: add new OTP verification flow
cc: @hoeril @iqbal
note: please test with dummy number before merge
```

### Example — Hotfix Delivery
```bash
Hotfix gateway_services:
- hotfix: fix login POST issue
cc: @hoeril
note: tested on staging, urgent production patch
```

---

## Versioning

Follow **Semantic Versioning (SemVer)**:  
`MAJOR.MINOR.PATCH`

| Level | Description | Example |
|--------|--------------|----------|
| MAJOR | Breaking changes | 2.0.0 |
| MINOR | Backward-compatible features | 2.1.0 |
| PATCH | Bug fixes | 2.1.1 |

---

## Best Practices

- Keep commits **small and meaningful**.
- Avoid committing directly to `staging` or `production`.
- Run local tests before pushing.
- Use `.gitignore` properly to exclude configs and binaries.
- Regularly rebase with `development` to avoid conflicts.

---

**Summary Flow:**

```
feature/bugfix → merge → development → merge → staging → merge → production
```

Each step includes:
- Code review
- CI/CD validation
- Deployment tests

---

**Team Roles**
- **Developer** → Create branches, implement features, write tests.
- **Reviewer** → Review MR, ensure quality and consistency.
- **DevOps** → Manage CI/CD pipelines and deployment environments.

---
