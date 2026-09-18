# Multi-tenant ecommerce

Two Next.js apps and one Express API:

| App | URL | Role |
| --- | --- | --- |
| Storefront + agency admin | http://{slug}.localhost:3000 | Customers shop here. Agency staff use `/admin`. |
| Super admin | http://localhost:3004/super-admin/dashboard | Creates and controls agencies |
| API | http://localhost:4000 | Auth, products, orders, Stripe, chatbot, email |

## Stack

- Frontend: Next.js, TypeScript, Tailwind, shadcn-style UI
- Backend: Node.js, Express, Knex, PostgreSQL, TypeScript

## Setup

1. Copy env values into `.env` (root). The keys below are already wired through the API:

```
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_UPLOAD_PRESET=
CLOUDINARY_WORKSPACE_UPLOAD_PRESET=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
SUPER_ADMIN_LOGIN_EMAIL=
SUPER_ADMIN_PASS=
JWT_SECRET=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
SMTP_HOST=
SMTP_PORT=
SMTP_SECURE=
SMTP_USER=
SMTP_PASS=
OPENROUTER_API_KEY=
DATABASE_URL=
```

2. Start Postgres (Docker or local):

```bash
docker compose up -d
```

Default `DATABASE_URL` is `postgres://postgres:postgres@localhost:5432/ecommerce`.

3. Install, migrate, and run:

```bash
npm install
npm run migrate
npm run dev
```

## First run

Dev servers are started with `npm run dev` (API `:4000`, shop `:3000`, super admin `:3004`).

1. Open http://localhost:3004/super-admin/login
2. Sign in with `SUPER_ADMIN_LOGIN_EMAIL` / `SUPER_ADMIN_PASS`
3. Create an agency (slug, brand name, admin email/password)
4. Open the storefront at `http://YOUR_SLUG.localhost:3000` (example: http://lumen.localhost:3000)
5. Agency login lives at http://YOUR_SLUG.localhost:3000/admin

Chrome and Firefox resolve `*.localhost` to your machine automatically. If a browser does not, add `127.0.0.1 lumen.localhost` to `/etc/hosts`.

A local `.env` is already created with development defaults (`superadmin@local.test` / `superadmin`). Replace those and the empty Cloudinary, Stripe, SMTP, and OpenRouter keys before using image drafts, payments, or invoice email.

If Postgres is already running on port 5432, skip Docker and create a database named `ecommerce` instead.

## MVP flows

- **Products:** `/admin/products/new` uploads an image to Cloudinary, OpenRouter drafts the catalog copy, then the admin confirms price and stock.
- **Orders:** paid checkouts become `ordered`. Admins can relabel them `lead`, `ordered`, `dispatched`, `delivered`, or `cancelled`.
- **Payments:** cart checkout creates a Stripe Checkout session. After payment, stock is decremented and an invoice email is sent.
- **Chatbot:** storefront widget answers from live published products and stock.
- **Branding:** each agency sets logo, brand name, color, and contact details. Those values appear on the shop and invoices.

## Stripe webhook (optional locally)

The success page also confirms the session against Stripe, so local checkout works without Stripe CLI. For production, point Stripe at:

```
POST http://localhost:4000/api/webhooks/stripe
```
# Stapnote
