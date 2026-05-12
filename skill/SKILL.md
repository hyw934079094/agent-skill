---
name: real-business-test-automation
description: Use only when a backend project or core module is already functionally complete or near feature-complete, and the user now wants to add, audit, or repair end-to-end automated business-flow tests that validate real workflows instead of superficial passing checks. Best for Java or service-based systems with databases, state transitions, permissions, transactions, MQ or events, scheduled jobs, and integration points.
---

# Real Business Test Automation

This skill is for the post-implementation verification phase: building a trustworthy business test system after the project or module is already largely built, not for routine feature delivery work.

## Use This Skill When

- The project or target module is already implemented, and the user now wants automated tests that prove business flows are truly working.
- Existing tests need a post-delivery audit because they look shallow, heavily mocked, order-dependent, or unreliable.
- The user is entering a stabilization, QA hardening, release-readiness, or post-development verification phase.
- The project relies on real state transitions, database consistency, transactions, side effects, permissions, or idempotency.

## Do Not Use This Skill When

- The user is still actively building core business features.
- The task is a normal bug fix, refactor, or small code change unrelated to business-flow test verification.
- The request is mainly about unit tests for isolated methods rather than end-to-end or integration-level business confidence.

## Working Mode

Prefer honest conclusions over forced passing tests.

- If business behavior is unclear, verify from code, schema, and call chains first.
- If something cannot be proven from the repository or runtime behavior, ask the user instead of guessing.
- If a failure is caused by a real system problem, surface it clearly rather than hiding it.

## Workflow

### 1. Understand the project before writing tests

Inspect the project structure, business modules, entry points, service layer, data layer, DTO or entity model, configuration, authentication, permissions, cache, MQ or events, scheduled jobs, initialization logic, and exception handling.

Use [references/checklists.md](references/checklists.md) for the full discovery checklist.

Do not generate tests before you understand the business path you are trying to prove.

### 2. Build the business map

Identify:

- Core business modules
- Core business tables
- Main and child records
- State fields and transitions
- Transaction boundaries
- Uniqueness and consistency constraints
- Key side effects and downstream writes

The goal is to understand how the business actually flows through the system.

### 3. Audit existing tests first

If tests already exist, review them before adding more.

Look for:

- Fake coverage
- Heavy mocking of core logic
- Assertions with little business meaning
- Database pollution
- Order dependence
- Environment dependence
- Missing cleanup or restore logic

Repair weak tests before treating them as trustworthy coverage.

### 4. Design coverage from business risk

Cover at least these categories:

- Main flow
- Branch flow
- Exception flow
- Blocking flow
- Boundary cases
- Data consistency
- Transaction behavior
- Idempotency

Re-check for uncovered business paths after each batch of tests.

### 5. Choose the highest-value test type

Prefer this order:

1. Integration Test
2. API Test
3. Service Integration Test
4. Repository Verification
5. Unit Test

Avoid mocking core business behavior unless the dependency is external and uncontrollable.

Allowed mock targets usually include payment gateways, SMS, email, or other external systems that the project does not own.

### 6. Generate data through real flows

Prefer this order:

1. Real API flow
2. Real service flow
3. Real business workflow
4. SQL setup as the last resort

Do not create passing conditions by directly editing database state in ways real users or real business logic would never produce.

### 7. Write real assertions

Assertions should prove the business actually completed.

Prefer asserting:

- Business result
- Database changes
- State transition
- Related table changes
- Side effects
- Idempotency result
- Rollback result

Avoid tests that only assert non-null responses, success flags, or HTTP 200.

### 8. Keep data closed-loop

Every test should leave the environment reusable.

- Clean up created data when possible
- Restore modified data when reuse is required
- Avoid leaving dirty, partial, or intermediate states
- Make repeated execution safe

### 9. Handle failures honestly

When a test fails:

- Analyze root cause
- Inspect call chain
- Inspect state flow
- Inspect transaction behavior
- Try to repair through normal business paths

If it still cannot be completed, report the blocker explicitly instead of hiding it.

### 10. Output in a reusable structure

When delivering work, include:

1. Business analysis
2. Coverage analysis
3. Test code
4. Test data generation plan
5. Data cleanup or restore plan
6. Failure analysis when applicable
7. System risks when discovered
8. Reasons for uncovered areas when applicable

## Reminders

- Do not guess business semantics.
- Do not invent nonexistent states or behaviors.
- Do not bypass the normal flow just to make tests pass.
- Do not hide uncovered risk.
- Keep tests readable, maintainable, stable, and repeatable.
