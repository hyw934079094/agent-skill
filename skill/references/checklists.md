# Real Business Test Automation Checklists

Use this file only when the task needs deeper structure than the core skill body.

## 1. Project Understanding Checklist

Before generating tests, inspect as many of these as the project actually contains:

- Overall architecture
- Module boundaries
- Controllers or entry points
- Services
- Repositories or mappers
- DTOs, VOs, entities, and domain objects
- Database schema
- Table relationships
- State machines and state fields
- Enums and coded values
- Transaction boundaries
- Authentication and authorization
- Cache behavior
- MQ, events, or async flows
- Scheduled jobs
- External dependencies
- Configuration files
- Environment variables
- Initialization logic
- Test or seed data generation logic
- Exception handling

Understand the business from:

- Code paths
- Schema relationships
- Call chains
- State transitions

## 2. Database And Business Analysis Checklist

Find and confirm:

- Core business tables
- Parent, child, junction, status, log, and audit tables
- Key foreign-key-like relationships, even if not enforced physically
- Critical status fields
- State transition rules
- Unique constraints and conflict points
- Soft-delete behavior
- Transaction boundaries
- Places where inconsistent data may appear

The aim is to explain how the business moves, not just how single methods behave.

## 3. Coverage Checklist

### Main flow

- Normal user main path
- End-to-end business loop

### Branch flow

- Different statuses
- Different roles
- Different parameters
- Different business conditions

### Exception flow

- Invalid parameters
- Invalid status
- Missing or inconsistent data
- Permission failures
- Duplicate requests
- Concurrency risks
- Transaction failures
- Missing records

### Blocking flow

- Status not allowed
- Permission not allowed
- Preconditions not met
- Data conflicts
- Unique-key conflicts

### Boundary flow

- Null values
- Minimum and maximum values
- Minimum and maximum lengths
- Special states

### Consistency

- Parent-child consistency
- Status consistency
- Count consistency
- Amount consistency
- Relationship consistency

### Transaction behavior

- Successful rollback
- Half-success risk
- Dirty data risk

### Idempotency

- Repeat request safety
- Duplicate submission handling

## 4. Existing Test Audit Checklist

If tests already exist, check whether they:

- Are real tests rather than superficial scaffolding
- Depend on fake coverage or empty assertions
- Mock core business logic heavily
- Only verify success responses
- Pollute the database
- Depend on fixed execution order
- Cannot be rerun safely
- Depend on dirty environment data
- Use brittle assertions
- Leave important business paths uncovered

## 5. Test Data Rules

Prefer this data creation order:

1. Real API creation
2. Real service creation
3. Real business flow creation
4. SQL setup only when the above are not practical

Do not:

- Bypass validation to insert data
- Forge illegal states
- Directly edit the database to skip the normal flow
- Leave irrecoverable changes on existing business data

## 6. Assertion Rules

A strong test usually proves several of these together:

- Business result
- Database write result
- State transition result
- Related data mutation
- Side effect result
- Idempotency behavior
- Rollback behavior

Avoid:

- `assertTrue(true)`
- Assertions that only prove a response exists
- Tests that only log output
- Tests that only verify HTTP 200

## 7. Failure Reporting Checklist

When a test cannot be completed through normal business flow, report:

- Problem location
- Root cause
- Involved modules
- Risk level
- Why normal flow is insufficient to resolve it
- Which business areas may be affected

## 8. Delivery Checklist

A strong delivery should include:

- Business analysis
- Coverage analysis
- Test implementation
- Data generation approach
- Cleanup or restore approach
- Failure analysis if present
- Potential system risk if found
- Uncovered areas and why they remain uncovered
