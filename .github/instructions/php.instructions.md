---

## applyTo: '**/*.php'

# PHP Coding Standards

Follow these rules whenever generating, reviewing, or modifying PHP code.

## General PHP Style

* Follow PSR-12.
* Every PHP file must declare strict types unless the file format does not permit it.

```php
declare(strict_types=1);
```

* Prioritize clear and explicit code over concise or advanced syntax.
* Do not use advanced language features merely to reduce the number of lines.
* Match the style of nearby PHP files when it does not conflict with these instructions.
* Keep lines within 120 characters where practical.
* Define only one primary class per file.
* Remove unused imports, variables, parameters, and methods.
* Do not leave commented-out obsolete code.

## Strings

* Use single quotes for normal PHP strings.

```php
$message = 'Success';
```

* Use double quotes only when escape sequences or variable interpolation are required.

```php
$message = "First line\nSecond line";
```

* Prefer string concatenation over variable interpolation when it improves readability.

```php
$message = 'User: ' . $userName;
```

* Add one space before and after the concatenation operator.
* Do not manually construct JSON strings. Use `json_encode()`.
* Do not construct SQL by concatenating external values.
* Avoid Heredoc and Nowdoc unless clearly necessary for long text.
* Compare empty strings explicitly with `=== ''`.
* Do not use `empty()` when `'0'`, `0`, `false`, and `null` must be distinguished.

## Control Flow

* Do not use `while`.
* Do not use `do...while`.
* Do not use recursion.
* Do not use `goto`.
* Loop nesting must not exceed three levels.
* Conditional nesting must not exceed three levels.
* Prefer early returns to deeply nested conditional blocks.
* Do not use nested ternary expressions.
* Do not use shorthand ternary expressions such as `?:`.
* Do not assign values inside conditional expressions.
* Do not use multi-level control statements such as `break 2` or `continue 2`.
* Avoid multiple `break` or `continue` statements in the same loop.
* Extract long or complex loop bodies into clearly named methods.
* Do not modify the collection currently being iterated.
* Do not perform database queries inside loops.

Preferred:

```php
if ($user === null) {
    return null;
}

if (!$user->isActive()) {
    return null;
}

return $user;
```

Avoid:

```php
if ($user !== null) {
    if ($user->isActive()) {
        return $user;
    }
}

return null;
```

## Restricted Syntax

* Do not use reference assignment.

```php
$value =& $source;
```

* Do not define reference parameters.
* Do not return values by reference.
* Do not capture closure variables by reference.
* Do not use dynamic variables such as `$$variableName`.
* Avoid dynamic method calls such as `$object->{$methodName}()` unless explicitly required.
* Do not use `eval()`.
* Do not use `extract()`.
* Avoid `compact()`; define array keys explicitly.
* Do not use anonymous classes.
* Do not use magic methods for core business logic.
* Avoid complex closures.
* Arrow functions may only be used for simple, readable, single-expression operations.
* Do not use chained assignment.

Avoid:

```php
$first = $second = $third = 0;
```

Preferred:

```php
$first = 0;
$second = 0;
$third = 0;
```

* Do not place increment or decrement operations inside complex expressions.

Avoid:

```php
$result[$index++] = $value;
```

Preferred:

```php
$result[$index] = $value;
$index++;
```

## Type Safety

* Declare types for all method parameters.
* Declare return types for all methods.
* Declare types for all class properties.
* Use `===` and `!==` by default.
* Do not rely on implicit PHP type conversion.
* Avoid `mixed`.
* Avoid union types that contain multiple unrelated types.
* A method should return one consistent type.
* When a single entity is not found, return `null`.
* Do not mix `null`, `false`, an empty string, and an empty array to represent the same failure.
* Validate numeric strings before converting them.
* Avoid unclear casts such as `(array)` and `(object)`.
* Use PHPDoc array shapes or DTOs for complex arrays.

Example:

```php
/**
 * @return array<int, User>
 */
public function findUsers(): array
{
    // ...
}
```

## Naming

* Use clear and descriptive English names.
* Do not use transliterated names.
* Avoid unclear abbreviations such as:

  * `$res`
  * `$ret`
  * `$tmp`
  * `$arr`
  * `$obj`
  * `$val`
* Boolean variables should describe a condition, for example:

  * `$isActive`
  * `$hasPermission`
  * `$canUpdate`
* Collection variables should use plural names.
* Single objects should use singular names.
* Do not reuse the same variable for a different meaning or type.
* Do not overwrite an input parameter with a processed value.
* Declare variables close to where they are used.
* Use the same term consistently for the same business concept.
* Avoid boolean parameters when they make calls unclear.

Avoid:

```php
saveUser($user, true);
```

Prefer a method with explicit meaning:

