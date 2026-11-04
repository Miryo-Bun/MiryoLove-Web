# MiryoLove Web Cookie Policy

**Updated: 4 November 2026**

This page describes the technical behavior of the static MiryoLove Web site as reviewed on the date above. It is not legal advice.

## What cookies are

Cookies are small pieces of data that a website can ask a browser to store and then send back to that website in HTTP requests. `localStorage` and `sessionStorage` are separate browser-storage mechanisms: they are not cookies and are not automatically sent in HTTP requests.

## Cookies used by MiryoLove Web

The reviewed source code does not access `document.cookie`. No `Set-Cookie` header was found when checking public responses for the homepage, JavaScript file, and this policy page. Therefore, no HTTP cookies were identified in those checked responses. This is a point-in-time observation for specific pages; it is not a guarantee about other routes, the hosting provider, or future server configurations.

No optional cookies or other optional tracking technologies requiring a user choice were found. The site therefore displays an informational notice rather than offering fictitious consent or rejection controls.

## Other storage technologies

Although the site does not use cookies, it uses two separate storage mechanisms:

| Mechanism and key | Data and purpose | Retention |
|---|---|---|
| `localStorage`: `miryolove-language` | The language selected by the visitor, `en` or `ru`, used on subsequent visits and page navigation. | Until removed by the visitor or site data is cleared; no separate expiration is set. |
| `sessionStorage`: `miryolove-cookie-notice-dismissed-v1` | The value `1` records that the informational notice was dismissed. It is not consent to data processing. | The current tab session; the browser normally removes it when that session ends. |

These values do not contain a visitor identifier and are not automatically sent by the site in network requests. The site does not use IndexedDB, a Service Worker, or other storage APIs found in the review. The browser may independently cache site resources.

The site previously stored category choices in `localStorage` under `miryolove-consent-v1`. That mechanism is no longer used; the current code attempts to remove the obsolete entry on page load. If browser storage is blocked, the old entry may remain until manually cleared.

## Third-party and analytics cookies

The reviewed site does not connect third-party cookies, advertising trackers, pixels, or analytics systems. Third-party fonts, scripts, and frames are not embedded in its pages. Links to external sites and separate MiryoLove projects are not embedded resources: a connection to them is made only if a visitor follows a link. Their owners' policies then apply.

The hosting provider may technically receive an IP address and request details to operate the site and may keep logs. The provider, log contents, and retention periods cannot be established from the source code and must be confirmed by the owner.

## Managing cookies and browser storage

- Remove the saved language by clearing MiryoLove Web site data in your browser. Without a saved value, the interface defaults to English.
- The notice may appear again after its dismissal entry is removed. “Got it” dismisses the notice only for the current tab session and does not record consent to optional processing.
- Browser cookie controls are separate; the reviewed site does not set HTTP cookies.
- `localStorage` and `sessionStorage` can be removed in your browser's site/storage settings. They should not be confused with cookies.

## Privacy contact

The project materials do not identify a public email address or other official contact for privacy questions. The project owner should publish a working contact for such requests; no unverified address is provided here.

## Legal information

Federal Law No. 152-FZ of 27 July 2006 “On Personal Data” and other applicable requirements need a separate assessment based on the actual data, purposes, operator, users, hosting, logs, and any data transfers. This document records technical observations; it does not determine a lawful basis or conclude which legal duties apply. The notice itself does not establish compliance with 152-FZ or other laws.

Official publication: [Federal Law No. 152-FZ on the official publication portal](https://publication.pravo.gov.ru/Document/View/0001200607290004). The portal could not be retrieved from the review environment; verify the current official text and related requirements before making decisions.

The [ConsultantPlus text of 152-FZ](https://www.consultant.ru/document/cons_doc_LAW_61801/) was also consulted as a secondary source to identify topics for review. Qualified counsel should assess, as applicable, the provisions on processing conditions and consent (Articles 6 and 9), cross-border transfers (Article 12), operator duties (Article 18.1), security (Article 19), and current localization, notification, and other requirements. This is a review checklist, not a conclusion that a particular requirement applies.
