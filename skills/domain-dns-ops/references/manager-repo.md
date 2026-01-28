# `~/Projects/manager` pointers

Read when: you need the "how we do it here" for domains/DNS/redirects/Vercel.

## Files

- `DOMAINS.md`: mappings + registrar + hosting (Vercel/Workers) + exclusions.
- `DNS.md`: Cloudflare onboarding + Vercel domain setup + verification steps.
- `scripts/redirect-workers.ts`: Worker implementation (fallback redirect behavior).
- `redirect-worker-mapping.md`: host -> target mapping input.
- `profile`: env vars (Cloudflare, Namecheap, Vercel tokens).
- `bin/namecheap-set-ns`: Namecheap NS flip helper.
- `bin/cloudflare-ai-bots`: bot management helper.

## Fast navigation

- Find a domain: `rg -n "\bexample\.com\b" ~/Projects/manager/DOMAINS.md`
- List scripts: `ls -la ~/Projects/manager/bin`

## Vercel quick ref

```bash
vercel domains ls                           # List all domains
vercel domains add example.com --project x  # Add domain to project
vercel domains inspect example.com          # Check domain status
```
