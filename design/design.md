# Design Rules

This document defines the design rules for the application.

The goal is to maintain a consistent, readable, mobile-first interface that is simple to maintain across the project.

This file serves as a common baseline.  
Project-specific artistic direction choices are defined in **artistic-direction.md**.

## Core Principle

The interface must be clear, useful, and immediately understandable.

Every visual element must help the user to:

- understand the screen;
- identify the primary action;
- read the important information;
- interact easily on mobile.

Do not add decoration if it does not improve the user experience.

## Visual Hierarchy

The screen must have a clear hierarchy.

Priority order:

- Primary action
- Important information
- Navigation
- Secondary information
- Decoration

Decoration must never interfere with reading or interaction.

A page must be understandable quickly, even on a small screen.

## Mobile First

The interface is designed for mobile first.

Rules:

- interactive elements must be large and easy to tap;
- important buttons must be reachable by the thumb;
- text must remain readable on small screens;
- content must scroll naturally;
- fixed areas must respect safe areas;
- modals must fit into an Ionic layout without visual hacks;
- breakpoints must improve the tablet/desktop experience, not fix a broken mobile layout.

Avoid:

- desktop layouts compressed on mobile;
- touch targets that are too small;
- text that is too long in buttons;
- unnecessary nested scrolls;
- excessive overlapping elements.

## Spacing

Use a base spacing system of 4px or 8px. Do not invent arbitrary values.

Rules:

- keep consistent spacing between elements;
- maintain sufficient internal margins;
- do not press text against edges;
- use spacing to reinforce visual hierarchy, not just to fill space.

Spacing must be defined in the project theme, not scattered across components.

## Layout

Screens must be built with a clear structure.

Recommended structure:

- header or context area;
- main content;
- primary actions;
- navigation or footer if necessary.

Rules:

- keep consistent spacing;
- maintain sufficient internal margins;
- do not press text against edges;
- avoid stacking frames within frames;
- do not overuse `absolute` positioning when flex/grid is sufficient.

An element must not be buried in too many unnecessary visual layers.

## Colors

Always use the project's Tailwind/CSS/Figma tokens when they exist.

Rules:

- do not add a color if a token already exists;
- do not multiply colors for the same type of element;
- maintain strong contrast between text and background;
- use vivid colors only for states or important actions;
- keep a consistent palette throughout the application.

Colors must be defined in the project theme, not scattered across components.

## Typography

Use the fonts defined by the project.

Rules:

- titles must be short;
- button labels must be readable at a glance;
- long texts must remain comfortable to read;
- stats or values must be easy to compare;
- do not use too many different sizes within the same component;
- stick to h1, h2, h3, h4, h5, h6, p, sub — no deeper levels;
- do not place important information only inside an image.

Typography must support hierarchy, not decorate unnecessarily.

## Panels and Surfaces

Panels are used to group information or an action.

Rules:

- a panel must have a clear responsibility;
- an important panel may have a visible border or relief;
- a secondary panel must remain more discreet;
- shadows must separate elements, not overload the screen;
- avoid unnecessarily nested panels.

A surface must always make content more readable.

## Buttons

Buttons must be visible, readable, and easy to tap.

States to handle:

- normal;
- pressed;
- active;
- disabled;
- keyboard focus;
- loading if necessary.

Rules:

- a primary button must be immediately identifiable;
- a secondary button must be less prominent than the primary button;
- a disabled button must lose contrast and not appear clickable;
- an icon-only button must have an `aria-label`;
- do not use hover as the only feedback;
- avoid labels that are too long.

An important action must not be hard to find.

## Icons

Icons must be simple and readable at small sizes.

Rules:

- use recognizable shapes;
- maintain a sufficient size;
- avoid details that are invisible on mobile;
- do not use an icon alone if the meaning is not obvious;
- add an accessible label if the icon carries an action or information.

## Cards

Cards are used to present self-contained information.

Rules:

- one card = one main idea;
- the title must be visible at a glance;
- information must be grouped clearly;
- actions must be close to the relevant information;
- the image or illustration must help understanding, not fill space;
- cards repeated in a list must remain lightweight.

A detail card can be richer.  
A card repeated 50 times must stay simple.

## Lists

Lists must remain readable and quick to scan.

Rules:

- each item must have a clear hierarchy;
- primary information must be visible without effort;
- secondary actions must not steal attention;
- selected, active, or disabled states must be obvious;
- repeated items must remain visually lightweight.

