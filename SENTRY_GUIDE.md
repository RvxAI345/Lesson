# Sentry Setup Guide
> Error reporting & feedback loops for the Lesson project.

---

## 1. Create Your Account & Project

1. Go to [sentry.io](https://sentry.io) → **Sign up** (free tier is fine to start)
2. Create an **Organization** (use your team/project name)
3. Create a **Project** → choose your platform (Next.js, React, Node, etc.)
4. Copy your **DSN** from: *Settings → Projects → [your project] → Client Keys (DSN)*

---

## 2. Install the SDK

**Next.js (recommended — wizard handles everything):**
```bash
npx @sentry/wizard@latest -i nextjs
```
The wizard auto-creates config files, updates `next.config.js`, and stubs your `.env`.

**React (manual):**
```bash
npm install @sentry/react
```

**Node / Express:**
```bash
npm install @sentry/node
```

---

## 3. Environment Variables

Add to `.env.local` (never commit to git):
```env
NEXT_PUBLIC_SENTRY_DSN=https://xxxx@o0.ingest.sentry.io/0
SENTRY_AUTH_TOKEN=sntrys_your_token_here   # for source maps upload
SENTRY_ORG=your-org-slug
SENTRY_PROJECT=your-project-slug
```

Add `SENTRY_AUTH_TOKEN` to your hosting provider's secret env vars (Vercel, Railway, etc.).

---

## 4. Initialize Sentry

**`sentry.client.config.ts`** (browser errors):
```typescript
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,   // "production" | "development"
  tracesSampleRate: 1.0,               // lower to 0.1–0.2 in prod under load
  replaysOnErrorSampleRate: 1.0,
  replaysSessionSampleRate: 0.1,
  integrations: [Sentry.replayIntegration()],
});
```

**`sentry.server.config.ts`** (server & API route errors):
```typescript
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
});
```

---

## 5. Source Maps (readable stack traces)

The wizard handles this via `withSentryConfig` in `next.config.js`:
```javascript
const { withSentryConfig } = require("@sentry/nextjs");

module.exports = withSentryConfig(nextConfig, {
  org: process.env.SENTRY_ORG,
  project: process.env.SENTRY_PROJECT,
  authToken: process.env.SENTRY_AUTH_TOKEN,
  hideSourceMaps: true,     // don't expose source to users
  widenClientFileUpload: true,
});
```

---

## 6. User Feedback Widget

Add anywhere in your app to let users report issues:
```typescript
import * as Sentry from "@sentry/nextjs";

// Trigger programmatically after catching an error
Sentry.showReportDialog({ eventId: Sentry.lastEventId() });
```

Or use the built-in feedback button via Sentry's **User Feedback** integration in the dashboard under *Settings → Integrations*.

---

## 7. Alert Rules to Set Up

In Sentry dashboard → **Alerts → Create Alert**:

| Alert | Condition | Why |
|-------|-----------|-----|
| New issue | First occurrence | Catch new bugs immediately |
| Error spike | >10 events/hour | Detect regressions post-deploy |
| Crash rate | >1% of sessions | Monitor stability |

Notify via: **Email** and/or **Slack** (connect under *Settings → Integrations → Slack*).

---

## 8. Test It

Trigger a test error to confirm everything works:
```typescript
// Drop this temporarily in a page or component
throw new Error("Sentry test error — delete me");
```

Then check your Sentry dashboard → **Issues** — it should appear within seconds.

---

## 9. Key Things to Remember

- ✅ DSN is **safe to expose** client-side (`NEXT_PUBLIC_`)
- 🔒 `SENTRY_AUTH_TOKEN` is **secret** — never commit it
- 🏷️ Always tag `environment` so prod vs. dev issues are separated
- 📉 Lower `tracesSampleRate` to `0.1` once you have real traffic
- 🔗 GitHub issue: **#1 — Set up Sentry for error reporting & feedback loops**
