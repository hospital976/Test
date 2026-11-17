# GameCard — final project starter

Dark green / black digital gaming-card storefront. Built with HTML, CSS, vanilla JS and Supabase.

## Included
- Customer email/password sign-up and login
- Customer wallet display and deposit request submission (manual UPI / USDT proof)
- Product catalogue with ATM-card proportions
- Admin-only product create/edit/activate/deactivate and stock editing
- Admin deposit review queue
- Order and refund request UI foundations
- Supabase SQL schema, RLS policies and storage bucket setup
- Cloudflare Pages / GitHub deployment guide

## Important status
This ZIP is a configurable MVP foundation, **not a live payment system**. Real wallet credits, order fulfilment, refunds and code delivery must be completed through trusted server-side Supabase Edge Functions and tested before accepting customers. The browser never gets the Supabase service-role key.

UPI ID configured: `luciferrx@slc`
USDT network: `BEP20`
USDT receiving address: configured in `app.js` as a public receiving address.
Wallet display currency: INR
Minimum deposit: ₹500
Admin email intended: `arsi100001@gmail.com`
Admin name: Aryan

The UPI ID and public crypto receiving address are payment destination details, not proof that a payment was received. Admin must verify against the actual bank / blockchain record before approval.

## Setup
1. Create a Supabase project.
2. In Supabase SQL Editor, run `supabase/schema.sql`.
3. In Supabase Authentication settings, enable Email provider. For production, configure your site URL and redirect URLs.
4. Create the admin user using the sign-up form with `arsi100001@gmail.com`, then copy that user's UUID from Authentication > Users.
5. Replace `ADMIN_USER_UUID_HERE` in the admin bootstrap SQL section at the bottom of `supabase/schema.sql` and run that one statement:
   `insert into public.user_roles (user_id, role) values ('ADMIN_USER_UUID_HERE', 'admin') on conflict (user_id) do update set role = 'admin';`
   Never expose an admin password or service-role key.
6. Create a Supabase Storage bucket named `product-assets` (public read for product artwork; only admins can upload/update/delete). SQL policies are included.
7. Open `app.js` and replace:
   - `SUPABASE_URL_HERE`
   - `SUPABASE_ANON_KEY_HERE`
   The anon key is designed for browser use when RLS is correctly enabled. Never paste the service-role key here.
8. Upload the project contents to a GitHub repository.
9. In Cloudflare Pages, connect the GitHub repository. Build command: leave blank. Build output directory: `/`.
10. Add your custom domain later in Cloudflare Pages > Custom domains.

## First admin / permissions
Admin role is granted by the explicit SQL statement above. Public signups cannot grant themselves admin. Keep the admin email secure and enable MFA in Supabase when possible.

## Payment / fulfilment safety
- UPI: user submits UTR and optional screenshot; admin checks merchant bank statement.
- USDT BEP20: user submits transaction hash; admin verifies chain, receiving address, token contract, amount, status and confirmations using a trusted BSC explorer / node.
- Do not approve based on screenshot or TXID text alone.
- This frontend deliberately does not credit balances merely because a user submits payment details.
- Do not deliver digital codes until payment is verified and the order is reserved/fulfilled through a server-side operation.
- Refund to original payment method needs the original payment details and a manual payout process. This app records a refund request; it does not send money automatically.
- Confirm the legal / tax / compliance requirements for selling digital goods and accepting crypto in India before going live.

## Supabase SQL
See `supabase/schema.sql`. Review and test policies in a separate test project before production.
