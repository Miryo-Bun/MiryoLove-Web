# MiryoLove Web

<!--
Copyright © 2026 Bogdan Melnikov.

MiryoLove is an independent project created by Bogdan Melnikov.

All original source code, project architecture, documentation, and original materials are protected under applicable copyright laws.

Third-party libraries, dependencies, and other external materials remain distributed under their respective licenses.
-->

Static, dependency-free MiryoLove website. The homepage, project pages, and shared RU/EN experience use the local assets in `logos/`.

## Pages

- `index.html` — MiryoLove homepage and project directory.
- `projects/miryolove-ai.html`
- `projects/miryowiki.html`
- `projects/miryoboard.html`
- `projects/miryolove-server.html`
- `projects/git-service.html`
- `privacy.html`, `cookies.html`, and `terms.html` — Russian/English legal information pages.
- `roadmap.html` — confirmed project directions and status, without unsupported release dates.
- `changelog.html` — confirmed website changes; entries without verified dates are undated.
- `philosophy.html` — MiryoLove values and approach in Russian and English.

Project and legal page content, translations, and the consent interface are kept in `script.js`; shared layout and responsive styles are in `styles.css`. The MiryoWiki interface preview is `logos/MiryoWiki-interface.png`.

## Run locally

From this directory, start Python's built-in static server:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000>. The site can also be hosted as static files at the domain root.

The interface defaults to Russian and stores the selected Russian or English language in local storage, including when navigating between pages. The privacy control stores only the user's category choices in local storage; the site does not set HTTP cookies or load analytics, advertising, or tracking scripts. Optional consent categories do not activate any integrations because none are currently present. Project URLs are shown as unverified unless their availability has been confirmed.

Before public launch, the owner should confirm the hosting provider's access logs and retention practices, and publish an official privacy contact. The legal pages explicitly identify these unknowns rather than guessing.
