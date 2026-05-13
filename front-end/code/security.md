# Security Rules

This document defines front-end security rules.

The front-end is never a trusted zone.

## Environment

Use separate environments: `development`, `staging`, `production`.

Environment files may contain:

- public API URL;
- environment name;
- public feature flags;
- public configuration.

They must never contain:

- API secrets;
- private keys;
- JWT secrets;
- admin tokens;
- passwords;
- database connection strings;
- service role keys;
- server credentials.

Secrets must stay on the backend or in a secured server configuration.

## Front-End Is Public

Everything in the front-end can be read by the user. Never treat as secret:

- compiled JavaScript;
- front-end environment variables;
- API calls visible in the browser;
- assets, routes, or feature flags.

If information must not be public, it must not be in the front-end.

## API Security

All important security rules must be verified on the backend.

The front-end may improve UX with guards or display conditions, but it does not actually protect data. The backend must always verify: authentication, permissions, resource ownership, business limits, and access rights.

Never use a front-end guard as the only protection:

```ts
// Not sufficient on its own
if (user.role === "admin") {
  showAdminButton();
}
```

## Authentication

Do not store sensitive tokens unnecessarily.

Prefer:

- secure server-side sessions when possible;
- `HttpOnly`, `Secure`, `SameSite` cookies when the architecture allows;
- short-lived, controlled storage if a front-end token is required.

Avoid:

- long-lived tokens in `localStorage`;
- refresh tokens exposed to JavaScript;
- admin tokens on the front-end.

## Local Storage

`localStorage` must only contain non-sensitive data.

Accepted: UI preferences, theme, non-critical local state, public cache, non-sensitive progress.

Avoid: sensitive tokens, critical personal data, payment data, user permissions, secrets.

## User Input

Any data coming from the user must be treated as untrusted.

- Validate on the front-end for UX.
- Always validate again on the backend.
- Escape or avoid dynamic HTML.
- Never inject user content directly into the DOM.

Avoid:

```ts
element.innerHTML = userContent;
```

Prefer standard Angular binding:

```html
<p>{{ userContent }}</p>
```

## External Links

External links opened in a new tab must use:

```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  External link
</a>
```

## Dependencies

Add a dependency only if it is necessary.

Before adding a library:

- verify it is actively maintained;
- verify its actual usage in the project;
- avoid unknown packages for simple logic;
- keep dependencies up to date.

Do not add a heavy library for simple logic.

## Error Messages

Errors shown to the user must be understandable but must not expose sensitive details.

Avoid:

```
Database connection failed with user admin on host...
```

Prefer:

```
An error occurred. Please try again in a moment.
```

## Checklist

Before validating a front-end feature:

- No secrets are in the front-end.
- Permissions are verified on the backend.
- The front-end does not trust its own guards.
- Tokens are not logged.
- Sensitive data is not in `localStorage`.
- User input is not injected as raw HTML.
- External links use `rel="noopener noreferrer"`.
- Capacitor permissions are limited to what is needed.
- User-facing errors do not reveal sensitive technical details.