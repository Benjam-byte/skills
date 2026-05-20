# Ionic Angular Architecture Rules

This document defines the structure and architecture rules for the project.

The project uses:

- Angular 20+
- Ionic Angular
- TypeScript strict
- Standalone components
- Signals
- Tailwind CSS
- Per-component SCSS

The goal is to keep the code simple, maintainable, mobile-first, and easy to evolve.

## General Principles

- Use modern Angular: `signal()`, `computed()`, `input()`, `output()`, `inject()`.
- Use standalone components only.
- Do not create `NgModule`.
- Use `ChangeDetectionStrategy.OnPush` on all components.
- Keep components small and focused.
- Avoid pages that grow too large.
- Put business logic in services or utility files.
- Put purely UI logic in components.
- Avoid unnecessary abstractions.
- Prefer simple, readable code over clever code.


## Pages

A page must primarily:

- compose child components;
- handle global orchestration;
- connect the necessary services;
- manage simple page states: loading, error, empty, content.

A page must not:

- contain too much business logic;
- contain multiple large visual sections directly in its template;
- contain complex modals inline;
- contain repeatable business calculations.

If a page grows too long, extract components.

## Components

Create a separate component for:

- a card;
- a list item;
- a complex section;
- a modal;
- a form;
- a repeated block;
- an SVG;
- a feature header;
- an action bar;
- a stats panel.

Each component must have a single responsibility.

If a component exceeds approximately 150 lines or contains multiple visual responsibilities, split it.

## Angular Syntax

Always use:

```ts
input()
output()
signal()
computed()
inject()
```

Avoid:

```ts
@Input()
@Output()
constructor injection
```

Example:

```ts
import { ChangeDetectionStrategy, Component, computed, input, output } from '@angular/core';

@Component({
  selector: 'app-resource-card',
  templateUrl: './resource-card.component.html',
  styleUrl: './resource-card.component.scss',
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class ResourceCardComponent {
  readonly name = input.required<string>();
  readonly value = input.required<number>();
  readonly selected = input(false);

  readonly selectResource = output<void>();

  protected readonly formattedValue = computed(() => {
    return this.value().toLocaleString('fr-FR');
  });

  protected onSelect(): void {
    this.selectResource.emit();
  }
}
```

## Angular Templates

Use the new Angular control flow:

```html
@if (isLoading()) {
  <app-loader />
} @else if (hasError()) {
  <app-error-state />
} @else {
  <app-content />
}
```

```html
@for (item of itemList(); track item.id) {
  <app-item-card [item]="item" />
}
```

Avoid `*ngIf`, `*ngFor`, `*ngSwitch`.

Keep templates simple. Do not put complex logic in HTML.

## State Management

Use signals for:

- local component state;
- simple UI state;
- selection;
- modal open/close;
- derived values with `computed()`.

Use RxJS for:

- API calls;
- shared async streams;
- external events;
- websockets;
- complex streams.

Do not use RxJS for a simple local boolean.

Update signals with `set()` or `update()`:

```ts
this.items.update(items => [...items, newItem]);
this.selectedId.set(id);
```

## Services

Services must have a clear, domain-based responsibility. Name them accordingly: `user.service.ts`, `auth.service.ts`, `camera.service.ts`. A service must not become a god service.

Prefer:

```ts
@Injectable({ providedIn: 'root' })
export class ImportService {
  private readonly api = inject(ApiService);
}
```

Avoid constructor injection.