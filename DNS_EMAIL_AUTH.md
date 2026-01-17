PoiseOps DNS Email Authentication Checklist (Cloudflare)
====================================================

This is DNS work (not an index.html change). These records help protect poiseops.com from spoofing and improve deliverability.

1) SPF (Sender Policy Framework)
--------------------------------
Cloudflare Email Routing documents the SPF record as:

v=spf1 include:_spf.mx.cloudflare.net ~all

Reference: Cloudflare Email Routing (Postmaster) docs.

✅ Your screenshot already shows an SPF TXT record on poiseops.com.
IMPORTANT: You should have ONE SPF record total for the root domain.
If you later send email from a provider (Google Workspace, Microsoft 365, etc.), you will need to merge their include into the same TXT record.

2) DKIM
-------
Cloudflare Email Routing adds DKIM signatures and publishes a DKIM key using selector:

cf2024-1._domainkey.poiseops.com

✅ Your screenshot shows a TXT record starting with cf2024-1._domainkey... so DKIM is in place.

3) DMARC (add this)
-------------------
Add a DNS TXT record:

Type: TXT
Name: _dmarc
Content:
v=DMARC1; p=none; rua=mailto:dmarc@poiseops.com; adkim=r; aspf=r; fo=1; pct=100

TTL: Auto

Before you publish it, create an address/route so DMARC reports don't get lost:
- In Cloudflare Email -> Email Routing, add custom address: dmarc@poiseops.com
- Forward it to the inbox you actually read (or to contact@poiseops.com if that is routed).

Rollout plan (recommended)
- Start with p=none (monitoring) for a couple weeks.
- Move to p=quarantine once you confirm legitimate mail passes.
- Move to p=reject when you're confident.

Important note about sending from @poiseops.com
-----------------------------------------------
Cloudflare Email Routing does not support sending from your domain address.
When you reply, it sends from your destination mailbox (example: your Gmail), not from contact@poiseops.com.
If you want to SEND as contact@poiseops.com, you will need a sending provider (Google Workspace, Microsoft 365, etc.) and we’ll adjust SPF/DKIM accordingly.

