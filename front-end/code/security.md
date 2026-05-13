# Security Rules

Ce document définit les règles de sécurité côté front-end.

Le front-end n’est jamais une zone de confiance.

## Environment

Utiliser des environnements séparés :

- development
- staging
- production

Les fichiers d’environnement peuvent contenir :

- URL publique d’API
- nom d’environnement
- feature flags publics
- configuration publique

Ils ne doivent jamais contenir :

- secret API
- clé privée
- JWT secret
- token admin
- mot de passe
- connection string de base de données
- clé service role
- credentials serveur

Les secrets doivent rester côté backend ou dans une configuration sécurisée serveur.

## Front-end is public

Tout ce qui est dans le front peut être lu par l’utilisateur.

Ne jamais considérer comme secret :

- le code JavaScript compilé ;
- les variables d’environnement front ;
- les appels API visibles dans le navigateur ;
- les assets ;
- les routes ;
- les feature flags front.

Si une information ne doit pas être publique, elle ne doit pas être dans le front.

## API security

Toutes les règles de sécurité importantes doivent être vérifiées côté backend.

Le front peut améliorer l’UX avec des guards ou des conditions d’affichage, mais il ne protège pas réellement les données.

À ne pas faire :

```ts
if (user.role === "admin") {
  showAdminButton();
}
```

comme unique protection.

Le backend doit toujours vérifier :

- l’authentification ;
- les permissions ;
- la propriété des ressources ;
- les limites métier ;
- les droits d’accès.
- Authentication

Ne pas stocker de token sensible inutilement.

Préférer :

- session sécurisée côté backend si possible ;
- cookies HttpOnly, Secure, SameSite si l’architecture le permet ;
- stockage court et contrôlé si un token front est nécessaire.

Éviter :

- token longue durée dans localStorage ;
- refresh token exposé au JavaScript ;
- token admin côté front.

## Local storage

Le localStorage ne doit contenir que des données non sensibles.

Accepté :

- préférences UI ;
- thème ;
- état local non critique ;
- cache public ;
- progression non sensible.

À éviter :

- token sensible ;
- données personnelles critiques ;
- données de paiement ;
- permissions utilisateur ;
- secrets.

## User input

Toute donnée venant de l’utilisateur doit être considérée comme non fiable.

À faire :

- valider côté front pour l’UX ;
- valider aussi côté backend ;
- échapper ou éviter le HTML dynamique ;
- ne pas injecter directement du contenu utilisateur dans le DOM.

Éviter :

```html
element.innerHTML = userContent;
```

Préférer l’affichage Angular standard :

```html
<p>{{ userContent }}</p>
```

## External links

Les liens externes ouverts dans un nouvel onglet doivent utiliser :

```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  External link
</a>
```

## Dependencies

Ajouter une dépendance seulement si elle est nécessaire.

Avant d’ajouter une librairie :

- vérifier qu’elle est maintenue ;
- vérifier son usage réel ;
- éviter les packages inconnus pour une petite fonction ;
- garder les dépendances à jour.

Ne pas ajouter une librairie lourde pour une logique simple.

## Error messages

Les erreurs affichées à l’utilisateur doivent être compréhensibles mais ne doivent pas exposer de détails sensibles.

À éviter :

Database connection failed with user admin on host...

À préférer :

Une erreur est survenue. Réessaie dans quelques instants.

## Checklist

Avant de valider une feature front :

- Aucun secret n’est dans le front.
- Les permissions sont vérifiées côté backend.
- Le front ne fait pas confiance à ses propres guards.
- Les tokens ne sont pas loggés.
- Les données sensibles ne sont pas dans localStorage.
- Les entrées utilisateur ne sont pas injectées en HTML brut.
- Les liens externes utilisent rel="noopener noreferrer".
- Les permissions Capacitor sont limitées.
- Les erreurs utilisateur ne révèlent pas de détails techniques sensibles.