Avoid:

- too many shadows in a long list;
- too many icons per item;
- items that are too tall for no reason;
- secondary text that is too prominent.

## Modals

A modal must serve a specific action or piece of information.

Rules:

- the background behind the modal must remain discreet;
- the content must be clear and limited;
- the close button must be visible and easy to tap;
- the modal must adapt to its content;
- internal scrolling must feel natural;
- primary actions must be at the bottom or easily accessible.

Avoid:

- a modal that is too tall for short content;
- multiple stacked modals;
- a modal that carries too many responsibilities;
- custom layouts that break Ionic's default behavior.

## Forms

Forms must be easy to fill out on mobile. Where possible, show validation requirements upfront and highlight them in green when met. Minimize errors and confirm success clearly.

Rules:

- visible labels;
- fields large enough to tap;
- understandable error messages;
- clear primary action;
- visible validation that does not rely solely on color;
- mobile keyboard adapted to the field type.

Avoid:

- too many fields on a single screen;
- placeholders used as the only labels;
- technical error messages;
- mandatory errors without guidance;
- buttons placed too far from the form.

## Progress Bars

Use the native HTML `<progress>` element when appropriate.

Rules:

- always display context around the progress;
- add a visible or accessible label;
- maintain a sufficient height on mobile;
- use a color consistent with the type of progress;
- do not fake a progress bar with a `div` if `<progress>` suffices.

## Empty States

A screen with no data must remain understandable.

Rules:

- display a clear message explaining why the screen is empty;
- propose an action if possible;
- avoid a blank page without context;
- keep the empty state consistent with the rest of the interface.

An empty state is part of the experience, not an afterthought.

## Images and Media

Images must help the user understand, not just fill space.

Rules:

- always provide an `alt` attribute on images;
- use appropriate aspect ratios and avoid distortion;
- prefer lazy loading for long lists or heavy pages;
- do not place important information only inside an image;
- keep images consistent in style and treatment across the interface;
- avoid decorative images that add visual noise without purpose.

An image that does not add meaning should be removed or replaced with a CSS background.

## Visual States

Every important state must be visible.

States to handle depending on the component:

- normal;
- active;
- selected;
- disabled;
- locked;
- available;
- loading;
- success;
- error;
- focus;
- pressed.

Rules:

- do not rely solely on color;
- also use a border, an icon, text, a change in relief, or opacity;
- errors must have explicit text;
- disabled states must remain readable;
- keyboard focus must be visible.

## User Feedback

Every important action must provide feedback.

Possible feedback:

- pressed state;
- toast;
- inline message;
- short animation;
- state change;
- loading indicator;
- visual confirmation.

Rules:

- feedback must be immediate;
- a blocking action must display a loading state;
- an error must explain what to do;
- a success must not unnecessarily interrupt the user.

## Animations

Animations must be rare, short, and purposeful.

Use an animation to:

- confirm an action;
- signal a state change;
- draw attention to important information;
- improve understanding of a transition.

Avoid:

- unnecessary permanent animations;
- fast movements;
- flashy effects;
- animations that interfere with reading;
- costly animations in lists.

Complex animations must be placed in SCSS, not in the HTML.

## Shadows and Visual Effects

Shadows are used to clarify depth and separation.

Rules:

- use shadows sparingly;
- avoid heavy shadows on repeated elements;
- place complex shadows in SCSS;
- keep effects consistent across components;
- do not use a visual effect if it reduces readability.

Effects must support the interface, not dominate it.

## Accessibility

The interface must remain usable by everyone.

Rules:

- contrast must be sufficient;
- icon-only buttons must have an `aria-label`;
- focus must be visible;
- important text must not appear only in an image;
- states must not rely solely on color;
- native HTML elements are preferred when they exist;
- components must target AXE / WCAG AA compatibility.

Accessibility is not a final step — it is part of the design.

## UI Validation Checklist

Before validating a component:

- The primary action is obvious.
- Text is readable on mobile.
- Visual states are present.
- Buttons are large enough for touch.
- Icons are understandable at small sizes.
- Colors use the project's tokens.
- The component remains accessible.
- Decoration does not interfere with reading.
- The component respects the project's artistic direction.

## Final Rule

Design must remain clear, consistent, and usable.

The priority is always:

- understand;
- read;
- tap;
- act.

Artistic direction must strengthen the experience, never make it harder.