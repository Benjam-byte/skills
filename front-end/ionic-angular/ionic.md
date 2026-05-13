# Ionic Rules

This document defines the Ionic-specific rules for the project.

It complements the architecture, design, and style rules.

The project uses:

- Ionic Angular
- Angular 20+
- Standalone components
- Capacitor for native features
- Tailwind CSS as the primary styling approach
- Per-component SCSS when necessary

## Objective

Use Ionic for what it does well:

- mobile structure;
- mobile navigation;
- mobile scroll;
- modals and overlays;
- accessibility;
- safe areas;
- touch components;
- Capacitor integration.

Do not use Ionic as a simple desktop web component library.

## Page Structure

A routed Ionic page must use the standard Ionic structure.

```html
<ion-page>
  <ion-header>
    <ion-toolbar>
      <ion-title>Page title</ion-title>
    </ion-toolbar>
  </ion-header>

  <ion-content>
    <main class="min-h-full p-4">
      <!-- Page content -->
    </main>
  </ion-content>
</ion-page>
```

Use `ion-page`, `ion-header`, `ion-toolbar`, `ion-content`, and `ion-footer` for fixed bottom actions.

Avoid:

- a page without `ion-page`;
- manual scroll with `overflow-auto` on the full page;
- unnecessary nested scrollable areas;
- desktop-first layouts;
- custom headers when `ion-header` is sufficient.

## Ionic Components

Use Ionic components when they bring useful mobile behavior. If a plain HTML element is clearer and lighter, use it instead.

Key components to use when appropriate: `ion-modal`, `ion-popover`, `ion-alert`, `ion-toast`, `ion-loading`, `ion-list`, `ion-item`, `ion-input`, `ion-select`, `ion-fab`, `ion-infinite-scroll`.

Import only the Ionic components actually used in each standalone component.

## Navigation

Use Angular Router with standalone routes.

```ts
export const routes: Routes = [
  {
    path: 'home',
    loadComponent: () =>
      import('./pages/home/home.page').then((m) => m.HomePage),
  },
];
```

Use `ion-router-outlet` once at the app root, or inside a tabs layout if the app uses tabs. Avoid nested `ion-router-outlet` without a real reason.

Do not create a feature module just to lazy load a page.

## Tabs

Use `ion-tabs` only for primary tab-based navigation.

```html
<ion-tabs>
  <ion-router-outlet />

  <ion-tab-bar slot="bottom">
    <ion-tab-button tab="home">
      <ion-icon name="home-outline" aria-hidden="true" />
      <ion-label>Home</ion-label>
    </ion-tab-button>

    <ion-tab-button tab="settings">
      <ion-icon name="settings-outline" aria-hidden="true" />
      <ion-label>Settings</ion-label>
    </ion-tab-button>
  </ion-tab-bar>
</ion-tabs>
```

Avoid too many tabs, long labels, tabs for secondary navigation, or primary actions in the tab bar.

## Actions

Use `ion-button` for primary actions.

```html
<ion-button
  type="button"
  expand="block"
  [disabled]="isDisabled()"
  (click)="confirm()"
>
  Confirm
</ion-button>
```

Buttons must have a comfortable touch target, a clear disabled state, visible focus, short text, and an `aria-label` if icon-only.

Use `ion-fab` only if the action is truly global or primary. Avoid hover as the only feedback.

## Lists

Use `ion-list` and `ion-item` for simple interactive lists.

```html
<ion-list>
  @for (item of items(); track item.id) {
    <ion-item button (click)="selectItem(item.id)">
      <ion-label>
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </ion-label>
    </ion-item>
  }
</ion-list>
```

For heavily custom visual lists, plain HTML with Tailwind may be preferable.

For long lists, use pagination, infinite scroll, progressive loading, or Angular CDK virtual scroll. Do not use `ion-virtual-scroll`.

## Forms

Use typed Reactive Forms with Ionic components.

```html
<form [formGroup]="form" (ngSubmit)="submit()">
  <ion-list>
    <ion-item>
      <ion-input label="Name" labelPlacement="stacked" formControlName="name" />
    </ion-item>
  </ion-list>

  <ion-button type="submit" expand="block" [disabled]="form.invalid">
    Save
  </ion-button>
</form>
```

Errors must be visible and understandable. Do not indicate an error by color alone.

## Modals and Overlays

Each complex modal must be a separate component:

```
components/edit-item-modal/
  edit-item-modal.component.ts
  edit-item-modal.component.html
  edit-item-modal.component.scss
```

The page controls the modal opening. The modal controls its internal content. Actions bubble up via `output()`.

Use `ion-toast` for short feedback, `ion-alert` for important confirmations, `ion-loading` for blocking operations, and inline states for non-blocking loading.

Avoid:

- large modals coded directly in the page;
- multiple stacked overlays;
- a modal that fills the screen without reason;
- a full-screen loading for a small local action;
- a toast for a critical error that requires user action.

## Theming

Use Ionic CSS variables to customize Ionic components.

```scss
ion-content {
  --background: var(--color-layer-bg);
}

ion-toolbar {
  --background: var(--color-layer-panel);
  --color: var(--color-text-primary);
}

ion-button {
  --border-radius: 0.875rem;
}
```

Use Tailwind for layout and simple styles. Use SCSS for Ionic variables, shadow DOM parts, animations, and complex shadows. See `style.md` for full styling rules.

## Safe Areas

Fixed areas must respect mobile safe areas.

Watch for: footer, tab bar, fixed buttons, HUDs, custom overlays, bottom actions.

```scss
.fixed-bottom-action {
  padding-bottom: calc(1rem + env(safe-area-inset-bottom));
}
```

Do not place important actions too close to the bottom edge.

## Capacitor

Use Capacitor for native features: camera, files, notifications, preferences, haptics, status bar, splash screen, app state.

Prefer official Capacitor plugins. Avoid Cordova and Ionic Native unless there is a legacy constraint.

Create one Angular service per native feature:

```
core/services/camera.service.ts
core/services/notification.service.ts
core/services/device.service.ts
```

Each service must handle: platform availability, web fallback when possible, native errors, permissions, and strict typing.

Never assume identical behavior across web, iOS, Android, PWA, and desktop browser. Isolate platform-specific logic in services and always provide a fallback or a clear error when a feature is unavailable.

## Capacitor Permissions

Request permissions only when they are actually needed, not at app launch.

- Explain to the user why the permission is needed.
- Handle denial gracefully.
- Provide a fallback when possible.
- Never keep an unused permission.