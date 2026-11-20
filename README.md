# HALLOFRESH Grocery Mart

A grocery delivery platform for Nairobi. Customers order fresh groceries and pay with M-Pesa, riders deliver, and staff manage stock, orders and promotions from one admin dashboard. It runs as a web app, an installable PWA and an Android/iOS app (Capacitor).

**Live:** [hallofreshmart.store](https://hallofreshmart.store)

## Features

**Customers**
- Browse products by category and branch, with favourites and search
- Cart, promo codes and delivery fees calculated by zone
- M-Pesa STK Push checkout with automatic payment confirmation
- Order tracking and live chat with support
- Phone and email OTP sign-in
- AI shopping assistant and product translation (English / Somali)
- Back-in-stock and promotion notifications

**Riders**
- Rider dashboard for assigned deliveries
- Chat with customers and the store

**Admin / staff**
- Dashboard with sales reports
- Inventory with low-stock alerts
- Order, rider and user management with role-based access
- Promotions, promo codes and a weekly deals digest

## Tech stack

| Layer | Tools |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS, shadcn/ui, TanStack Query |
| Backend | Supabase (Postgres, Auth, Row Level Security, Storage, Realtime) |
| Serverless | 16 Supabase Edge Functions (Deno) |
| Payments | Safaricom M-Pesa Daraja API (STK Push, callback, status query) |
| Mobile | Capacitor (Android / iOS), PWA |
| Hosting | Ubuntu VPS with Nginx (`deploy.sh`, `vps-setup.sh`, `nginx.conf`) |
| Testing | Vitest |

## Architecture

```
React app (web / PWA / Capacitor)
        │
        ▼
Supabase ── Postgres + RLS (45 migrations)
        │── Auth (phone + email OTP)
        │── Realtime (chat, order status)
        └── Edge Functions
              ├── mpesa-stk-push / mpesa-callback / mpesa-query
              ├── send-otp / verify-otp / send-email-otp / verify-email-otp
              ├── check-low-stock / notify-back-in-stock
              ├── send-promo-notifications / weekly-deals-digest
              ├── hallofresh-assistant / translate-text / generate-product-image
              └── create-user / staff-password-reset
```

## Getting started

```bash
git clone https://github.com/ibraanwar7565-ai/Hallo-Fresh-Group.git
cd Hallo-Fresh-Group
npm install
```

Create a `.env` file:

```
VITE_SUPABASE_URL=your-project-url
VITE_SUPABASE_PUBLISHABLE_KEY=your-anon-key
VITE_SUPABASE_PROJECT_ID=your-project-id
```

Edge Function secrets (set in Supabase, never commit them): `MPESA_CONSUMER_KEY`, `MPESA_CONSUMER_SECRET`, `MPESA_PASSKEY`, `MPESA_SHORTCODE`, `SUPABASE_SERVICE_ROLE_KEY`.

```bash
npm run dev      # local dev server
npm run test     # run tests
npm run build    # production build
```

## Deployment

```bash
bash vps-setup.sh   # one-time server setup (Nginx, SSL)
bash deploy.sh      # build and upload to the VPS
```

Android build:

```bash
npm run build && npx cap sync android && npx cap open android
```

## Author

**Abdirahim Ibrahim** — [NexaCore Technologies](https://nexacoretechnologies.com), Nairobi
