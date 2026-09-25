# MPR JEWELLERY

A full-stack Indian jewellery storefront with an owner console, PostgreSQL/Supabase media, server-authoritative checkout pricing, and manual UPI verification. **No UPI PIN, OTP, or bank credential is ever collected.**

## Stack
- React 18 + TypeScript + Tailwind CSS + Vite
- Node.js + Express + TypeScript + PostgreSQL
- Supabase Storage through the backend service role

## Local installation
1. Create a PostgreSQL database named `mpr_jewellery`, or create a Supabase project and copy its direct Postgres connection string.
2. In Supabase Storage create a **private** bucket named `jewellery-media` (or use another name in `SUPABASE_BUCKET`).
3. Copy `.env.example` to `.env`; set a unique `JWT_SECRET`, database URL, Supabase URL/key, and production CORS origin. Never commit it.
4. Copy `.env.example` values required by the frontend into `frontend/.env` as `VITE_API_URL=http://localhost:4000/api`.
5. Run `npm install`, then `npm run db:migrate`, `npm run seed`, and `npm run dev`.
6. Open `http://localhost:5173`. The owner login is `/owner/login`. The seed script reads `OWNER_EMAIL` and `OWNER_PASSWORD` from the server `.env`, hashes the password, and never sends it to the browser.

## Production deployment
- Deploy `frontend` as static files (Vercel/Netlify/Cloudflare Pages) with `VITE_API_URL` set to the HTTPS API URL.
- Deploy `backend` to a Node host; run `npm run build -w backend` then `npm start -w backend`. Set all server variables in the host secret manager.
- Use managed Postgres/Supabase, HTTPS, a strong random JWT secret, secure cookies, and a narrow `CORS_ORIGIN`.
- Add a custom domain, configure CSP/CORS, database backups, monitoring, and a real payment-provider webhook before enabling automatic payment confirmation.

## Security model
- Owner endpoints require a signed, httpOnly JWT cookie. Passwords are bcrypt hashes.
- All request bodies are validated with Zod; owner price fields are only accepted on protected owner routes.
- Checkout re-reads product price and availability inside a database transaction; the browser total is ignored.
- Files pass through authenticated backend upload, have MIME/size limits, are stored in a private bucket, and are served by short-lived signed URLs.
- UPI links are generated only from server-calculated totals. `payment_claimed` means **pending verification**, not paid.

## GitHub
This folder is Git-ready (`.gitignore` included). Create an empty repository and run `git init && git add . && git commit -m "Initial MPR Jewellery" && git remote add origin <repository-url> && git push -u origin main`. A GitHub connection/repository was not available in this session, so no repository link can be created yet.
