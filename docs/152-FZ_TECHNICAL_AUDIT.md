# 152-FZ technical inventory for owner and legal review

Updated 7 November 2026 after a read-only review of the current deployment and server state. This is a technical inventory, not legal advice; it does not establish compliance with Federal Law No. 152-FZ or other requirements. No legal basis, consent, retention period, operator detail, or security measure is inferred where evidence is unavailable. See [the final technical audit](./privacy-final-audit.md) and [the detailed data inventory](./privacy-data-audit.md) for the latest evidence and limitations.

## Confirmed in the reviewed website source

- The site is static HTML, CSS, and JavaScript. No registration, contact form, data-submission endpoint, application API call, payment feature, or embedded chat was found.
- No `document.cookie`, analytics, advertising pixel, third-party script, embedded frame, or externally hosted font was found in the website source.
- The application stores the selected `ru`/`en` language in `localStorage` under `miryolove-language`. It stores dismissal of the informational cookie notice in `sessionStorage` under `miryolove-cookie-notice-dismissed-v1`. These values are not submitted by the site JavaScript in requests.
- The notice is informational; dismissing it is not represented as consent.
- External services are linked to, not embedded. A visitor's browser contacts them only after the visitor follows a link.
- The privacy policy says the project has no public privacy contact and expressly says hosting-provider logs and retention must be confirmed.

## Server and hosting observations

- The current `/etc/caddy/Caddyfile` and read-only Caddy admin API both point `miryolove.org` and `www.miryolove.org` to `/mnt/Manchild/Project/MiryoLove/MiryoLove Web-deploy`. The current runtime contains only those hosts, with no reverse proxy, imports, or site access logging. The earlier observation of root `/mnt/Manchild/Project/MiryoLove/Web/1` is superseded; the exact time, cause, and actor of the configuration change/reload were not established.
- Caddy runs as `caddy:caddy`; standard output is directed to systemd journal. The journal's aggregate size was about 1.2 GB, not a Caddy-specific measurement. No explicit retention period was established. `/var/log/caddy` is mode `750`, owned by `caddy:caddy`; its contents could not be inspected by the current user, so presence/content of files there is unknown.
- During the seven-day journal sample, 4,323 Caddy events were observed, including 110 structured request objects attributed to other hosts. Those objects contained IP, URI, and header fields; Referer appeared in 85 and Cookie in 34. Values were not copied. This is evidence about historical records for other hosts, not proof that main-site requests were logged in the same way.
- A saved local Caddy candidate is stale: it includes AI/press site blocks absent from current runtime and an inactive access-log configuration. It must not be applied as-is. An isolated test of its filtering showed only synthetic User-Agent among request-specific fields; production access logging remains disabled, and the configured production path could not be validated without permission.
- Read-only public HTTPS GET checks on 7 November 2026 returned 200 for checked canonical pages/assets and 404 for an unknown path. No `Set-Cookie` was found in the checked responses; TLS verification succeeded. These checks do not establish all HTTP-method behavior or practices at hosting/network providers.
- The journal aggregate size is not proof of a retention period. `/var/log/caddy` contents, external/provider logging, Snapper retention, and effective journal access controls remain unverified.
- The domain's hosting provider, infrastructure location, subprocessors, contractual terms, and possible cross-border data flows were not identified from the website source or the observed Caddy block.
- The repository includes earlier point-in-time public HTTP checks in the cookie audit documents. They do not prove the current response behavior, all routes, or hosting-provider practices.

## Risks and verification limits

- A web server and its infrastructure may receive source IP addresses and request metadata as part of delivering pages. The actual recipients, fields recorded, purposes, access, and retention are unknown.
- The current disk and runtime root do match the deployment copy, but the history and author of the active configuration change are unknown. Historical journal entries for other hosts may remain in the shared journal.
- This review did not inspect hosting-provider dashboards, contracts, infrastructure logs, backups, access-control configuration, incident records, or data-subject request handling.

## Questions for the project owner

1. Who is the data operator? Confirm the exact legal name, address, and official privacy contact to publish and monitor.
2. What actual purposes and processing operations does the operator determine for the site and server logs? What lawful bases apply to each operation? Please provide the owner-approved legal assessment; this audit does not select a basis.
3. Which hosting provider, registrar, CDN, reverse proxy, or other infrastructure vendors process requests? Where are the systems and data located, and which subprocessors can access them?
4. Which request fields are logged at each layer (including IP address, request path, user agent, timestamps, and errors)? Who can access the logs, for what purposes, and under what documented retention and deletion schedule?
5. Are backups, monitoring, security tools, or support systems copying logs or other site data? Where are copies stored, who can access them, and when are they deleted?
6. Are any data transfers outside Russia involved? If so, which data, recipients, locations, and safeguards are applicable, and have required assessments or notifications been completed?
7. What operator notifications, public disclosures, localization measures, and security measures have actually been implemented or determined to be required? Provide evidence for any measures the published policy will claim.
8. Does the operator intend to collect information through any planned forms, contact channels, or third-party services not present in the reviewed repository? If so, document the fields, purpose, recipients, and retention before enabling them.
9. Has counsel reviewed the current consolidated legal text, the privacy/cookie policies, and the actual production configuration? Who owns updates when those facts change?

## Suggested next steps

- Confirm the owner answers above with the hosting and infrastructure providers.
- Compare their evidence with the privacy and cookie policies before publication; do not add unverified claims.
- Have qualified counsel assess the current legal requirements and the operator's concrete processing practices.
- Repeat route, response-header, browser-network, and logging checks after any separately approved production configuration change.

No Caddy configuration, DNS, database, log, or data-collection behavior was changed as part of this inventory. The proposed Caddy file is for review only.
