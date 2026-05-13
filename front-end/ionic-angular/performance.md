# Performance Rules

Ce document définit uniquement les règles performance essentielles du projet.

## Principe principal

Ne pas optimiser trop tôt, mais éviter les problèmes évidents.

Une page doit rester :

- rapide à charger ;
- fluide au scroll ;
- légère en DOM ;
- raisonnable en images ;
- simple à mettre à jour.

## Pages

Toutes les pages routées doivent être lazy-loaded.

À préférer :

```ts
export const routes: Routes = [
  {
    path: "home",
    loadComponent: () =>
      import("./pages/home/home.page").then((m) => m.HomePage),
  },
];
```

Ne pas importer toutes les pages dans le routing principal.

## Composants lourds

Ne pas charger un composant lourd s’il n’est pas visible ou utile immédiatement.

Utiliser @defer pour :

- section secondaire ;
- graphique ;
- panneau de détail ;
- composant sous la ligne de flottaison ;
- composant utilisant une librairie lourde.

```html
@defer (on viewport) {
<app-heavy-section />
} @placeholder {
<app-section-skeleton />
}
```

Ne pas utiliser @defer partout.

## Templates

Les templates doivent rester légers.

À faire :

- utiliser @for avec track ;
- utiliser computed() pour les valeurs dérivées ;
- utiliser @if pour ne pas rendre ce qui est inutile ;
- garder les composants en OnPush.

À éviter :

```html
<p>{{ calculateTotal(itemList()) }}</p>
```

À préférer :

```ts
protected readonly total = computed(() => {
  return this.itemList().reduce(
    (total, item) => total + item.value,
    0,
  );
});
```

```html
<p>{{ total() }}</p>
```

## Listes

Une liste longue ne doit pas tout rendre d’un coup.

Selon le besoin, utiliser :

- pagination ;
- chargement progressif ;
- ion-infinite-scroll ;
- Angular CDK virtual scroll pour les très grandes listes.

Toujours utiliser track :

```html
@for (item of itemList(); track item.id) {
<app-item-card [item]="item" />
}
```

Éviter track $index si la liste peut changer d’ordre.

Une UI répétée doit rester plus simple qu’une UI isolée.

Une carte de détail peut être riche.
Un item répété 100 fois doit rester léger.

## Images

Les images doivent être optimisées avant intégration.

Règles :

- préférer WebP ou AVIF quand possible ;
- définir width et height quand possible ;
- lazy loader les images secondaires ;
- éviter les images énormes dans les listes ;
- utiliser NgOptimizedImage pour les images statiques importantes.

```html
<img
  ngSrc="assets/images/example.webp"
  width="320"
  height="180"
  alt="Example"
/>
```

## Ion Content

ion-content doit rester fluide.

Éviter :

- trop de composants lourds directement dans ion-content ;
- plusieurs scrolls imbriqués ;
- de grosses shadows sur beaucoup d’items ;
- des animations permanentes dans une liste ;
- des modales lourdes montées en permanence.

À préférer :

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

## Animations et styles coûteux

Préférer animer :

- transform;
- opacity.

Éviter d’animer souvent :

- width;
- height;
- top;
- left;
- box-shadow;
- filter;
- backdrop-filter.

Les effets visuels lourds doivent être rares, surtout dans les listes.

## Données

Ne pas charger plus de données que nécessaire.

Préférer :

- pagination ;
- filtres côté API ;
- payloads limités ;
- cache simple si utile ;
- chargement à la demande.

Éviter :

- charger toute la base au démarrage ;
- garder plusieurs copies inutiles des mêmes données ;
- recalculer toute une liste pour une petite modification.

## Nettoyage

Nettoyer correctement :

- subscriptions manuelles ;
- timers ;
- listeners DOM ;
- listeners Capacitor ;
- observers ;
- animations JS.

Utiliser takeUntilDestroyed() quand une subscription manuelle est nécessaire.

## Checklist

Avant de valider une page :

- La page est lazy-loaded.
- Les composants lourds sont différés si possible.
- Le template ne contient pas de calcul coûteux.
- Les listes utilisent @for avec track.
- Les longues listes sont paginées ou chargées progressivement.
- Les images sont optimisées et dimensionnées.
- Le scroll dans ion-content reste fluide.
- Les animations sont simples.
- Les listeners et subscriptions sont nettoyés.
