# GAVRO Hairstyle – Netlify + Supabase

## 1. Supabase
Use the existing Supabase project `mdrbsxeepcqmfvnrrrtx`. Obtain its **database password** from the Supabase dashboard (or reset it in Database settings). The database is initialized automatically with the original `data/db.json` on the first API request.

## 2. Netlify
Deploy this folder from GitHub (recommended) or upload the ZIP using Netlify's project import workflow that supports functions and dependency installation. **Dragging the `public/` directory onto Netlify Drop deploys only the static site and will not install the API.**

Build settings: publish directory `public`, functions directory `netlify/functions`, build command `npm run build`, Node.js 22.

Set Environment Variables in Netlify dashboard:
- `DATABASE_URL` = `postgresql://postgres.mdrbsxeepcqmfvnrrrtx:<YOUR_DB_PASSWORD>@aws-0-eu-west-1.pooler.supabase.com:6543/postgres` (URL-encode special password characters).
- `ADMIN_USER` = a chosen admin username (default `admin`).
- `ADMIN_PASS` = a **unique, long, secret password** (minimum 16 characters recommended).
- `SALON_TIME_ZONE` = `Europe/Belgrade`.

DO NOT place passwords, service-role keys or DATABASE_URL in `public`, frontend JS, GitHub, or `netlify.toml`.

## 3. Verify
- Visit `https://YOUR-SITE.netlify.app/api/public` for salon data.
- Book a test slot on the homepage, then see it in `/admin`.
- Add an image via admin and confirm it still loads after redeploy.

## Storage and limitations
The booking data, settings, services, staff, gallery and image uploads are stored in PostgreSQL. Netlify functions are stateless and use a PostgreSQL transaction lock for sequential booking writes. Live status refresh is polling rather than SSE. Image sizes are capped by the current server validation and Netlify request payload limits; compress large images before uploading. The original backend logic is retained behind the serverless API adapter. No live Supabase connection was possible without the database password; database connection needs verification after deployment.
