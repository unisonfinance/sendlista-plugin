---
name: send-email
description: Send or schedule an email through Sendlista. Use when the user asks to send, resend, schedule or cancel an email (a receipt, a reply, a notice, a one-off message) from their own domain, or asks whether a send went through.
---

# Send an email with Sendlista

Sending is real: the email goes to real people and counts towards the user's allowance. The user's own instructions always come first. Never invent an address, a name or a fact to put in an email.

## Steps

1. **Find a domain that can send.** Call `list_domains`. Only a domain whose `status` is `verified` can send. If none is, stop and offer to help connect one (the connect-domain skill). If `list_domains` is refused with `key_not_allowed`, the connection only allows sending: ask the user which address to send from instead.
2. **Settle the details.** You need: `from` (any address on a verified domain, such as `Shop <hello@shop.com>`; it need not be a real mailbox), `to` (one or more addresses), `subject`, and the content as `text`, `html` or both. Ask for anything missing. Do not guess a recipient's address.
3. **Pick the stream.** `transactional` (the default) is for email a person expects because of something they did: receipts, sign-in codes, replies. `marketing` is for newsletters and offers, and only for people who agreed to get them. If you are unsure which it is, ask.
4. **Show it and ask.** Before calling `send_email`, show the user the from address, every recipient, the subject and the full content, and wait for a clear yes. A yes to one email is not a yes to the next.
5. **Send.** Call `send_email` once. To send later, add `send_at` as a time like `2027-01-31T09:00:00Z` (up to 30 days ahead); say the time back to the user in their own time zone.
6. **Report.** Each recipient gets a line in `messages` with an `id` and a `status`. Tell the user plainly:
   - `queued`: accepted and on its way.
   - `suppressed`: not sent, because the address is on the account's blocked list (it bounced, complained or unsubscribed before). Do not try to get around this.
   Keep the message `id` in case the user asks what happened later.

## Afterwards

- "Did it arrive?" Call `get_email` with the id. `delivered` means the other mail server accepted it; it can still be in a spam folder, which no sender can see. `deferred` means the other side said try later, and Sendlista is retrying. `bounced` means the address or server refused it.
- Scheduled email: `list_scheduled_emails` shows what is waiting; `cancel_scheduled_email` stops one that hasn't gone. Cancelling can't be undone, so confirm first.

## When a send is refused

Explain the reason in plain words and do not retry in a loop:

- `domain_not_verified`: the from domain isn't connected yet. Offer the connect-domain skill.
- `hourly_limit`: the account is over its hourly limit. It is a rolling hour; try again later. New accounts start low and the limit rises by itself after clean sending.
- `monthly_limit`: this month's allowance is used up.
- `marketing_not_approved`, `marketing_unavailable`, `marketing_paused`: marketing isn't open for this account or domain right now. Do not resend it as transactional to get around this.
- `account_paused`: sending is paused. The user needs to contact Sendlista from their account (Get help).
- `key_not_allowed`: this connection may only send. To do more, the user disconnects it under AI assistants in their Sendlista account and connects again choosing Everything.

## Do not

- Send the same email to a long list of people one by one as a way to run a campaign. Campaigns are made in the user's Sendlista account, which handles unsubscribes for them.
- Send to addresses the user did not give you.
