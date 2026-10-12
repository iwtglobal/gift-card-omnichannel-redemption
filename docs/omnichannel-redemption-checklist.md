# Omnichannel Redemption Checklist

Practical checklist for launching **gift card redemption** across web, POS, and app on one ledger.

## 1. Shared API requirements

| Capability | Required | Notes |
|------------|----------|-------|
| Balance inquiry | Yes | Read-only; no debit |
| Auth hold | Yes | Amount + TTL + idempotency key |
| Capture | Yes | Against hold or direct with key |
| Void / release | Yes | Unwind unused holds |
| Status inquiry | Yes | Active / frozen / expired / depleted |
| Remaining balance | Yes | Returned after every capture |

## 2. Channel go-live checklist

- [ ] Web and app use tokenised credentials, not raw PIN in storage
- [ ] POS timeout and retry use the same idempotency key
- [ ] Hold TTL documented for cashiers and checkout UX
- [ ] Freeze propagation tested under 1 minute across channels
- [ ] Partial redemption receipts show amount used and left
- [ ] Offline POS policy written (decline vs limited store-and-forward)

## 3. Race and failure tests

- [ ] Concurrent web + POS auth on same card
- [ ] Capture retry after network drop
- [ ] Hold expiry during abandoned cart
- [ ] Freeze between inquiry and capture

## 4. Reconciliation

- Daily match of channel settlement to ledger captures
- Exception queue for orphan holds and late voids
- Clear owner for multi-channel disputes

## Related

See the main [README](../README.md). Product context: [voucher management system](https://evdsystem.com/voucher-management-system-mobogage/).

## 5. Support playbook notes

- Train agents to ask which channel failed (web, POS, app) and for the auth/hold ID
- Never manually "fix" balance in a channel DB — correct via ledger adjustments
- Keep a cross-channel freeze contact list for fraud events outside business hours
