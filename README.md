# Laundry Manager — setup guide

Built from the Dental City clinic app pattern (login + db-proxy Edge Functions, PWA).
The clinic app and database are not touched by anything here.

## Files
- `index.html` — the app (single file)
- `sw.js`, `manifest.json`, `icon-192.png`, `icon-512.png`, `_headers` — PWA files
- `supabase/schema.sql` — database tables
- `supabase/functions/login/index.ts` — login Edge Function
- `supabase/functions/db-proxy/index.ts` — db-proxy Edge Function

## 1. Supabase (NEW project for the laundry client)
1. Create the project (region: Southeast Asia / Singapore).
2. SQL Editor → paste `supabase/schema.sql` → Run.
3. Edge Functions → create `login`, paste `functions/login/index.ts`, deploy. Do the same for `db-proxy`.
4. Edge Functions → Secrets, add:
   - `DEV_USER` = your developer username
   - `DEV_PASS` = a strong developer password (not the clinic one)
   - `SESSION_SIGNING_SECRET` = a long random string
   - `ALLOWED_ORIGINS` = `https://volts1922.github.io`
5. Project Settings → API: copy the Project URL and publishable key.

## 2. App
1. Open `index.html`, find `CONFIG` near the top of the script, and set:
   - `SHOP_NAME`, `_SB_URL`, `_SB_API_KEY` (the NEW project's values)
2. Create a new GitHub repo (e.g. `laundry_app`), upload all files except the `supabase` folder.
3. Settings → Pages → deploy from `main`. Live at `https://volts1922.github.io/laundry_app/`.

## 3. First login
1. Log in with `DEV_USER` / `DEV_PASS`.
2. Settings → edit "Branch 1", add the other branches (each needs a short ticket code like BR2).
3. Settings → check Services & prices.
4. Settings → Add account → role **Owner** for the client, then staff per branch.

## Roles
- **Owner / Admin**: all branches, reports, settings, prices
- **BranchAdmin**: own branch, reports, can add Staff, can cancel orders
- **Staff**: own branch, new orders, board, payments, customers, expenses

## Updating later
Always upload `index.html` and `sw.js` together, and bump `CACHE_VER` in `sw.js`
(e.g. `laundry-app-v2`) so the "new version" banner shows on every device.
