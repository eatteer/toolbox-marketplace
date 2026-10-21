---
name: aesthetic-code
description: Write readable, visually pleasant code - well spaced, with descriptive names, short functions, explicit logic and a clear structure, instead of dense, cramped blocks. Use this skill whenever you write, generate, refactor or review code in any language, even if the user doesn't explicitly ask for "clean code".
---

# Aesthetic code

The goal: someone opening the file for the first time should understand **what the code does**
within seconds, and the visual shape of the code should mirror its logical structure.

Code is read far more often than it is written. Optimize for the reader.

## 0. First, respect the project

Before applying any rule in this guide:

- If the project has a formatter or linter (Prettier, Black, Ruff, gofmt, rustfmt, ESLint...),
  its configuration wins. Don't fight it.
- Match the style of the surrounding code: naming conventions, comment density, error-handling
  patterns. A consistent file looks better than one that is "perfect" in patches.
- This guide fills in the gaps the project leaves undefined.

## 1. Vertical space: let the code breathe

The most common flaw in generated code is that everything is crammed together. Use blank lines
to separate **logical steps**, the same way paragraphs separate ideas in prose.

Bad:

```python
def process_order(order):
    if not order.items:
        raise ValueError("Empty order")
    subtotal = sum(i.price * i.quantity for i in order.items)
    discount = subtotal * 0.1 if order.customer.is_vip else 0
    total = subtotal - discount
    order.total = total
    order.status = "processed"
    db.save(order)
    send_email(order.customer.email, total)
    return order
```

Good:

```python
VIP_DISCOUNT = 0.10


def process_order(order):
    if not order.items:
        raise ValueError("Empty order")

    subtotal = sum(item.price * item.quantity for item in order.items)
    discount = subtotal * VIP_DISCOUNT if order.customer.is_vip else 0

    order.total = subtotal - discount
    order.status = "processed"

    db.save(order)

    send_email(order.customer.email, order.total)

    return order
```

### Within a step: separate by kind of statement

Lines that belong to the same business step can still be different *kinds* of code. Being
related is not a reason to glue them together. Inside a step, give each kind its own group:

- **Assignments and declarations**, including `value = call()`: what matters there is the value.
- **Standalone calls with side effects**: saving, sending, logging, emitting events.
- **Control-flow blocks** (`if`, `for`, `while`, `try`, `switch`): a blank line before and after.
- **`return` / `throw`** at the end of a function: a blank line before it, unless it is the only
  statement in its block.

Statements of the same kind stay together: three consecutive assignments are one group, and two
consecutive side-effect calls are one group.

Bad:

```javascript
async function registerUser(input) {
  const email = input.email.trim().toLowerCase();
  const existingUser = await usersRepository.findByEmail(email);
  if (existingUser) {
    throw new ConflictError(`Email already registered: ${email}`);
  }
  const passwordHash = await hashPassword(input.password);
  const user = await usersRepository.create({ email, passwordHash });
  await mailer.sendWelcome(user.email);
  logger.info("User registered", { userId: user.id });
  return user;
}
```

Good:

```javascript
async function registerUser(input) {
  const email = input.email.trim().toLowerCase();
  const existingUser = await usersRepository.findByEmail(email);

  if (existingUser) {
    throw new ConflictError(`Email already registered: ${email}`);
  }

  const passwordHash = await hashPassword(input.password);
  const user = await usersRepository.create({ email, passwordHash });

  await mailer.sendWelcome(user.email);
  logger.info("User registered", { userId: user.id });

  return user;
}
```

Rules:

- One blank line between logical blocks (validate → compute → persist → notify).
- Inside a block, one blank line between different kinds of statements (see above).
- Keep lines that belong together grouped; separate them from those that don't.
- Don't sprinkle blank lines randomly or after every other line: whitespace must mean something.
- Separate functions and classes according to the language's convention (e.g. 2 lines in Python).
- Avoid very long lines: if an expression doesn't fit comfortably (~80-100 characters),
  break it across lines with clear indentation.

## 2. Self-explanatory names

A good name removes the need for a comment.

| Avoid                    | Prefer                                 |
| ------------------------ | -------------------------------------- |
| `d`, `tmp`, `data2`      | `days_remaining`, `active_user`        |
| `process()`, `handle()`  | `calculate_taxes()`, `send_invoice()`  |
| `flag`, `check`          | `is_valid`, `has_permission`           |
| `list`, `arr`            | `customers`, `pending_invoices`        |
| `x = 86400`              | `SECONDS_PER_DAY = 86400`              |