```php
saveAndNotifyUser($user);
```

## Arrays

* Use short array syntax `[]`.
* Do not use `array()`.
* Associative arrays must use explicit and meaningful keys.
* Do not use numeric array positions to represent business fields.

Avoid:

```php
$user = [1001, 'Tom', true];
```

Preferred:

```php
$user = [
    'id' => 1001,
    'name' => 'Tom',
    'is_active' => true,
];
```

* Use a DTO or value object when an array structure becomes complex.
* Do not mix unrelated data types in one array without a documented structure.
* Check that a key exists before accessing uncertain input.
* Use `array_key_exists()` when the key may exist with a `null` value.
* Use `isset()` when the value must also be non-null.
* Do not suppress undefined-key errors with `@`.
* Avoid long chains of uncertain nested array access.
* Avoid nested `array_map()`, `array_filter()`, and `array_reduce()` calls.
* Do not use complex `array_reduce()` logic when a `foreach` loop is clearer.
* Split long array-processing chains into explicit intermediate steps.

## Methods

* Each method must have one clear responsibility.
* A method should generally not exceed 50 lines.
* A method should generally not accept more than four parameters.
* Use a DTO or parameter object when more parameters are required.
* Do not combine database access, business logic, logging, and response formatting in one method.
* Method names must clearly describe the action and target.
* Query methods must not modify data.
* Do not use global variables.
* Do not modify input objects unless the method name clearly indicates mutation.
* Do not return `false` as a generic business failure result.
* Use the project's existing exception or result-handling mechanism.
* Avoid methods that contain many unrelated branches.
* Extract complex conditions into a clearly named variable or method.

Example:

```php
$isEligibleUser = $user->isActive()
    && $user->hasPermission()
    && !$user->isBlocked();

if (!$isEligibleUser) {
    return;
}
```

## Classes and Dependencies

* Class properties should be `private` by default.
* Do not expose mutable public properties.
* Constructors should only assign dependencies and initialize simple state.
* Constructors must not perform database queries, HTTP requests, logging, or business logic.
* Avoid classes with too many unrelated responsibilities.
* Avoid deep inheritance hierarchies.
* Prefer composition over inheritance.
* Traits should only contain small, stateless, single-purpose behavior.
* Do not create broad utility classes containing unrelated static methods.
* Do not use service locators or global containers when constructor injection is already used.

## Exception Handling

* Do not use empty `catch` blocks.
* Do not catch and silently ignore exceptions.
* Catch only exceptions that the current layer can handle.
* Do not catch `Throwable` and return a successful response.
* Preserve the original exception when wrapping it.
* Do not expose database errors, SQL, stack traces, internal file paths, or internal class names to clients.
* Do not use exceptions for normal control flow.
* Do not log the same exception repeatedly at multiple layers.
* Follow the project's existing exception hierarchy and error-response format.
* Transactions must have a clear commit and rollback strategy.

## Database Access

* Use parameter binding for all external values.
* Do not concatenate user input into SQL.
* Do not use `SELECT *`.
* Explicitly list required columns.
* Do not execute database queries inside loops.
* Prefer batch queries and batch updates where appropriate.
* Every `UPDATE` and `DELETE` must have an explicit condition.
* Keep database transactions as small as possible.
* Do not make external HTTP requests inside a transaction.
* Do not perform slow file operations inside a transaction.
* Pagination must enforce a maximum page size.
* Dynamic column names and sorting fields must use an explicit whitelist.
* Always handle empty query results.
* Do not use floating-point values for monetary calculations.
* Represent money using the smallest currency unit as an integer or use the project's precise decimal type.

## Security

* Do not use `unserialize()` with untrusted data.
* Do not directly use user input as a file path.
* Validate uploaded file type, extension, size, and name.
* Escape HTML output where required.
* Do not execute shell commands using raw user input.
* Do not log sensitive data, including:

  * Passwords
  * Access tokens
  * Session IDs
  * Authentication headers
  * Full payment card numbers
  * Personal identification information
* Authorization must be checked on the server.
* Do not trust client-provided roles, permissions, prices, user IDs, or authorization results.
* Add idempotency or duplicate-request protection for sensitive operations where required.

## Comments

* Comments should explain why a decision was made.
* Do not write comments that merely repeat the code.
* Avoid end-of-line comments for complex logic.
* TODO comments must include a task ID, issue ID, or responsible person.

## Final Review

Before returning PHP code:

1. Check the code against all applicable rules in this file.
2. Check that the implementation follows nearby project patterns.
3. Confirm that no unrelated code was modified.
4. Identify any assumption made because project information was missing.
5. List any rule that could not be followed and explain why.
6. When all applicable checks pass, state:

`Coding standards check passed.`
