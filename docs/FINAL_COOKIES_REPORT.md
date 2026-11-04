# MiryoLove Web — final cookies report

**Audit date:** 4 November 2026

## 1. Cookies found

No `document.cookie` use was found in the reviewed site code. Public responses for the homepage, `script.js`, and `cookies.html` returned HTTP 200 without a `Set-Cookie` header at the time checked. No site HTTP cookies were identified in this sample.

No optional cookies or other optional tracking technologies requiring user choice were found. The site keeps the informational notice and does not offer a fictitious accept/reject flow.

## 2. Other browser storage found

- `localStorage` / `miryolove-language`: stores the visitor-selected `en` or `ru` interface language until removed or site data is cleared.
- `sessionStorage` / `miryolove-cookie-notice-dismissed-v1`: stores `1` after the informational notice is dismissed, for the current tab session only.
- The former `localStorage` key `miryolove-consent-v1` stored category flags. The consent interface was removed; the current script no longer reads or writes those flags and attempts to remove the old key.
- No IndexedDB, Service Worker, Cache API, or other application-managed browser storage was found.

## 3. Third-party connections

No analytics, advertising pixels, tracking scripts, third-party fonts, or embedded frames were found. Site assets and scripts are local. Links to GitHub, Hugging Face, Telegram, and separate MiryoLove project hosts are navigational links and are contacted only if followed. The hosting provider's request logging and retention practices remain unverified.

## 4. Changes made

- Replaced the accept/reject/customize consent interface with an informational bilingual notice. “Got it” / “Понятно” is explicitly a dismissal, not consent.
- Removed local persistence of obsolete consent categories and added cleanup of the former consent key.
- Kept the selected-language preference and changed the default for new visitors to English.
- Added tab-session-only notice dismissal in `sessionStorage`; no cookie or persistent notice identifier was added.
- Updated the cookie and privacy policy content to distinguish HTTP cookies from `localStorage` and `sessionStorage`, explain retention and controls, disclose the missing public privacy contact, and avoid unsupported legal conclusions.

## 5. Files changed

- `script.js`
- `styles.css`
- `README.md`
- `docs/COOKIES_AUDIT.md`
- `docs/COOKIE_POLICY_RU.md`
- `docs/COOKIE_POLICY_EN.md`
- `docs/FINAL_COOKIES_REPORT.md`

## 6. Pages

The existing static `cookies.html` page is retained and rendered in RU/EN through the site's existing language switch. A policy link is added to the existing footer. The project uses flat `.html` pages rather than a route-based `/cookies` path; no alternate route or server rewrite was introduced.

## 7. RU/EN notice behavior

The notice uses the saved site language and updates when the visitor switches RU/EN. A new visitor defaults to English. It links to `cookies.html`, is keyboard operable, is exposed as a labeled live region, and respects reduced-motion settings. Dismissal is remembered for the current tab session only and creates no consent record.

## 8. Checks performed

- Source inventory across the website's HTML, JavaScript, CSS, and static assets.
- Searches for cookie APIs, browser storage, service workers, request APIs, third-party embeds, and common analytics/ad scripts.
- Public HTTP response checks for `https://miryolove.org/`, `/script.js`, and `/cookies.html`; all returned HTTP 200 and none returned `Set-Cookie`.
- `node --check` on `script.js` and `git diff --check`; both passed. The repository has no package manifest or configured test runner/build.
- Local browser loaded all 11 HTML pages with HTTP 200, RU/EN rendering, and footer policy links. The notice appeared in English by default, updated in Russian when switched, and persisted the language selection.
- Keyboard activation with Enter dismissed the notice. `sessionStorage` recorded only the dismissal; same-tab reload kept it hidden, while a new tab showed it again. `document.cookie` was empty in the local browser.
- Local browser reported no JavaScript console errors or external network requests while visiting the pages.
- At 390 × 844, the notice stayed within the viewport without horizontal overflow. Desktop and mobile screenshots were visually inspected; CSS also honors reduced-motion preferences.
- The policy page was verified in both languages and links to it were present in the footer.

## 9. Remaining limitations

- Public HTTP checks were performed before these repository changes are deployed; repeat them on the deployed version.
- The three sampled public paths do not prove behavior for all server routes or hosting-provider infrastructure.
- Hosting-provider identity, server logs, recipients, and retention are not documented in source.
- No official privacy contact is published in the project materials.

## 10. Legal questions needing separate review

Assess the current 152-FZ text and other applicable laws against the actual operator, processing purposes, browser values, access logs, hosting, data locations, vendors, users, and any cross-border transfers. Confirm any operator-notification, localization, security, transparency, contact, and lawful-basis requirements with qualified counsel. No specific legal basis or mandatory-consent conclusion is asserted here. A cookie notice alone does not establish legal compliance.

Reference: [Federal Law No. 152-FZ official publication record](https://publication.pravo.gov.ru/Document/View/0001200607290004); verify the current consolidated version and related rules.

The official publication portal could not be retrieved from this environment. A current consolidated text was consulted through [ConsultantPlus](https://www.consultant.ru/document/cons_doc_LAW_61801/) as a secondary source; legal review should verify the official current text.
