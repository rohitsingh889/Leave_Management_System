# Engineering Standards Checklist

## 1. Naming Conventions

The project should use clear and consistent naming conventions across the codebase.

- Use meaningful and descriptive names.
- Use `snake_case` for variables and functions in Python.
- Use `PascalCase` for classes.
- Use consistent naming for database tables and fields.
- Avoid unclear abbreviations.
- Function names should clearly describe the operation they perform.

Example:

    leave_request
    employee_id
    calculate_leave_balance()

---

## 2. Branch Naming

Branches should follow a consistent naming format so that their purpose is easy to understand.

Recommended formats:

    feature/<feature-name>
    bugfix/<bug-name>
    hotfix/<issue-name>

Examples:

    feature/leave-request
    feature/leave-approval
    bugfix/leave-validation
    hotfix/leave-balance

Each branch should be created for a specific task or issue.

---

## 3. Commit Format

Commit messages should be short, clear, and describe the actual change.

Recommended format:

    <type>: <description>

Common commit types:

- `feat` - New feature
- `fix` - Bug fix
- `test` - Testing changes
- `docs` - Documentation changes
- `refactor` - Code restructuring
- `chore` - Maintenance changes

Examples:

    feat: add leave request validation
    fix: prevent invalid leave dates
    test: add leave balance tests
    docs: update leave API documentation
    refactor: simplify leave approval logic

---

## 4. Pull Request (PR) Process

The project should follow a standard Pull Request workflow.

    Issue
       ↓
    Create Feature Branch
       ↓
    Implement Changes
       ↓
    Test Changes
       ↓
    Commit Changes
       ↓
    Push Branch
       ↓
    Create Pull Request
       ↓
    Code Review
       ↓
    Address Review Comments
       ↓
    Approval
       ↓
    Merge

A PR should contain a clear description of the changes and reference the related issue when applicable.

---

## 5. Code Review

Code review should be completed before merging changes into the main branch.

Reviewers should check:

- Code follows project standards.
- Code is readable and maintainable.
- Business logic is correct.
- Input validation is implemented.
- Error handling is appropriate.
- Security issues are not introduced.
- Tests are included where required.
- No unnecessary code is added.
- Documentation is updated when required.

All important review comments should be addressed before merging.

---

## 6. Testing Requirement

Every feature or bug fix should be tested before the Pull Request is merged.

Testing should cover:

- Valid inputs.
- Invalid inputs.
- Business rules.
- Authorization.
- Error handling.
- Important edge cases.
- API responses.

For the Leave Management System, tests should include:

- Valid leave request submission.
- Invalid leave dates.
- Insufficient leave balance.
- Invalid leave type.
- Unauthorized approval.
- Unauthorized rejection.
- Approval of a pending request.
- Rejection of a pending request.
- Attempt to process an already processed request.

All required tests should pass before the change is considered complete.

---

## 7. Documentation Requirement

Documentation should be updated whenever a change affects system functionality or technical design.

Documentation may include:

- README
- Functional requirements
- Technical documentation
- API documentation
- Database documentation
- Architecture documentation
- Changelog
- Code comments where necessary

Examples:

- A new API endpoint requires an API documentation update.
- A database structure change requires a database documentation update.
- An architecture change requires an architecture documentation update.
- A major feature change should be recorded in the changelog.

Documentation should remain consistent with the actual system.

---

## 8. Engineering Standards Checklist

| Standard | Requirement | Status |
|---|---|---|
| Naming Conventions | Use clear and consistent names | ☐ |
| Branch Naming | Follow standard branch naming format | ☐ |
| Commit Format | Use meaningful commit messages | ☐ |
| PR Process | Create and review PR before merging | ☐ |
| Code Review | Complete review and address comments | ☐ |
| Testing | Required tests are implemented and passing | ☐ |
| Documentation | Required documentation is updated | ☐ |