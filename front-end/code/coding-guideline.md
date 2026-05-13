# Coding Guidelines

Ce document définit les règles générales d’écriture du code.

Le but est de garder le code lisible, explicite et simple à maintenir.

## Principe principal

Le code doit être facile à comprendre sans effort.

Préférer :

- des noms explicites ;
- des fonctions courtes ;
- des responsabilités claires ;
- une logique simple ;
- peu de commentaires, mais utiles.

Éviter :

- les abréviations ;
- les noms trop courts ;
- les fonctions qui font trop de choses ;
- les commentaires qui répètent le code ;
- les abstractions prématurées.

## Naming

Toujours préférer un mot complet à une lettre.

À éviter :

```ts
for (const i of words) {
  // ...
}
```

À préférer :

```ts
for (const index of wordList) {
  // ...
}
```

## Variables

Les variables doivent expliquer ce qu’elles contiennent.

À éviter :

const data = getUser();
const result = calculateScore();
const temp = value + 1;

À préférer :

const user = getUser();
const finalScore = calculateScore();
const nextLevel = currentLevel + 1;

Éviter les noms génériques sauf si le contexte est évident.

Noms à éviter si possible :

data
result
value
temp
obj
arr
el
e
x
y
i
j

Exceptions acceptées :

event
error
index
Arrays

### Un tableau doit être nommé avec le suffixe List.

À préférer :

const userList: User[] = [];
const selectedItemList: Item[] = [];
const visibleCardList: Card[] = [];

Éviter :

const users: User[] = [];
const selectedItems: Item[] = [];
const cards: Card[] = [];

Pour un seul élément, utiliser le nom singulier.

const user: User = userList[0];
const selectedItem: Item = selectedItemList[0];

## Booleans

Les booléens doivent commencer par un préfixe clair. Limite l'usage des ternaires.

Préfixes recommandés :

is
has
can
should
was
will

À préférer :

const isOpen = signal(false);
const hasError = computed(() => errorMessage() !== null);
const canSubmit = computed(() => form.valid);
const shouldShowModal = signal(false);

À éviter :

const open = signal(false);
const error = signal(false);
const submit = computed(() => form.valid);
Functions

## Une fonction doit faire une seule chose.

Le nom doit commencer par un verbe.

Préfixes recommandés :

- get
- set
- create
- update
- delete
- remove
- find
- filter
- sort
- build
- format
- calculate
- validate
- toggle
- open
- close
- handle

  À préférer :

```ts
function calculateTotalPrice(itemList: Item[]): number {
  return itemList.reduce((totalPrice, item) => totalPrice + item.price, 0);
}
```

À éviter :

```ts
function total(items: Item[]): number {
  return items.reduce((t, i) => t + i.price, 0);
}
```

## Event handlers

Les méthodes appelées directement par le template doivent être préfixées avec on.

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

Éviter de mettre trop de logique dans un handler.

Le handler peut appeler une méthode privée plus précise.

## Private methods

Les méthodes privées doivent rester simples et explicites. Toutes methodes qui n'est pas utiliser en dehors du composant doit etre déclare private. Les methodes privates sont déclarer à la fin du fichier apres les publics.

```ts
private buildUserPayload(): UserPayload {
return {
name: this.name(),
email: this.email(),
};
}
```

Éviter les méthodes privées trop génériques :

private processData(): void {}
private handleStuff(): void {}
Angular signals

## Types

Éviter any.

Utiliser unknown si le type est vraiment incertain.

À préférer :

```ts
function parseApiResponse(response: unknown): User | null {
  if (!isUser(response)) {
    return null;
  }

  return response;
}
```

À éviter :

```ts
function parseApiResponse(response: any): User {
  return response;
}
```

## Conditions

Préférer les conditions lisibles.

À éviter :

```ts
if (!user || !user.isActive || user.score < 10 || user.status !== "ready") {
  return;
}
```

À préférer :

```ts
const canStartGame =
  user !== null && user.isActive && user.score >= 10 && user.status === "ready";

if (!canStartGame) {
  return;
}
```

## Early returns

Utiliser les early returns pour éviter les gros blocs imbriqués.

À éviter :

```ts
function submit(): void {
  if (this.form.valid) {
    if (!this.isLoading()) {
      this.save();
    }
  }
}
```

À préférer :

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

## Loops

Toujours préférer des noms explicites dans les boucles.

À éviter :

```ts
for (const i of items) {
  console.log(i.name);
}
```

À préférer :

```ts
for (const item of itemList) {
  console.log(item.name);
}
```

Pour les index :

```ts
for (const [index, item] of itemList.entries()) {
  console.log(index, item.name);
}
```

Éviter :

```ts
for (let i = 0; i < itemList.length; i++) {
  console.log(i);
}
```

## Comments

Ne pas commenter chaque règle. Jamais de TODO,FIXME ou autre commentaire pour le "futur".

Un commentaire doit expliquer pourquoi le code existe, pas ce que le code fait.

Ajouter un commentaire uniquement pour expliquer :

- un hack ;
- une contrainte Ionic ;
- une contrainte navigateur ;
- une valeur non évidente ;
- un choix UI important ;
- une règle métier complexe ;
- une limitation technique ;
- un contournement temporaire.

À éviter :

```ts
// Set loading to true
this.isLoading.set(true);
```

À préférer :

```ts
// Required because ion-content creates its own scroll container.
this.maxModalHeight.set("70dvh");
```

## Comment format

Les commentaires doivent être courts et utiles.

À préférer :

```ts
// Required because Safari does not support this behavior consistently.
```

À éviter :

```ts
// This is here because we had a bug one day and this seems to fix it.
```

## Error handling

Les erreurs doivent être explicites. Ajouter la gestion des erreurs uniquement si elle précisé/demandé. N'invente pas des gestion de cas d'erreur si elles ne sont pas spécifié par le metier. Demande si une erreur semble trop évidente mais qu'elle n'est pas précisé.

À éviter :

```ts
catch (error) {
console.log(error);
}
```

À préférer :

```ts
catch (error: unknown) {
this.errorMessage.set('Unable to save settings.');
console.error('Settings save failed', error);
}
```

Ne pas ignorer une erreur silencieusement.

## Magic values

Éviter les valeurs magiques.

À éviter :

```ts
if (score > 100) {
  return "gold";
}
```

À préférer :

```ts
const goldScoreThreshold = 100;

if (score > goldScoreThreshold) {
  return "gold";
}
```

## Final checklist

Avant de valider du code :

- Les noms sont explicites.
- Les tableaux utilisent le suffixe List.
- Les variables ne sont pas nommées avec une seule lettre.
- Les fonctions ont un verbe clair.
- Les booléens commencent par is, has, can ou should.
- Les conditions complexes sont extraites dans des variables lisibles.
- Les fonctions restent courtes.
- Les commentaires expliquent une vraie raison.
- Aucune abstraction n’est créée “pour plus tard”.
- Les erreurs ne sont ajoutés qu'aux endoits metier.
