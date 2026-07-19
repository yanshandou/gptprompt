---

## applyTo: 'tests/**/*.php'

# PHP Test Instructions

Follow these rules whenever generating or modifying PHP tests.

## General Rules

* Use the testing framework and version already configured in the project.
* Do not introduce a different testing framework.
* Follow the style of nearby existing tests.
* Keep each test focused on one observable behavior.
* Use clear and descriptive test method names.
* Use the Arrange, Act, Assert structure.
* Avoid conditional logic inside tests.
* Do not use loops to combine unrelated test cases.
* Use data providers only when the same behavior is tested with different input values.
* Do not use data providers when they make failures difficult to understand.
* Tests must be deterministic.
* Do not depend on test execution order.
* Do not use real external HTTP services.
* Do not depend on the current system date or time without controlling the clock.
* Do not use random values unless the random seed is fixed and the value is relevant to the test.
* Clean up any data or resources created by the test.

## Test Coverage

For changed behavior, include applicable tests for:

* Successful execution
* Invalid input
* Missing required input
* Empty input
* Boundary values
* Resource not found
* Authorization failure
* Dependency failure
* Database failure
* Duplicate execution or idempotency
* Transaction rollback

Only add cases relevant to the requested behavior.

## Assertions

* Assert the actual business result, not only that a method was called.
* Use the most specific assertion available.
* Avoid assertions that verify multiple unrelated outcomes.
* Include assertion messages only when they improve failure diagnosis.
* Do not assert implementation details unless those details are part of the public contract.

## Mocks and Stubs

* Mock external boundaries such as repositories, HTTP clients, queues, and file systems where appropriate.
* Do not mock the class being tested.
* Avoid mocking value objects and simple data objects.
* Do not mock every internal method.
* Prefer state-based assertions when practical.
* Verify interactions only when the interaction itself is a requirement.
* Do not use overly permissive mocks that accept any argument without reason.

## Database Tests

* Use the project's existing test database strategy.
* Do not connect to production or shared environments.
* Keep fixtures minimal and relevant.
* Do not rely on existing database records.
* Ensure each test starts from a known state.
* Use transactions or cleanup mechanisms already established by the project.

## Test Naming

Test names should clearly describe:

* The behavior being tested
* The condition
* The expected outcome

Preferred examples:

```php
public function testReturnsUserWhenUserExists(): void
{
}

public function testReturnsNullWhenUserDoesNotExist(): void
{
}

public function testThrowsExceptionWhenRepositoryFails(): void
{
}
```

Avoid vague names such as:

```php
public function testUser(): void
{
}

public function testSuccess(): void
{
}
```

## Final Review

Before returning test code:

1. Confirm the tests follow the existing project testing style.
2. Confirm each test verifies one clear behavior.
3. Confirm external dependencies are isolated where required.
4. Confirm the tests do not depend on execution order or external state.
5. Confirm both successful and relevant failure cases are covered.
