# mereeditions.com — DNS record inventory (pre-cutover)

Captured 2026-07-07 from Mort's Hover screenshot, full values verified by live
DNS query the same day. This is the checklist for the Cloudflare zone import:
every record below must exist in Cloudflare (DNS-only / gray cloud) BEFORE
switching nameservers at Hover. Email must not blip.

## Records to preserve

| Type | Host | Value | TTL |
|---|---|---|---|
| A | @ | 216.40.34.41 | 15 min — Hover parking page; REPLACE at cutover with Worker custom domain |
| MX | @ | `1 smtp.google.com` | 5 min — Google Workspace email (modern single-record form) |
| TXT | @ | `google-site-verification=IAYfkvaUHBJyE6MmE8O_kJ9vBZ22wtf1U4i5P96cx14` | 5 min |
| TXT | google._domainkey | `v=DKIM1;k=rsa;p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkRR9QJVbWDIGGo1P6li12yFF7gW/KkkbYnN4UDBqynRkB2nfCD5d8anWHsl7B0K1C8TqneccdTOIKFai+kVj+voo/Uf355F9U+GIOS4e51dALSJPjQpWullym5i/xG6uipW00Rh92O23jFD3wOD0MSdrAnHRlqvY3YRmFbj9yTi+pqCjC3nrQGapLQt0Bsq9cMukngz0kDqMkfvpdz0KYdZpD1mPCTaYv3F2w3NprUAmsNkKJDtczEqh0o7HLTAYtGFwLCrGG/BtJferPIDX1RcYlD+zs69rTAJ5gQdFFcRS35FbRuQ1l51H4UsErpoUDqCMsP2k/Wi16t9EtNOl+QIDAQAB` | 5 min — DKIM (value shown split by DNS into two strings; enter as one) |

That's the complete set — only 4 records at Hover.

## Gaps found (add at Cloudflare, don't exist today)

- **No SPF record.** Add TXT @ `v=spf1 include:_spf.google.com ~all`.
  Without it, mail from @mereeditions.com is more likely to land in spam.
- **No DMARC record.** Optional but cheap: TXT _dmarc
  `v=DMARC1; p=none; rua=mailto:mort@mereeditions.com` (monitor-only to start).

## Cutover order

1. Add mereeditions.com zone to Cloudflare (free plan).
2. Verify all 4 records imported exactly; fix any the scan missed. Gray-cloud everything.
3. Add SPF (and optionally DMARC) records.
4. Switch nameservers at Hover to the pair Cloudflare assigns.
5. Wait for propagation; send + receive a test email at mort@mereeditions.com.
6. Only then: add custom domain www.mereeditions.com to the Worker, apex redirect to www, and delete/replace the old Hover A record.
