# Ionic Rules

Ce document définit les règles spécifiques à Ionic dans le projet.

Il complète les règles Angular, architecture, design et CSS.

Le projet utilise :

- Ionic Angular
- Angular 20+
- Standalone components
- Capacitor pour les fonctionnalités natives
- Tailwind CSS en priorité
- SCSS par composant quand nécessaire

---

## Objectif

Utiliser Ionic pour ce qu’il fait bien :

- structure mobile ;
- navigation mobile ;
- scroll mobile ;
- modales et overlays ;
- accessibilité ;
- safe areas ;
- composants tactiles ;
- intégration Capacitor.

Ne pas utiliser Ionic comme une simple librairie de composants web desktop.

---

## Structure de page

Une page Ionic doit utiliser la structure Ionic standard.

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

Utiliser :

ion-page pour chaque page routée ;
ion-header pour l’en-tête de page ;
ion-toolbar pour la barre supérieure ;
ion-content pour le scroll principal ;
ion-footer pour les actions fixes en bas d’écran.

Éviter :

- une page sans ion-page ;
- un scroll manuel avec overflow-auto sur toute la page ;
- plusieurs zones scrollables inutiles ;
- des layouts desktop-first ;
- des headers custom si ion-header suffit.

## Composants Ionic à privilégier

Utiliser les composants Ionic quand ils apportent un comportement mobile utile.

Composants recommandés :

ion-page
ion-header
ion-toolbar
ion-title
ion-content
ion-footer
ion-button
ion-icon
ion-modal
ion-popover
ion-list
ion-item
ion-input
ion-select
ion-textarea
ion-toggle
ion-checkbox
ion-radio
ion-alert
ion-toast
ion-loading

Ne pas utiliser un composant Ionic juste parce qu’il existe.

Si un simple élément HTML est plus clair et plus léger, l’utiliser.

## Imports Ionic

Importer uniquement les composants Ionic nécessaires dans le composant standalone.

## Navigation

Utiliser Angular Router avec des routes standalone.

Préférer :

```ts
export const routes: Routes = [
  {
    path: "home",
    loadComponent: () =>
      import("./pages/home/home.page").then((m) => m.HomePage),
  },
];
```

Utiliser ion-router-outlet au niveau adapté :

- une fois à la racine de l’app ;
- dans un layout tabs si l’app utilise des tabs ;
- éviter les ion-router-outlet imbriqués sans vraie raison.

Ne pas créer de feature module uniquement pour lazy loader une page.

## Tabs

Utiliser ion-tabs uniquement pour une navigation principale par onglets.

Structure recommandée :

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

Les tabs doivent rester simples et accessibles.

Éviter :

trop d’onglets ;
des labels trop longs ;
des tabs pour une navigation secondaire ;
des actions principales dans la tab bar.
Actions

Utiliser ion-button pour les actions principales.

Les boutons doivent :

avoir une zone tactile confortable ;
avoir un état disabled clair ;
avoir un focus visible ;
avoir un texte court ;
avoir un aria-label s’ils sont icon-only.

Exemple :

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

Pour les actions flottantes, utiliser ion-fab seulement si l’action est réellement globale ou principale.

Éviter :

trop de boutons flottants ;
des boutons minuscules ;
des actions importantes uniquement sous forme d’icône ;
des effets hover comme seul feedback.
Lists

Utiliser ion-list et ion-item pour les listes interactives simples.

Exemple :

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

Pour une liste très custom visuellement, un layout HTML + Tailwind peut être préférable.

Pour les longues listes, éviter de tout rendre d’un coup.

Utiliser plutôt :

- pagination ;
- infinite scroll ;
- chargement progressif ;
- Angular CDK virtual scroll si nécessaire.

Ne pas utiliser ion-virtual-scroll.

## Forms

Utiliser les Reactive Forms typés avec les composants Ionic.

Composants recommandés :

ion-input
ion-textarea
ion-select
ion-checkbox
ion-toggle
ion-radio-group

Exemple :

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

Les erreurs doivent être visibles et compréhensibles.

Ne pas afficher une erreur uniquement par couleur.