- **Variables**: nouns that say what they hold. Collections are plural.
- **Functions**: verbs that say what they do (`get_`, `calculate_`, `validate_`, `create_`).
- **Booleans**: yes/no questions (`is_`, `has_`, `can_`, `should_`).
- **Constants**: no loose "magic" numbers or strings; give them a name.
- Name length should match scope: `i` is fine in a 3-line loop, not in a global variable.
- No cryptic abbreviations (`usr`, `cfg`, `mgr`) unless they are a project convention.
- Follow the language's convention: `snake_case` in Python, `camelCase` in JS/TS/Java,
  `PascalCase` for classes and types, and so on.

## 3. Short functions with a single responsibility

- Each function does **one thing**, and its name says which.
- If you need a comment like `# Step 2: validate...` inside a long function,
  that step probably deserves its own function.
- Few parameters (ideally ≤ 3). If there are more, group them into an object or struct.
- But don't split for the sake of splitting: three one-line functions that only call each other
  are worse than one clear ten-line function.

## 4. Flatten the code: early returns

Avoid the "pyramid" of nested `if`s. Handle special cases first and get out.

Bad:

```javascript
function getDiscount(user) {
  if (user) {
    if (user.active) {
      if (user.purchases > 10) {
        return 0.15;
      } else {
        return 0.05;
      }
    }
  }
  return 0;
}
```

Good:

```javascript
const FREQUENT_DISCOUNT = 0.15;
const BASE_DISCOUNT = 0.05;
const PURCHASES_TO_BE_FREQUENT = 10;

function getDiscount(user) {
  if (!user || !user.active) {
    return 0;
  }

  const isFrequentCustomer = user.purchases > PURCHASES_TO_BE_FREQUENT;

  return isFrequentCustomer ? FREQUENT_DISCOUNT : BASE_DISCOUNT;
}
```

- At most 2-3 levels of indentation per function. Beyond that, extract or invert conditions.
- Avoid `else` after a `return`.
- Complex conditions go into a named variable or function
  (`const canEdit = isAuthor || isAdmin`).

## 5. Explicit over implicit

Code should say exactly what it does. Don't make the reader decode shortcuts, operator tricks
or hidden conversions.

**No optional chaining or safe-navigation operators.** Write the check as a plain `if`.
This applies to every language that has them: `?.` in JavaScript/TypeScript, C#, Kotlin,
Swift and Dart, `&.` in Ruby, `?->` in PHP.

Bad:

```javascript
onSuccess?.(result);

const city = user?.address?.city;
```

Good:

```javascript
if (onSuccess) {
  onSuccess(result);
}

if (!user || !user.address) {
  return null;
}

const city = user.address.city;
```

Rules:

- Always use braces on `if`, `else`, `for` and `while`, even for a single statement.
- Don't use `&&` or `||` for control flow (`isReady && start()`); write an `if`.
- No nested ternaries. One simple ternary is fine; anything more becomes an `if`/`else`.
- Don't rely on implicit type coercion: use strict equality (`===`, `!==`) and explicit
  conversions (`Number(value)`, `String(value)`, `Boolean(value)`) instead of tricks
  like `+value`, `"" + value` or `!!value`.
- Prefer explicit types on function signatures when the language supports them
  (TypeScript, Python type hints, etc.).
- Idioms that the language's official style guide recommends are fine, e.g. `if not items:`
  in Python (PEP 8).

## 6. Comments: the why, not the what

- Don't comment what the code already says (`i += 1  # increment i`).
- Do comment non-obvious decisions, limitations, workarounds and the **why**.
- Prefer renaming or extracting a function over adding an explanatory comment.
- No commented-out code "just in case", and no comments narrating your process
  (`// now add the missing validation`).

## 7. File order and structure

- Predictable order: imports → constants → types → helper functions → main function/class
  (or the usual order for the language/project).
- Imports grouped (standard library, third-party, local modules), with unused ones removed.
- Keep related things close: a helper next to the code that uses it.
- Readable data structures: an object or list with many entries gets one entry per line,
  with a trailing comma if the language allows it.

```typescript
const config = {
  port: 3000,
  host: "localhost",
  maxRetries: 3,
  timeoutMs: 5_000,
};
```

## 8. Simplicity above all

- The simplest solution that works is usually the most beautiful one.
- Don't add abstractions, layers or patterns "in case we need them later".
- Don't duplicate logic: if you're copying a block for the third time, extract it.
- Consistency: if you do something one way in one place, do it the same way everywhere.

## Checklist before delivering code

- [ ] Are logical steps separated by blank lines?
- [ ] Within each step, are assignments, side-effect calls, control-flow blocks and the final
      `return` in separate groups?
- [ ] Is every name understandable without reading its implementation?
- [ ] Are there any unnamed magic numbers or strings?
- [ ] Does any function do more than one thing or nest more than 3 levels deep?
- [ ] Is the logic explicit: plain `if` checks instead of `?.`, braces everywhere, no coercion tricks?
- [ ] Do comments explain the why rather than the what?
- [ ] Does it respect the project's formatter and existing style?

For more examples across languages, see `references/examples.md`.
