# Supabase setup — GameCard

Create a Supabase project, then apply a reviewed schema with:
- profiles (customer/admin role; role changes restricted to server-side admin)
- products (up to 15 active products enforced by admin workflow)
- wallet_transactions (append-only ledger; INR amounts in paise)
- payment_submissions (UPI UTR or USDT tx hash, network, amount, status)
- orders and order_items
- card_codes (private; never readable by ordinary customers)
- refunds and admin_audit_logs

Security requirements before launch:
1. Enable RLS on every table.
2. Customers may read only their own profile, transactions, payment submissions, orders and refunds.
3. Customers must never write wallet balances or set payment status to approved.
4. Admin approval and wallet credit must happen in a trusted Edge Function / server transaction, with idempotency protection.
5. Keep card codes in a private table/storage area. Reveal a code only after the corresponding paid order is fulfilled.
6. Keep Supabase service-role secrets only in Edge Function secrets, never in frontend or GitHub.
7. Validate UPI UTRs against the actual merchant bank account. Validate USDT BEP20 transfers on-chain: chain ID 56, token contract, receiving address, amount, tx hash, confirmations, and duplicate use.
8. Refunds are requests until an admin verifies and completes the original payment-method refund. Never credit a refund twice.

The provided BEP20 receiving address should be configured as a server-side setting after independently verifying ownership and doing a small test transfer. Do not commit private keys or seed phrases.
