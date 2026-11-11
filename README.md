# Sendlista

Send email from your own domain, and see what happened to it, by asking your AI assistant in plain words.

This plugin has two parts:

- **The Sendlista connector.** It connects your assistant to your own Sendlista account. You sign in to Sendlista, choose what the assistant may do ("Only send emails" or "Everything"), and press Allow.
- **Four skills.** They teach the assistant how to do the common jobs properly: send an email safely, connect a domain, find out why an email didn't arrive, and check a list of addresses before sending to it.

## What you need

A Sendlista account (free to start at https://sendlista.com) with a domain connected. The assistant can help you connect the domain.

## Use it

After installing, connect the Sendlista connector and sign in. Then ask things like:

- "Send Ann at ann@example.com her receipt again, from hello@myshop.com."
- "Is myshop.com ready to send email? What's still missing?"
- "Why didn't my email to bob@example.com arrive?"
- "How many emails bounced this week?"
- "Check these 200 addresses before I send to them."

The assistant shows you who an email goes to, the subject and the text, and asks you first. Emails it sends are real and count towards your plan's allowance.

## What it can't do

It can't see or change billing, your team, your password or your API keys. It doesn't create or send campaigns, or change audience lists: those stay in your Sendlista account.

## Data

The plugin itself stores nothing and runs no code on your computer. Through the connector at api.sendlista.com, your assistant sends Sendlista what you ask it to: the emails you send (addresses, subject, content), domain names you add, and addresses you ask it to check. Sendlista answers with information from your own account: your emails and their delivery status, sending figures, domains and their DNS records, templates and blocked addresses. Nothing is sent anywhere else. Privacy policy: https://sendlista.com/privacy

## Disconnect

In your Sendlista account, open **AI assistants** and press **Disconnect**. It stops working straight away.

## Help

Step-by-step guide: https://sendlista.com/docs/ai-assistants. Questions: https://sendlista.com/contact

## License

MIT
