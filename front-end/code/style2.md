# Style Rules

Ce document définit les règles d’écriture Tailwind, CSS et SCSS du projet.

Le but est de garder les styles simples, utiles, lisibles et maintenables.

## Principe principal

Ne pas écrire de CSS si ce n’est pas nécessaire.

Chaque règle de style doit avoir une vraie utilité :

- améliorer la lisibilité ;
- améliorer l’utilisabilité ;
- clarifier un état visuel ;
- corriger un vrai problème d’UI ;
- éviter une duplication réelle ;
- Ne jamais écrire de style inline dans le HTML.
- gérer un cas que Tailwind ne fait pas proprement.

Ne pas ajouter de style “au cas où”.

Si retirer une règle ne change rien visuellement ou fonctionnellement, elle ne doit pas exister.

## Priorité Tailwind

Utiliser Tailwind en priorité pour :

- layout ;
- spacing ;
- flex ;
- grid ;
- taille ;
- typographie simple ;
- couleurs via tokens ;
- borders simples ;
- radius ;
- responsive ;
- états simples.

Exemple :

```html
<section class="rounded-2xl border border-layer-border bg-layer-panel p-4">
  <h2 class="font-title text-xl text-text-primary">Title</h2>

  <p class="mt-2 text-sm text-text-secondary">Description</p>
</section>
```

Ne pas créer une classe CSS si une classe Tailwind claire suffit.

## Quand utiliser du SCSS

Utiliser le SCSS uniquement pour :

- shadows complexes ;
- animations ;
- textures ;
- styles natifs ;
- variables Ionic ;
- pseudo-éléments ;
- styles réutilisables difficiles à lire en Tailwind ;
- cas où Tailwind rend le HTML trop bruyant.

Exemple :

```css
.panel-shadow {
  box-shadow:
    inset 0 1px 2px rgb(255 255 255 / 0.16),
    inset 0 -3px 4px rgb(0 0 0 / 0.38),
    0 3px 0 var(--color-layer-border),
    0 5px 7px rgb(0 0 0 / 0.55);
}
<section class="panel-shadow rounded-2xl border border-layer-border bg-layer-panel p-4">
  ...
</section>
```

## Styles interdits

Ne jamais utiliser :

- styles inline ;
- !important ;
- sélecteurs ID ;
- classes génériques comme .box, .wrapper, .style1 ;
- couleurs hardcodées si un token existe ;
- animations décoratives inutiles ;
- selectors trop profonds ;
- CSS ajouté pour un besoin futur hypothétique.
- em,rem
- BEM notation

## Angular templates

Éviter ngStyle.

Éviter ngClass si une liaison de classe simple suffit.

Préférer :

```html
<button
  class="rounded-xl px-4 py-3"
  [class.opacity-50]="disabled()"
  [class.pointer-events-none]="disabled()"
>
  Save
</button>
```

Éviter :

```html
<button [ngClass]="{ disabled: disabled() }">Save</button>
```

## Organisation des fichiers

Utiliser un fichier SCSS par composant.

example-card/
example-card.component.ts
example-card.component.html
example-card.component.scss

Le SCSS doit rester court.

Si un fichier devient long, vérifier si :

- le composant fait trop de choses ;
- une section doit devenir un composant enfant ;
- une classe est inutile ;
- un style peut être remplacé par Tailwind.

Éviter les gros fichiers globaux.

Les styles globaux sont réservés à :

- tokens ;
- fonts ;
- reset minimal ;
- utilitaires vraiment partagés ;
- keyframes globales réellement réutilisées.

## Nommage des classes CSS

Utiliser le kebab-case. No BEM notation, limit nesting as mutch as possible.

À préférer :

.player-card {}
.is-active {}
.has-error {}

À éviter :

.playerCard {}
.blue-box {}
.style1 {}

Nommer selon le rôle, pas selon l’apparence.

À préférer :

.action-panel {}

À éviter :

.brown-box {}

Préfixes autorisés pour les états :

.is-active {}
.is-disabled {}
.has-error {}
.has-badge {}
Couleurs

## Utiliser les tokens du projet quand ils existent.

À préférer :

```html
<div class="bg-layer-panel text-text-primary border-layer-border"></div>
```

À éviter :

```html
<div class="bg-[#3f2818] text-[#fff3d0]"></div>
```

Dans le SCSS, utiliser les variables existantes :

.card {
background: var(--color-layer-panel);
border-color: var(--color-layer-border);
color: var(--color-text-primary);
}

Ne pas hardcoder une couleur si elle existe déjà dans le thème.

Les nouvelles couleurs doivent être ajoutées au thème seulement si elles sont réutilisées ou ont un vrai rôle UI.

## Valeurs arbitraires Tailwind

Éviter les valeurs arbitraires quand une classe standard existe.

À éviter :

```html
<div class="rounded-[16px] p-[16px]"></div>
```

À préférer :

```html
<div class="rounded-2xl p-4"></div>
```

Les valeurs arbitraires sont acceptées pour :

- dimensions très spécifiques ;
- effets visuels complexes ;
- intégration avec Ionic ;
- cas où aucun token ne correspond.

## Selecteurs

Éviter les sélecteurs complexes.

## Units

Ne pas imposer une unité unique.

Règles simples :

- utiliser Tailwind pour la majorité des tailles ;
- utiliser les tokens ou variables quand possible ;
- utiliser px pour les détails visuels précis ;
- utiliser %, vh, dvh, calc() seulement quand le layout le justifie.
- ne jamais utiliser em et rem dans un fichier css

Éviter les valeurs magiques non expliquées.

## Layout

Préférer flex et grid.

Éviter les tailles fixes si le contenu peut être fluide.

À éviter :

```css
.card {
  width: 320px;
  height: 180px;
}
```

À préférer :

```html
<section class="w-full min-h-44 rounded-2xl p-4"></section>
```

Utiliser absolute seulement si le layout ne peut pas être fait proprement avec flex/grid.

## Nesting SCSS

Limiter le nesting.

Maximum recommandé : 2 niveaux.

À éviter :

```css
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

À préférer :

```css
.card-title {
  color: var(--color-text-primary);
}
```

## Accessibilité

Préférer les éléments HTML natifs quand ils existent.

À préférer :

```html
<progress value="40" max="100"></progress>
```

À éviter :

```html
<div role="progressbar"></div>
```

Les boutons icon-only doivent avoir un aria-label.

Le focus doit rester visible.

Les contrastes doivent rester suffisants.

## Checklist avant d’ajouter du style

Avant d’ajouter une classe CSS ou une règle SCSS, vérifier :

- Est-ce que Tailwind suffit ?
- Est-ce que cette règle améliore vraiment l’UI ?
- Est-ce que cette règle améliore la lisibilité ?
- Est-ce que cette règle améliore l’utilisabilité ?
- Est-ce que cette règle corrige un vrai problème ?
- Est-ce que cette règle sera comprise facilement plus tard ?

Si la réponse est non, ne pas l’ajouter.

## Règle finale

Le style doit servir l’interface, pas remplir les fichiers.

Tailwind d’abord.

SCSS seulement quand il apporte une vraie valeur.
