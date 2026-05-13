# Performance Rules

This document defines the essential performance rules for the project.

## Core Principle

Do not optimize too early, but avoid obvious problems.

A page must remain:

- fast to load;
- smooth to scroll;
- light in DOM;
- reasonable in image weight;
- simple to update.

## Pages

All routed pages must be lazy-loaded.

```ts
export const routes: Routes = [
  {
    path: 'home',
    loadComponent: () =>
      import('./pages/home/home.page').then((m) => m.HomePage),
  },
];
```

Do not import all pages in the main routing file.

## Heavy Components

Do not load a heavy component if it is not immediately visible or useful.

Use `@defer` for:

- secondary sections;
- charts;
- detail panels;
- components below the fold;
- components using a heavy library.

```html
@defer (on viewport) {
  <app-heavy-section />
} @placeholder {
  <app-section-skeleton />
}
```

Do not use `@defer` everywhere.

## Templates

Templates must remain lightweight.

Avoid calling functions in templates:

```html
<!-- Avoid -->
<p>{{ calculateTotal(itemList()) }}</p>
```

Prefer computed signals:

```ts
protected readonly total = computed(() => {
  return this.itemList().reduce((total, item) => total + item.value, 0);
});
```

```html
<p>{{ total() }}</p>
```

Always use `@for` with `track`. Keep components on `OnPush`. Use `@if` to avoid rendering what is not needed.

## Lists

A long list must not render everything at once.

Use pagination, progressive loading, `ion-infinite-scroll`, or Angular CDK virtual scroll for very large lists.

Always use `track`:

```html
@for (item of itemList(); track item.id) {
  <app-item-card [item]="item" />
}
```

Avoid `track $index` if the list can change order.

A repeated UI element must be simpler than a standalone one. A detail card can be rich. An item repeated 100 times must stay light.

## Images

Images must be optimized before integration.

- Prefer WebP or AVIF when possible.
- Define `width` and `height` when possible.
- Lazy load secondary images.
- Avoid large images in lists.
- Use `NgOptimizedImage` for important static images.

```html
<img
  ngSrc="assets/images/example.webp"
  width="320"
  height="180"
  alt="Example"
/>
```

## Bundle Size

Do not import an entire library for a single function.

Avoid:

```ts
import { debounce } from 'lodash';
```

Prefer:

```ts
import debounce from 'lodash/debounce';
```

Before adding a library, verify it is necessary and check its impact on bundle size.

## Ion Content

`ion-content` must remain smooth.

Avoid heavy components directly inside `ion-content`, nested scrolls, heavy shadows on many items, permanent animations in lists, and heavy modals mounted permanently.

Prefer:

```html
<ion-content>
  <main class="min-h-full p-4">
    <app-page-header />
    <app-main-section />

    @defer (on viewport) {
      <app-secondary-section />
    }
  </main>
</ion-content>
```

## Animations

Prefer animating `transform` and `opacity`.

Avoid animating frequently: `width`, `height`, `top`, `left`, `box-shadow`, `filter`, `backdrop-filter`.

Heavy visual effects must be rare, especially in lists.

## Data

Do not load more data than necessary.

Prefer pagination, server-side filters, limited payloads, simple caching when useful, and on-demand loading.

Avoid loading the entire dataset at startup, keeping multiple unnecessary copies of the same data, or recalculating a full list for a small change.

## Cleanup

Clean up properly:

- manual subscriptions;
- timers;
- DOM listeners;
- Capacitor listeners;
- observers;
- JS animations.

Use `takeUntilDestroyed()` when a manual subscription is necessary.

## Checklist

Before validating a page:

- The page is lazy-loaded.
- Heavy components are deferred when possible.
- The template contains no costly calculations.
- Lists use `@for` with `track`.
- Long lists are paginated or progressively loaded.
- Images are optimized and sized.
- Scroll inside `ion-content` remains smooth.
- Animations are simple.
- Listeners and subscriptions are cleaned up.