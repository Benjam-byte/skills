# Design Rules

Ce document définit les règles de design de l’application.

L’objectif est de garder une interface cohérente, lisible, mobile-first et simple à maintenir dans un projet.

Ce fichier sert de base commune.  
Les choix spécifiques à la direction artistique du projet se trouve dans le fichier **direction-artistique.md**.

## Principe principal

L’interface doit être claire, utile et immédiatement compréhensible.

Chaque élément visuel doit aider à :

- comprendre l’écran ;
- identifier l’action principale ;
- lire les informations importantes ;
- interagir facilement sur mobile.

Ne pas ajouter de décoration si elle n’améliore pas l’expérience utilisateur.

## Hiérarchie visuelle

L’écran doit avoir une hiérarchie claire.

Ordre de priorité :

- Action principale
- Information importante
- Navigation
- Informations secondaires
- Décoration

La décoration ne doit jamais gêner la lecture ou l’interaction.

Une page doit pouvoir être comprise rapidement, même sur petit écran.

## Mobile first

L’interface est pensée d’abord pour mobile.

Règles :

- les éléments interactifs doivent être grands et faciles à toucher ;
- les boutons importants doivent être accessibles au pouce ;
- les textes doivent rester lisibles sur petit écran ;
- le contenu doit scroller naturellement ;
- les zones fixes doivent respecter les safe areas ;
- les modales doivent rentrer dans un layout Ionic sans hack visuel ;
- les breakpoints doivent améliorer l’expérience tablette/desktop, pas réparer un layout mobile cassé.

Éviter :

- les layouts desktop compressés sur mobile ;
- les zones tactiles trop petites ;
- les textes trop longs dans les boutons ;
- les scrolls imbriqués inutiles ;
- les superpositions excessives.

## Layout

Les écrans doivent être construits avec une structure claire.

Structure recommandée :

- header ou zone de contexte ;
- contenu principal ;
- actions principales ;
- navigation ou footer si nécessaire.

Règles :

- garder des espacements réguliers ;
- garder des marges internes suffisantes ;
- ne pas coller le texte aux bordures ;
- éviter les empilements de cadres dans des cadres ;
- ne pas abuser de absolute si un layout flex/grid suffit.

Un élément ne doit pas être dans trop de couches visuelles inutiles.

## Couleurs

Toujours utiliser les tokens Tailwind/CSS/Figma du projet quand ils existent.

Règles :

- ne pas ajouter une couleur si un token existe ;
- ne pas multiplier les couleurs pour un même type d’élément ;
- garder un contraste fort entre texte et fond ;
- utiliser les couleurs vives uniquement pour les états ou les actions importantes ;
- garder une palette cohérente sur toute l’application.

Les couleurs doivent être définies dans le thème du projet, pas dispersées dans les composants.

## Typographie

Utiliser les polices définies par le projet.

Règles :

- les titres doivent être courts ;
- les labels de boutons doivent être lisibles rapidement ;
- les textes longs doivent rester confortables à lire ;
- les stats ou valeurs doivent être faciles à comparer ;
- ne pas utiliser trop de tailles différentes sur un même composant ;
- Se contenter de h1,h2,h3,h4,h5,h6,p,sub pas plus de niveau
- ne pas mettre une information importante uniquement dans une image.

La typographie doit aider la hiérarchie, pas décorer inutilement.

## Panneaux et surfaces

Les panneaux servent à regrouper une information ou une action.

Règles :

- un panneau doit avoir une responsabilité claire ;
- un panneau important peut avoir une bordure ou un relief visible ;
- un panneau secondaire doit rester plus discret ;
- les ombres doivent séparer les éléments, pas surcharger l’écran ;
- éviter les panneaux imbriqués inutilement.

Une surface doit toujours rendre le contenu plus lisible.

## Boutons

Les boutons doivent être visibles, lisibles et faciles à toucher.

États à prévoir :

- normal ;
- pressé ;
- actif ;
- désactivé ;
- focus clavier ;
- chargement si nécessaire.

Règles :

- un bouton principal doit être immédiatement identifiable ;
- un bouton secondaire doit être moins visible que le bouton principal ;
- un bouton désactivé doit perdre en contraste et ne pas sembler cliquable ;
- un bouton icon-only doit avoir un aria-label ;
- ne pas utiliser le hover comme seul feedback ;
- éviter les labels trop longs.

Une action importante ne doit pas être difficile à trouver.

## Icônes

Les icônes doivent être simples et lisibles en petite taille.

Règles :

- utiliser des formes reconnaissables ;
- garder une taille suffisante ;
- éviter les détails invisibles sur mobile ;
- ne pas utiliser une icône seule si le sens n’est pas évident ;
- ajouter un label accessible si l’icône porte une action ou une information.

## Cartes et fiches

Les cartes servent à présenter une information autonome.

Règles :

- une carte = une idée principale ;
- le titre doit être visible rapidement ;
- les informations doivent être regroupées clairement ;
- les actions doivent être proches de l’information concernée ;
- l’image ou l’illustration doit aider à comprendre, pas remplir l’espace ;
- les cartes répétées dans une liste doivent rester légères.

Une carte de détail peut être plus riche.
Une carte répétée 50 fois doit rester simple.

## Listes

Les listes doivent rester lisibles et rapides à parcourir.

Règles :

