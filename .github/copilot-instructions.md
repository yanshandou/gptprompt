# Project Instructions

Follow these instructions whenever generating, reviewing, or modifying code in this repository.

## General Principles

* Prioritize readability, maintainability, and consistency over brevity.
* Follow the existing project architecture, directory structure, naming conventions, and coding style.
* Inspect nearby files and similar existing implementations before generating new code.
* Prefer existing project patterns over introducing new abstractions.
* Keep all changes limited to the requested scope.
* Do not perform unrelated refactoring.
* Do not change public method signatures unless explicitly requested.
* Do not rename existing classes, methods, variables, database fields, routes, or configuration keys unless explicitly requested.
* Do not introduce third-party dependencies unless explicitly requested.
* Do not invent classes, methods, services, exceptions, configuration values, database fields, or utility functions that do not exist in the project.
* When required information is missing, clearly state the assumption instead of silently inventing project behavior.
* If the requested implementation conflicts with an existing project convention, identify the conflict before changing the code.

## Architecture

* Preserve the existing separation of concerns.
* Controllers should receive input, validate input, call the appropriate service, and return responses.
* Controllers must not contain core business logic.
* Services should contain business rules and application logic.
* Repositories should only handle data access.
* Repositories must not format HTTP responses or view data.
* DTOs and value objects must not perform database queries.
* Do not bypass an existing service or repository layer for convenience.
* Do not introduce a new design pattern unless it is already used in the project or explicitly requested.

## Change Strategy

* Prefer the smallest safe change that satisfies the requirement.
* Reuse existing project components where appropriate.
* Do not duplicate existing functionality.
* Do not modify unrelated files.
* Do not remove existing validation, logging, authorization, error handling, or tests unless explicitly requested.
* Preserve backward compatibility unless the requirement explicitly allows breaking changes.

## Generated Output

When responding with code changes:

1. Identify the files that need to be created or modified.
2. Explain the implementation briefly.
3. Provide complete code for the changed sections.
4. Clearly state any assumptions.
5. Include or update relevant tests.
6. Review the result against all applicable repository instructions.
7. Report any rule that could not be followed.

Do not return pseudocode unless explicitly requested.
Do not omit important validation, error handling, or edge-case handling.
