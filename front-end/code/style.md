# Style Rules

This document defines the rules for writing Tailwind, CSS, and SCSS in the project.

The goal is to keep styles simple, useful, readable, and maintainable.

## Core Principle

Do not write CSS if it is not necessary.

Every style rule must have a real purpose:

- improve readability;
- improve usability;
- clarify a visual state;
- fix a real UI problem;
- avoid actual duplication;
- handle a case that Tailwind cannot do cleanly.

Never write inline styles in HTML.

Do not add styles "just in case". If removing a rule changes nothing visually or functionally, it must not exist.

## Tailwind First

Use Tailwind as the first option for:

- layout;
- spacing;
- flex;
- grid;
- sizing;
- simple typography;
- colors via tokens;
- simple borders;
- border radius;
- responsive;
- simple states.

Example:

```html
<section class="rounded-2xl border border-layer-border bg-layer-panel p-4">
  <h2 class="font-title text-xl text-text-primary">Title</h2>
  <p class="mt-2 text-sm text-text-secondary">Description</p>
</section>
```

Do not create a CSS class if a clear Tailwind class is sufficient.

## When to Use SCSS

Use SCSS only for:

- complex shadows;
- animations;
- textures;
- native element styles;
- Ionic variables;
- pseudo-elements;
- reusable styles that are too noisy to read in Tailwind;
- cases where Tailwind makes the HTML hard to read.

Example:

```scss
.panel-shadow {
  box-shadow:
    inset 0 1px 2px rgb(255 255 255 / 0.16),
    inset 0 -3px 4px rgb(0 0 0 / 0.38),
    0 3px 0 var(--color-layer-border),
    0 5px 7px rgb(0 0 0 / 0.55);
}
```

```html
<section class="panel-shadow rounded-2xl border border-layer-border bg-layer-panel p-4">
  ...
</section>
```

## Forbidden Styles

Never use:

- inline styles;
- `!important`;
- ID selectors;
- generic class names like `.box`, `.wrapper`, `.style1`;
- hardcoded colors if a token exists;
- unnecessary decorative animations;
- overly deep selectors;
- CSS added for a hypothetical future need;
- `em` or `rem` in CSS files;
- BEM notation.

## Angular Templates

Avoid `ngStyle`.

Avoid `ngClass` if a simple class binding is sufficient.

Prefer:

```html
<button
  class="rounded-xl px-4 py-3"
  [class.opacity-50]="disabled()"
  [class.pointer-events-none]="disabled()"
>
  Save
</button>
```

Avoid:

```html
<button [ngClass]="{ disabled: disabled() }">Save</button>
```

## File Organisation

Use one SCSS file per component.

```
example-card/
  example-card.component.ts
  example-card.component.html
  example-card.component.scss
```

SCSS files must remain short. If a file grows long, check whether:

- the component is doing too many things;
- a section should become a child component;
- a class is unnecessary;
- a style can be replaced by Tailwind.

Avoid large global files.

Global styles are reserved for:

- tokens;
- fonts;
- minimal reset;
- truly shared utilities;
- global keyframes that are actually reused.

## CSS Class Naming

Use kebab-case. No BEM notation. Limit nesting as much as possible.

Prefer:

```css
.player-card {}
.is-active {}
.has-error {}
```

Avoid:

```css
.playerCard {}
.blue-box {}
.style1 {}
```

Name by role, not by appearance.

Prefer: `.action-panel`  
Avoid: `.brown-box`

Allowed state prefixes: `.is-active`, `.is-disabled`, `.has-error`, `.has-badge`.

## Colors and Theming

Always use the project's tokens.

Prefer:

```html
<div class="bg-layer-panel text-text-primary border-layer-border"></div>
```

Avoid:

```html
<div class="bg-[#3f2818] text-[#fff3d0]"></div>
```

In SCSS, use existing variables:

```scss
.card {
  background: var(--color-layer-panel);
  border-color: var(--color-layer-border);
  color: var(--color-text-primary);
}
```

Do not hardcode a color if it already exists in the theme. New colors must be added to the theme only if they are reused or serve a real UI role.

Components must work with both light and dark themes through tokens. Never assume a fixed background color.

## Tailwind Arbitrary Values

Avoid arbitrary values when a standard class exists.

Avoid:

```html
<div class="rounded-[16px] p-[16px]"></div>
```

Prefer:

```html
<div class="rounded-2xl p-4"></div>
```

Arbitrary values are acceptable for:

- very specific dimensions;
- complex visual effects;
- Ionic integration;
- cases where no token matches.

## Units

- Use Tailwind for the majority of sizes.
- Use tokens or variables when possible.
- Use `px` for precise visual details.
- Use `%`, `vh`, `dvh`, `calc()` only when the layout requires it.
- Never use `em` or `rem` in CSS files.

Avoid unexplained magic values.

## Layout

Prefer flex and grid. Avoid fixed sizes if the content can be fluid.

Avoid:

```css
.card {
  width: 320px;
  height: 180px;
}
```

Prefer:

```html
<section class="w-full min-h-44 rounded-2xl p-4"></section>
```

Use `absolute` only if the layout cannot be done cleanly with flex or grid.

## SCSS Nesting

Limit nesting. Maximum recommended: 2 levels.

Avoid:

```scss
.card {
  .header {
    .title {
      span {
        color: var(--color-text-primary);
      }
    }
  }
}
```

Prefer:

```scss
.card-title {
  color: var(--color-text-primary);
}
```

## Accessibility

Prefer native HTML elements when they exist.

Prefer:

```html
<progress value="40" max="100"></progress>
```

Avoid:

```html
<div role="progressbar"></div>
```

Icon-only buttons must have an `aria-label`. Focus must remain visible. Contrast must remain sufficient.

## Checklist Before Adding Style

Before adding a CSS class or SCSS rule:

- Does Tailwind suffice?
- Does this rule genuinely improve the UI?
- Does this rule improve readability?
- Does this rule improve usability?
- Does this rule fix a real problem?
- Will this rule be easy to understand later?

If the answer is no, do not add it.

## Final Rule

Style must serve the interface, not fill files.

Tailwind first. SCSS only when it adds real value.