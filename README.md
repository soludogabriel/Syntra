# Syntра — AI Automation Landing Page

A complete, production-ready launch package for **Syntра**: landing page, waitlist backend, confirmation emails, and one-command deployment.

---

## What's included

```
syntra/
├── public/
│   ├── index.html        ← Landing page (hero, features, pricing, FAQ, CTA)
│   └── thank-you.html    ← Post-signup confirmation page
├── api/
│   └── waitlist.js       ← Serverless API: saves signups + sends emails
├── vercel.json           ← Routing + security headers config
├── package.json
└── README.md
```

---

## Deploy in 5 minutes (Vercel — free)

### 1. Install Vercel CLI
```bash
npm install -g vercel
```

### 2. Create a Vercel account
Go to [vercel.com](https://vercel.com) and sign up (free).

### 3. Deploy
```bash
cd syntra
vercel --prod
```

Vercel will guide you through linking your account. Your site will be live at a `.vercel.app` URL instantly.

### 4. Add a custom domain (optional)
In the Vercel dashboard → your project → **Settings → Domains** → add `syntra.ai` (or whatever domain you own).

---

## Set up email confirmations (Resend — free)

Emails are sent via [Resend](https://resend.com). Free tier: **3,000 emails/month**.

### Steps:
1. Sign up at [resend.com](https://resend.com)
2. Add and verify your domain (e.g. `syntra.ai`)
3. Create an API key
4. In Vercel dashboard → your project → **Settings → Environment Variables**, add:

| Variable | Value |
|---|---|
| `RESEND_API_KEY` | `re_xxxxxxxxxxxx` |
| `FROM_EMAIL` | `waitlist@syntra.ai` |
| `NOTIFY_EMAIL` | `you@youremail.com` *(get pinged on every signup)* |

5. Redeploy: `vercel --prod`

> **Without these env vars**, the waitlist still works (emails are just skipped silently). Add them when ready.

---

## View your waitlist

The waitlist is saved to `/tmp/syntra_waitlist.json` on the serverless function. To export it properly for production, replace the file-based storage with:

- **[Vercel KV](https://vercel.com/docs/storage/vercel-kv)** — Redis-compatible key-value store (free tier available)
- **[Airtable](https://airtable.com)** — spreadsheet-style database with a great free tier
- **[Supabase](https://supabase.com)** — full Postgres database, free tier available

### Upgrade to Vercel KV (recommended):
```bash
vercel kv create syntra-waitlist
```

Then update `api/waitlist.js`:
```js
import { kv } from '@vercel/kv';

// Replace loadDB/saveDB with:
const emails = await kv.lrange('waitlist', 0, -1);
await kv.rpush('waitlist', JSON.stringify({ email, timestamp }));
const count = await kv.llen('waitlist');
```

---

## Local development

```bash
cd syntra
npm install
vercel dev
```

This runs your serverless functions and static files locally at `http://localhost:3000`.

---

## Customisation checklist

- [ ] Replace placeholder company logos in the "Trusted by" section
- [ ] Update pricing if needed (currently $49 / $149 / Custom)
- [ ] Add your real email to `NOTIFY_EMAIL` env var
- [ ] Set up domain in Vercel dashboard
- [ ] Connect Resend for email delivery
- [ ] Upgrade `/tmp` storage to Vercel KV or Supabase for production persistence
- [ ] Add Google Analytics / Plausible tracking snippet (optional)
- [ ] Update `og:image` meta tag with a real screenshot

---

## Tech stack

| Layer | Tool | Cost |
|---|---|---|
| Hosting | Vercel | Free |
| Serverless API | Vercel Functions (Node.js) | Free |
| Email | Resend | Free (3k/mo) |
| Database | Vercel KV (upgrade) | Free tier |
| Fonts | Google Fonts | Free |
| Domain | Your registrar | ~$10/yr |

**Total cost to launch: $0** (until you need to scale)

---

## Support

Questions? Reply to your waitlist confirmation email or reach out at hello@syntra.ai.
