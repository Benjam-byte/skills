# Ionic Angular Architecture Rules

Ce document définit les règles de structure et d’architecture du projet.

Le projet utilise :

- Angular 20+
- Ionic Angular
- TypeScript strict
- Standalone components
- Signals
- Tailwind CSS
- SCSS par composant

Le but est de garder le code simple, maintenable, mobile-first et facile à faire évoluer.

## Principes généraux

- Utiliser Angular moderne : `signal()`, `computed()`, `input()`, `output()`, `inject()`.
- Utiliser des standalone components uniquement.
- Ne pas créer de `NgModule`.
- Utiliser `ChangeDetectionStrategy.OnPush` sur tous les composants.
- Garder les composants petits et ciblés.
- Éviter les pages trop grosses.
- Mettre la logique métier dans des services ou fichiers utilitaires.
- Mettre la logique purement UI dans les composants.
- Éviter les abstractions inutiles.
- Préférer du code simple et lisible au code trop clever.

## Structure des dossiers

## Structure des dossiers

Utiliser une structure claire avec `core` et `pages`.

- `core` contient le code global / partagé de l’application.
- `pages` contient les écrans de l’application et leurs composants spécifiques.

```txt
src/app/
  core/
    services/
    guards/
    models/
    utils/
    components/
      app-button/
        app-button.component.ts
        app-button.component.html
        app-button.component.scss

      empty-state/
        empty-state.component.ts
        empty-state.component.html
        empty-state.component.scss

    directives/
    pipes/
    models/
    utils/

  pages/
    home/
      home.page.ts
      home.page.html
      home.page.scss

      components/
        page-header/
          page-header.component.ts
          page-header.component.html
          page-header.component.scss

        content-card/
          content-card.component.ts
          content-card.component.html
          content-card.component.scss

        action-panel/
          action-panel.component.ts
          action-panel.component.html
          action-panel.component.scss

    detail/
      detail.page.ts
      detail.page.html
      detail.page.scss

      components/
        detail-header/
          detail-header.component.ts
          detail-header.component.html
          detail-header.component.scss

        detail-section/
          detail-section.component.ts
          detail-section.component.html
          detail-section.component.scss

        detail-modal/
          detail-modal.component.ts
          detail-modal.component.html
          detail-modal.component.scss
```

## Pages

Une page doit principalement :

- composer les composants enfants
- gérer l’orchestration globale
- connecter les services nécessaires
- gérer les états de page simples : loading, error, empty, content.

Une page ne doit pas :

- contenir trop de logique métier
- contenir plusieurs grosses sections visuelles directement dans son template
- contenir des modales complexes directement
- contenir des calculs métier répétables.

Si une page devient trop longue, extraire des composants.

## Composants

Créer un composant séparé pour :

- une carte
- un item de liste
- une section complexe
- une modale
- un formulaire
- un bloc répété
- un SVG
- un header de feature
- une barre d’action
- un panneau de statistiques

Chaque composant doit avoir une seule responsabilité.

Si un composant dépasse environ 150 lignes ou contient plusieurs responsabilités visuelles, il faut le découper.

## Syntaxe Angular

Toujours utiliser :

input()
output()
signal()
computed()
inject()

Éviter :

@Input()
@Output()
constructor injection

Exemple :

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

## Templates Angular

Utiliser le nouveau control flow Angular :

@if (isLoading()) {
<app-loader />
} @else if (hasError()) {
<app-error-state />
} @else {
<app-content />
}

Utiliser :

@for (item of items(); track item.id) {
<app-item-card [item]="item" />
}

Éviter :

*ngIf
*ngFor
\*ngSwitch

Garder les templates simples.

Ne pas mettre de logique complexe dans le HTML.

State management

Utiliser les signals pour :

- état local de composant
- état UI simple
- sélection
- ouverture/fermeture de modale
- valeur dérivée avec computed().

Utiliser RxJS pour :

- appels API
- flux asynchrones partagés
- événements externes
- websocket
- streams complexes.

Ne pas utiliser RxJS pour un simple booléen local.

Ne pas modifier un signal avec mutate.

Utiliser :

this.items.update(items => [...items, newItem]);
this.selectedId.set(id);
Services

es services doivent avoir une responsabilité claire.

Exemples :

user.service.ts
auth.service.ts
storage.service.ts
api.service.ts
notification.service.ts
settings.service.ts
data-sync.service.ts
image-loader.service.ts
cache.service.ts
navigation.service.ts

Un service ne doit pas devenir un “god service”.

Préférer :

@Injectable({ providedIn: 'root' })
export class ImportService {
private readonly api = inject(ApiService);
}

Éviter :

constructor(private api: ApiService) {}

```

```