## Modals et overlays

Utiliser les overlays Ionic pour :

- modales ;
- popovers ;
- alerts ;
- toasts ;
- loading states ;
- action sheets.

Chaque modale complexe doit être un composant séparé.

Exemple :
components/edit-item-modal/
edit-item-modal.component.ts
edit-item-modal.component.html
edit-item-modal.component.scss

La page contrôle l’ouverture de la modale.

La modale contrôle son contenu interne.

Les actions remontent avec output().

Éviter :

- une grosse modale codée directement dans la page ;
- plusieurs overlays empilés ;
- une modale qui prend tout l’écran sans raison ;
- un absolute inset-0 si Ionic gère déjà l’overlay.

## Toasts, alerts et loading

Utiliser :

ion-toast pour un feedback court ;
ion-alert pour une confirmation importante ;
ion-loading pour une opération bloquante ;
un état inline pour les chargements non bloquants.

Ne pas afficher un loading plein écran pour une petite action locale.

Ne pas utiliser un toast pour une erreur critique qui demande une action utilisateur.

## Theming Ionic

Utiliser les variables Ionic quand il faut modifier un composant Ionic.

Exemple :

```css
:host {
  --page-padding: 1rem;
}

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

Utiliser Tailwind pour le layout et les styles simples.

Utiliser SCSS pour :

- variables Ionic ;
- shadow DOM parts ;
- animations ;
- shadows complexes ;
- styles natifs difficiles à faire avec Tailwind.

Ne pas utiliser de style inline.

## Tailwind avec Ionic

Tailwind est prioritaire pour :

- spacing ;
- layout ;
- flex/grid ;
- typo ;
- couleurs du projet ;
- bordures simples ;
- responsive.

Exemple :

```html
<ion-content>
  <main class="min-h-full space-y-4 p-4">
    <section class="rounded-2xl border border-layer-border bg-layer-panel p-4">
      <h1 class="font-title text-xl text-text-primary">Title</h1>

      <p class="mt-2 text-sm text-text-secondary">Description</p>
    </section>
  </main>
</ion-content>
```

Éviter :

- de tout styliser en SCSS ;
- les classes Tailwind arbitraires si un token existe ;
- les styles inline ;
- les wrappers inutiles autour des composants Ionic.

## Safe areas

Les zones fixes doivent respecter les safe areas mobiles.

À surveiller :

- footer ;
- tab bar ;
- boutons fixes ;
- HUD ;
- overlays custom ;
- actions en bas d’écran.

Exemple :

```css
.fixed-bottom-action {
  padding-bottom: calc(1rem + env(safe-area-inset-bottom));
}
```

Ne pas placer une action importante trop proche du bord bas.

## Capacitor

Utiliser Capacitor pour les fonctionnalités natives.

Exemples :

- caméra ;
- fichiers ;
- notifications ;
- préférences ;
- haptics ;
- status bar ;
- splash screen ;
- app state.

Préférer les plugins Capacitor officiels quand ils existent.

Éviter Cordova et Ionic Native sauf contrainte legacy.

Créer un service Angular par fonctionnalité native.

Exemple :

core/services/camera.service.ts
core/services/notification.service.ts
core/services/device.service.ts

Le service doit gérer :

- disponibilité de la plateforme ;
- fallback web si possible ;
- erreurs natives ;
- permissions ;
- typage strict.
- Platform differences

Ne jamais supposer que le comportement est identique sur :

- web ;
- iOS ;
- Android ;
- PWA ;
- navigateur desktop.

Quand une fonctionnalité dépend de la plateforme :

- isoler la logique dans un service ;
- fournir un fallback ;
- afficher une erreur claire si non disponible ;
- tester sur un vrai appareil quand possible.

## Capacitor Security

Les permissions natives doivent être limitées au strict nécessaire.

À faire :

- demander une permission seulement au moment où elle est utile ;
- expliquer pourquoi la permission est demandée ;
- gérer le refus ;
- prévoir un fallback si possible.

À éviter :

- demander toutes les permissions au lancement ;
- garder une permission inutile ;
- supposer que la permission est toujours accordée.