- chaque item doit avoir une hiérarchie claire ;
- les informations principales doivent être visibles sans effort ;
- les actions secondaires ne doivent pas voler l’attention ;
- les états sélectionné, actif ou désactivé doivent être évidents ;
- les items répétés doivent rester visuellement légers.

Éviter :

- trop de shadows dans une longue liste ;
- trop d’icônes par item ;
- des items trop hauts sans raison ;
- des textes secondaires trop présents.

## Modales

Une modale doit servir une action ou une information précise.

Règles :

- le fond derrière la modale doit rester discret ;
- le contenu doit être clair et limité ;
- le bouton de fermeture doit être visible et facile à toucher ;
- la modale doit s’adapter à son contenu ;
- le scroll interne doit rester naturel ;
- les actions principales doivent être en bas ou facilement accessibles.

Éviter :

- une modale trop haute pour un contenu court ;
- plusieurs modales empilées ;
- une modale qui contient trop de responsabilités ;
- des layouts custom qui cassent le comportement Ionic.

## Formulaires

Les formulaires doivent être faciles à remplir sur mobile. Si possible faire des formulaires ou les validations attendus sont spécifiés à l'utilisatuer et passe en vert quand ils sont bon. Limités l'erreurs est indiqué la réussite.

Règles :

- labels visibles ;
- champs assez grands ;
- erreurs compréhensibles ;
- action principale claire ;
- validation visible sans dépendre uniquement de la couleur ;
- clavier mobile adapté au type de champ.

Éviter :

- trop de champs sur un seul écran ;
- placeholders utilisés comme seuls labels ;
- messages d’erreur techniques ;
- Erreurs obligatoires ;
- boutons trop loin du formulaire.

## Barres de progression

Utiliser l’élément HTML natif <progress> quand c’est adapté.

Règles :

- toujours afficher un contexte autour de la progression ;
- ajouter un label visible ou accessible ;
- garder une hauteur suffisante sur mobile ;
- utiliser une couleur cohérente avec le type de progression ;
- ne pas créer une fausse progressbar avec une div si <progress> suffit.

## États visuels

Chaque état important doit être visible.

États à prévoir selon le composant :

- normal ;
- actif ;
- sélectionné ;
- désactivé ;
- verrouillé ;
- disponible ;
- chargement ;
- succès ;
- erreur ;
- focus ;
- pressé.

Règles :

- ne pas dépendre uniquement de la couleur ;
- utiliser aussi une bordure, une icône, un texte, un changement de relief ou une opacité ;
- les erreurs doivent avoir un texte explicite ;
- les états désactivés doivent rester lisibles ;
- le focus clavier doit être visible.

## Feedback utilisateur

Chaque action importante doit donner un retour.

Feedback possible :

- état pressé ;
- toast ;
- message inline ;
- animation courte ;
- changement d’état ;
- loading ;
- confirmation visuelle.

Règles :

- le feedback doit être immédiat ;
- une action bloquante doit afficher un état de chargement ;
- une erreur doit expliquer quoi faire ;
  -un succès ne doit pas interrompre inutilement l’utilisateur.

## Animations

Les animations doivent être rares, courtes et utiles.

Utiliser une animation pour :

- confirmer une action ;
- signaler un changement d’état ;
- attirer l’attention sur une information importante ;
- améliorer la compréhension d’une transition.

Éviter :

- animations permanentes inutiles ;
- mouvements rapides ;
- effets flashy ;
- animations qui gênent la lecture ;
- animations coûteuses dans les listes.

Les animations complexes doivent être dans le SCSS, pas dans le HTML.

## Ombres et effets visuels

Les ombres servent à clarifier la profondeur et la séparation.

Règles :

- utiliser les shadows avec modération ;
- éviter les ombres lourdes sur les éléments répétés ;
- placer les shadows complexes dans le SCSS ;
- garder les effets cohérents entre composants ;
- ne pas utiliser d’effet visuel s’il réduit la lisibilité.

Les effets doivent soutenir l’interface, pas la dominer.

## L’interface doit rester utilisable par tous.

Règles :

- les contrastes doivent être suffisants ;
- les boutons icon-only doivent avoir un aria-label ;
- le focus doit être visible ;
- les textes importants ne doivent pas être uniquement dans une image ;
- les états ne doivent pas dépendre uniquement de la couleur ;
- les éléments HTML natifs sont à privilégier quand ils existent ;
- les composants doivent viser une compatibilité AXE / WCAG AA.

L’accessibilité n’est pas une étape finale, elle fait partie du design.

## Responsive

Concevoir d’abord pour mobile portrait.

Règles :

- boutons larges ;
- textes lisibles ;
- zones tactiles confortables ;
- contenu scrollable naturellement ;
- actions principales accessibles ;
- pas de layout desktop compressé sur mobile.

Les breakpoints doivent améliorer l’expérience sur grand écran, pas compenser une mauvaise base mobile.

## Checklist avant validation UI

Avant de valider un composant :

- L’action principale est évidente.
- Le texte est lisible sur mobile.
- Les états visuels sont présents.
- Les boutons sont assez grands pour le tactile.
- Les icônes sont compréhensibles en petite taille.
- Les couleurs utilisent les tokens du projet.
- Le composant reste accessible.
- La décoration ne gêne pas la lecture.
- Le composant respecte la direction artistique du projet.

## Règle finale

Le design doit rester clair, cohérent et utilisable.

La priorité reste toujours :

- comprendre ;
- lire ;
- toucher ;
- agir.

La direction artistique doit renforcer l’expérience, jamais la rendre plus difficile.
