# mereeditions.com — coming-soon page

Single static page holding the domain until the full Mere Editions site is built
(winter/spring 2027 per STRATEGY.md). Same design language as mortosullivan.com
(Inter, warm paper) and the same deploy flow (GitHub → Cloudflare Workers).

No build step. `public/index.html` is the whole site; Inter is self-hosted at
`public/fonts/` (latin subset of @fontsource-variable/inter 5.2.8).

## Deploy (same as the other two sites)

1. Create private GitHub repo `mereeditions-com`, push this folder.
2. Cloudflare dash → Workers & Pages → import repo. No build command;
   wrangler.jsonc points assets at `./public`.
3. Preview at `mereeditions-com.<account>.workers.dev`.
4. Custom domain: requires the mereeditions.com zone on Cloudflare —
   **DNS still at Hover with Google Workspace MX. See caution below.**

## DNS cutover caution (email must not blip)

mereeditions.com carries live Google Workspace email. Before switching
nameservers to Cloudflare: export/screenshot ALL current Hover records
(MX ×1–5, SPF TXT, DKIM TXT `google._domainkey`, any verification records),
add the zone to Cloudflare, verify every record was imported (the aligivens
scan missed two), set records to DNS-only (gray cloud), THEN switch
nameservers at Hover. Only after email is confirmed working: add
www.mereeditions.com custom domain + apex redirect to the Worker.

## Open items

- [x] Contact email: `info@mereeditions.com` confirmed valid (7/7). Aliases
      mort@, m@, studio@, editions@ all reach the same inbox — swap on request.
- [x] Copy: "Full site coming soon" per Mort (7/7) — no year; description lines approved.
