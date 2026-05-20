## Folder Structure

Use a clear structure with `core` and `pages`.

- `core` contains global and shared application code.
- `pages` contains the application screens and their specific components.

```txt
src/app/
  core/
    services/
    guards/
    models/
    utils/
    directives/
    pipes/
    components/
      app-button/
        app-button.component.ts
        app-button.component.html
        app-button.component.scss

      empty-state/
        empty-state.component.ts
        empty-state.component.html
        empty-state.component.scss

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

Files are named by role: `.page.ts` for routed pages, `.component.ts` for components, `.service.ts` for services.


## DO NOT 

DO NOT SEPARATE BY FEATURE 