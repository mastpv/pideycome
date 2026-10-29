# PideyCome — Multi-tenant POS + CRM for local businesses

**Live:** https://pideycome.com

PideyCome is a SaaS point-of-sale (POS) and CRM that I built and maintain as a solo developer in Almoradí (Alicante, Spain). A single codebase serves several businesses (tenants) with different needs: a functional training gym in production with real paying members, and restaurants in test/onboarding.

> Built by a non-specialist developer with heavy use of AI-assisted coding (Claude). I design the features, test them with real users, deploy and operate the system in production.

## What it does

**Gym (MasFit Club — in production)**
- Member lifecycle: leads → trial → sign-up → active → cancellation, with follow-up checklists per member.
- Recurring billing with Stripe subscriptions (monthly plans, fixed billing day, card saved for members who pay the first month by Bizum).
- Training programming: workout templates with blocks and exercises (drag & drop), three difficulty versions per exercise, coach notes and athlete-facing comments.
- TV mode: workouts displayed on gym screens, one panel per member.
- Member tags (New / Regular / Loyal / Veteran) computed from real months of training.
- Cash register closing, bookings, staff, loyalty and events modules.

**Restaurants (test / onboarding)**
- Public menu by URL per business (`/restaurante/<slug>`), bilingual menus.
- Orders, kitchen screen with per-order timers, QR ordering.
- Dynamic Open Graph so shared links show each restaurant's name and logo.

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML/CSS/JavaScript (single-page app, no framework) |
| Backend / DB | Supabase (PostgreSQL, Row Level Security, Auth) |
| Server logic | Supabase Edge Functions (Deno / TypeScript) |
| Payments | Stripe (Checkout, Subscriptions, Billing, webhooks) |
| Hosting | Cloudflare Workers (static assets) |

## Some problems solved in production

- **Multi-tenancy:** every query is scoped by business ID, and global state is reset when switching tenant so data from one business never flashes in another.
- **Stripe webhooks:** a dedicated Edge Function processes subscription events (`checkout.session.completed`, `invoice.payment_succeeded/failed`, `customer.subscription.updated/deleted`), verifies signatures with a per-tenant secret, and keeps member status and plan in sync — including edge cases like incomplete payments that expire.
- **Pagination bug:** follow-up checkmarks "disappeared" after reopening a tab once a table passed 1,000 rows (the API's default limit). Fixed with a paginated loader applied to every large table, plus proper error checking on writes.
- **Deploy routing:** pretty URLs (`/restaurante/<slug>`) served through rewrite rules without losing the slug.

## Screenshots

_(coming soon)_

## Source code

The application code is kept in a private repository because it is a commercial product in use by real customers. Happy to walk through it on request.
