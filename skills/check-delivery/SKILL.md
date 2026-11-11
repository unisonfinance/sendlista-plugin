---
name: check-delivery
description: Find out what happened to an email sent through Sendlista, or how sending is going overall. Use when the user asks why an email didn't arrive, whether something was delivered, about bounces, complaints, spam, blocked addresses, open or click numbers, or how much of their sending allowance is left.
---

# Check on delivery with Sendlista

Answer from what the tools return. Never guess a status, and never say an email reached the inbox: no sender can see that.

## One email

1. Find it. If you have the message id, call `get_email`. Otherwise call `list_emails` with `q` set to the recipient's address or part of the subject (and `status` if the user named one), and pick the matching one. If several match, ask which.
2. Explain its status in plain words:
   - `queued`: accepted by Sendlista, about to be handed over.
   - `delivered`: the recipient's mail server accepted it. If they can't see it, it's most likely in spam or another tab; ask them to search for the subject.
   - `deferred`: the recipient's server said "try again later". Sendlista keeps retrying by itself for up to three days. Nothing to do yet.
   - `bounced`: refused. The address doesn't exist, the mailbox is full, or the server rejected it. The reason is in the status detail; quote it and say what it means.
   - `complained`: the recipient marked it as spam. Sendlista stops sending to them.
   - `suppressed`: never sent, because the address was already on the blocked list.
   - `failed`: it could not be sent; the detail says why.
3. If it bounced or was suppressed, call `list_blocked_addresses` to show why the address is blocked (bounced, complained, unsubscribed or blocked by hand). An address the user knows is good can be unblocked in their Sendlista account, under Blocked addresses. Do not suggest sending from another address to get around a block.

## Sending overall

Call `get_stats`. It gives totals by status and day for the last 30 days, opens and clicks, the plan, and how much of this month's and this hour's allowance is used.

- Give the few numbers that answer the question, not the whole answer.
- Bounces above about 5% or spam complaints above about 0.1% of recent email put the account at risk of being paused. If the figures are near that, say so, and suggest checking the list (the check-addresses skill) before the next send.
- Opens can read much higher than the truth, because some mail apps load pictures by themselves. Clicks are the better signal. Say this if the user leans on open rates.
- If nothing is delivered at all, call `list_domains`: a domain that isn't `verified` can't send.

## If a tool is refused

`key_not_allowed` means this connection may only send, so it can't read the account. `get_email` and `list_scheduled_emails` still work. For the rest, the user looks under Activity in their Sendlista account, or disconnects this assistant under AI assistants and connects it again choosing Everything.

## Do not

- Paste raw tool output. Summarise it, and show an address only when it's the one the user asked about.
- Promise that a change will fix spam placement. Say what is known and what isn't.
