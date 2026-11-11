---
name: check-addresses
description: Check email addresses with Sendlista before sending to them. Use when the user has a list of addresses (pasted, or in a file) and wants to clean it, find typos or bad addresses, lower bounces, or asks whether an address is valid.
---

# Check addresses before sending

Bad addresses bounce, and too many bounces get an account paused. This check finds the ones that are sure to fail, without sending anything.

## Steps

1. **Collect the addresses.** Take them from what the user pasted or from their file. Remove exact duplicates. Do not add, guess or "fix" any address yourself.
2. **Mind the allowance.** Each address checked counts towards the plan's monthly checks, and one call takes up to 1,000 addresses. For more than 1,000, tell the user how many calls it will take and ask before starting; then go in batches of up to 1,000.
3. **Check.** Call `check_addresses` with `emails`. If it is refused because the month's checks are used up, say how many are left (the message says) and offer to check only that many.
4. **Report.** The answer has a result for each address and a count of each kind:
   - `deliverable_likely`: nothing wrong was found.
   - `risky`: might work, but something is off, such as a throwaway-address provider or a provider name that looks mistyped (`gmial.com` for `gmail.com`). When the answer has a `suggestion`, show it, but let the user decide; do not change the address yourself.
   - `undeliverable`: will not arrive: badly written, or the domain can't receive email.
   Each result has `reasons` with a plain message. Lead with the counts, then list the undeliverable and risky addresses with the reason for each. For a long list, offer the clean list back as a file or a block they can copy.
5. **Say what it can't tell.** This check never contacts the other mail server, on purpose. So it cannot see a closed or full mailbox at a working domain: an address marked `deliverable_likely` can still bounce. For an old list, suggest sending to a small part first.

## Do not

- Send a test email to find out whether an address works.
- Call the list "verified" or "safe". Say what was checked.

If `check_addresses` is refused with `key_not_allowed`, the connection only allows sending. The user can run the check under Check addresses in their Sendlista account, or disconnect this assistant under AI assistants and connect it again choosing Everything.
