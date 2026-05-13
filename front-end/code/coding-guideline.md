# Coding Guidelines

This document defines the general rules for writing code.

The goal is to keep code readable, explicit, and simple to maintain.

## Core Principles

1. Don't assume. Don't hide confusion. Surface tradeoffs.
2. Minimum code that solves the problem. Nothing speculative.
3. Touch only what you must. Clean up only your own mess.
4. Define success criteria. Loop until verified.

## Naming

Code must be easy to understand without effort.

Prefer:

- explicit names;
- short functions;
- clear responsibilities;
- simple logic;
- few comments, but useful ones.

Avoid:

- abbreviations;
- names that are too short;
- functions that do too many things;
- comments that repeat the code;
- premature abstractions.

Always prefer a full word over a single letter.

Avoid:

```ts
for (const i of words) {
  // ...
}
```

Prefer:

```ts
for (const index of wordList) {
  // ...
}
```

## Variables

Variables must explain what they contain.

Avoid:

```ts
const data = getUser();
const result = calculateScore();
const temp = value + 1;
```

Prefer:

```ts
const user = getUser();
const finalScore = calculateScore();
const nextLevel = currentLevel + 1;
```

Avoid generic names unless the context makes them obvious.

Names to avoid when possible: `data`, `result`, `value`, `temp`, `obj`, `arr`, `el`, `e`, `x`, `y`, `i`, `j`.

Exceptions: `event`, `error`, `index`.

### Arrays

An array must be named with the `List` suffix.

Prefer:

```ts
const userList: User[] = [];
const selectedItemList: Item[] = [];
const visibleCardList: Card[] = [];
```

Avoid:

```ts
const users: User[] = [];
const selectedItems: Item[] = [];
const cards: Card[] = [];
```

For a single item, use the singular name:

```ts
const user: User = userList[0];
const selectedItem: Item = selectedItemList[0];
```

## Booleans

Booleans must start with a clear prefix. This also limits ternary usage.

Recommended prefixes: `is`, `has`, `can`, `should`, `was`, `will`.

Prefer:

```ts
const isOpen = signal(false);
const hasError = computed(() => errorMessage() !== null);
const canSubmit = computed(() => form.valid);
const shouldShowModal = signal(false);
```

Avoid:

```ts
const open = signal(false);
const error = signal(false);
const submit = computed(() => form.valid);
```

## Functions

A function must do one thing.

The name must start with a verb.

Common prefixes: `get`, `set`, `create`, `update`, `delete`, `remove`, `find`, `filter`, `sort`, `build`, `format`, `calculate`, `validate`, `toggle`, `open`, `close`, `handle`.

Prefer:

```ts
function calculateTotalPrice(itemList: Item[]): number {
  return itemList.reduce((totalPrice, item) => totalPrice + item.price, 0);
}
```

Avoid:

```ts
function total(items: Item[]): number {
  return items.reduce((t, i) => t + i.price, 0);
}
```

## Event Handlers

Methods called directly from the template must be prefixed with `on`.

```ts
protected onSubmit(): void {
  this.submitForm();
}

protected onModalClose(): void {
  this.isModalOpen.set(false);
}

protected onItemSelect(itemId: string): void {
  this.selectedItemId.set(itemId);
}
```

Avoid putting too much logic in a handler. The handler can call a more specific private method.

## Private Methods

Private methods must remain simple and explicit. Any method not used outside the component must be declared `private`. Private methods are declared at the end of the file, after public ones.

```ts
private buildUserPayload(): UserPayload {
  return {
    name: this.name(),
    email: this.email(),
  };
}
```

Avoid overly generic private methods:

```ts
private processData(): void {}
private handleStuff(): void {}
```

## Types

Avoid `any`.

Use `unknown` if the type is truly uncertain.

Prefer:

```ts
function parseApiResponse(response: unknown): User | null {
  if (!isUser(response)) {
    return null;
  }
  return response;
}
```

Avoid:

```ts
function parseApiResponse(response: any): User {
  return response;
}
```

## Conditions

Prefer readable conditions.

Avoid:

```ts
if (!user || !user.isActive || user.score < 10 || user.status !== "ready") {
  return;
}
```

Prefer:

```ts
const canStartGame =
  user !== null && user.isActive && user.score >= 10 && user.status === "ready";

if (!canStartGame) {
  return;
}
```

## Early Returns

Use early returns to avoid deeply nested blocks.

Avoid:

```ts
function submit(): void {
  if (this.form.valid) {
    if (!this.isLoading()) {
      this.save();
    }
  }
}
```

Prefer:

```ts
function submit(): void {
  if (this.form.invalid) {
    return;
  }

  if (this.isLoading()) {
    return;
  }

  this.save();
}
```

## Comments

Do not comment every line. Never use `TODO`, `FIXME`, or other "future" comments.

A comment must explain *why* the code exists, not *what* it does.

Add a comment only to explain:

- a hack;
- an Ionic constraint;
- a browser constraint;
- a non-obvious value;
- an important UI decision;
- a complex business rule;
- a technical limitation;
- a temporary workaround.

Avoid:

```ts
// Set loading to true
this.isLoading.set(true);
```

Prefer:

```ts
// Required because ion-content creates its own scroll container.
this.maxModalHeight.set("70dvh");
```

Keep comments short and useful:

```ts
// Required because Safari does not support this behavior consistently.
```

## Error Handling

Errors must be explicit. Only add error handling where it is specified or requested. Do not invent error cases that are not defined by the business. Ask if an error seems obvious but is not specified.

Avoid:

```ts
catch (error) {
  console.log(error);
}
```

Prefer:

```ts
catch (error: unknown) {
  this.errorMessage.set('Unable to save settings.');
  console.error('Settings save failed', error);
}
```

Never silently ignore an error.

## Magic Values

Avoid magic values.

Avoid:

```ts
if (score > 100) {
  return "gold";
}
```

Prefer:

```ts
const goldScoreThreshold = 100;

if (score > goldScoreThreshold) {
  return "gold";
}
```

## Final Checklist

Before validating code:

- Names are explicit.
- Arrays use the `List` suffix.
- No single-letter variable names.
- Functions have a clear verb.
- Booleans start with `is`, `has`, `can`, or `should`.
- Complex conditions are extracted into readable variables.
- Functions remain short.
- Comments explain a real reason.
- No abstraction created "for later".
- Errors are only added at business-relevant points.