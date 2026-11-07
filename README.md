# MiryoLove Web

<!--
Copyright © 2026 Bogdan Melnikov.

MiryoLove is an independent project created by Bogdan Melnikov.

All original source code, project architecture, documentation, and original materials are protected under applicable copyright laws.

Third-party libraries, dependencies, and other external materials remain distributed under their respective licenses.
-->

Static, dependency-free MiryoLove website. The homepage, project pages, and shared RU/EN experience use the local assets in `logos/`.

## Pages

- `/` — MiryoLove homepage and project directory.
- `/miryolove-ai/`, `/miryolove-server/`, `/git-service/` — project pages.
- `/privacy-policy/`, `/cookies/`, `/terms-of-use/` — Russian/English legal information pages.
- `/changelog/` — confirmed website changes; entries without verified dates are undated.
- `/philosophy/` — MiryoLove values and approach in Russian and English.
- Missing routes — branded bilingual not-found page; Caddy is configured to preserve HTTP 404 status.

The HTML files remain the static route sources. The current `/etc/caddy/Caddyfile` points to this site's root at `/mnt/Manchild/Project/MiryoLove/MiryoLove Web` and configures a filtered access log. The files under `deploy/` are reference material only; they do not represent the active full configuration and must not be applied without a separate review.

Project and legal page content, translations, and the informational cookie notice are kept in `script.js`; shared layout and responsive styles are in `styles.css`.

## Run locally

From this directory, start Python's built-in static server:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000>. Python's server exposes the source `.html` files directly and does not implement clean routes or the custom HTTP 404 behavior.

The interface defaults to English and stores the selected Russian or English language in local storage, including when navigating between pages. A bilingual, informational cookie notice remembers dismissal only for the current tab session; it does not ask for or record consent. The reviewed source does not access `document.cookie`; a separate check found no `Set-Cookie` header in the checked GET responses on 7 November 2026. That HTTP check does not cover every route or request method. See `/cookies/` and `docs/` for the cookie policy and technical audits. Project URLs are shown as unverified unless their availability has been confirmed.

The personal data operator is identified in the privacy policy as Bogdan Igorevich Melnikov. That policy lists `bonjour.1958@mail.ru` as the primary contact for personal-data matters and `aloha.778343@gmail.com` as an additional address; the operator's postal address is not provided. Hosting-provider logging, retention, and recipients are not confirmed. The technical 152-FZ checklist is in `docs/152-FZ_TECHNICAL_AUDIT.md`; it does not establish legal compliance or replace legal review.
