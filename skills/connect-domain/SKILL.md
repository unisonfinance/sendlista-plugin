---
name: connect-domain
description: Connect a domain to Sendlista so it can send email, or find out why a domain isn't ready. Use when the user wants to start sending from their own domain, asks about DNS records, SPF, DKIM or DMARC for Sendlista, or a send failed because the domain is not verified.
---

# Connect a domain to Sendlista

A domain is the part after the @, like `myshop.com`. Before Sendlista can send from it, the user adds two records where the domain's DNS is managed. This proves the domain is theirs and lets inboxes trust the email. Most people find this the hard part, so go one step at a time and use plain words.

## Steps

1. **See what exists.** Call `list_domains`. If the domain is already there, skip to step 3. If its `status` is `verified`, tell the user it's ready and stop.
2. **Add it.** Confirm the exact domain with the user, then call `add_domain` with `name`. A subdomain such as `mail.myshop.com` is fine. If it's refused because the plan's domain limit is reached, say so; the user changes plan in their account.
3. **Show the records.** The answer has a `records` list. Each record has a `type` (TXT or CNAME), a `host` (the name), a `value`, `required`, and `ok` (whether Sendlista can see it yet). Present each record that is `required` and not yet `ok` as three things to copy: type, name, value. Copy the values exactly, every character; never shorten or retype a DKIM value.
   - `spf`: says Sendlista may send for this domain. A domain can only have one SPF record. If the answer has `spf_existing: true`, the value shown already merges theirs with ours: tell the user to **replace** their existing SPF record's value with it, not add a second one.
   - `dkim`: a signature key, so inboxes can tell the email wasn't changed.
   - Records with `required: false` (`return_path`, `dmarc`, `tracking`) are optional extras. Mention them only after the required ones work.
4. **Say where to add them.** If the answer has a `provider` (the company that runs the domain's DNS, such as Cloudflare or GoDaddy), name it: the records go in that company's DNS settings. Many DNS screens add the domain name by themselves, so if the name box already ends in the domain, the user types only the part before it (for `sendlista._domainkey.myshop.com`, that's `sendlista._domainkey`; for the domain itself, `@`). If someone else manages their domain, offer to write a short message with the records for that person.
5. **Check.** When the user says the records are in, call `check_domain` with the domain's `id`. Report each record as found or not yet found, from `ok`. When `status` is `verified`, say the domain is ready.
6. **If something isn't found.** DNS changes can take from a few minutes to a few hours to show. Sendlista also keeps checking by itself and emails the account owner when the domain is ready. Before waiting, check the usual slips: the value was cut short, the name has the domain twice (`sendlista._domainkey.myshop.com.myshop.com`), a second SPF record was added instead of replacing the first, or the record went on a different domain.

## Do not

- Invent or reconstruct record values. Only ever show what the tool returned.
- Tell the user to remove records they already have for other services, apart from merging SPF as described.
- Call `check_domain` over and over. Once after the user says they're done, then again only when they ask or after a real wait.

If `list_domains` or `add_domain` is refused with `key_not_allowed`, the connection only allows sending. The user can connect the domain under Domains in their Sendlista account, or disconnect this assistant under AI assistants and connect it again choosing Everything.
