---
name: sendapi
description: Send WhatsApp messages, SMS, OTP verification codes, and transactional email through SendAPI. Use when the user wants to send a message, notification, alert, reminder, verification/2FA code, or email to one or many recipients, or to check delivery status. Works through the SendAPI MCP server tools when available, or the SendAPI REST API.
license: MIT
---

# SendAPI

Send messages across WhatsApp, SMS, OTP, and email through one API. This skill tells you
*how* to send well; the SendAPI MCP server gives you the tools to actually send.

## Before you send

1. **Check for the MCP tools first.** If the SendAPI MCP server is connected, you will see
   tools like `send_sms`, `send_whatsapp`, `send_email`, `send_otp`, `check_otp`,
   `get_usage`, and `list_whatsapp_sessions`. Prefer these — they handle auth for you.
2. **No MCP tools? Use the REST API.** Base URL `https://sendapi.co/v1`, auth header
   `Authorization: Bearer $SENDAPI_API_KEY`. Read the key from the environment; never hardcode
   or print it. See [references/api.md](references/api.md) for the endpoint contract.
3. **Confirm the recipient and content with the user before sending anything real.** Sends
   cost money and reach real people.

## Choosing a channel

- **SMS** — short alerts, codes, anything that must reach any phone. `send_sms` / `POST /sms/send`.
- **WhatsApp** — richer notifications to people who use WhatsApp. Requires a connected session
  (see below). `send_whatsapp` / `POST /whatsapp/send`.
- **Email** — receipts, summaries, anything long-form. `send_email` / `POST /email/send`.
- **OTP / 2FA** — use the dedicated verify flow, not a hand-rolled SMS. `send_otp` then `check_otp`.
- If the user is unsure, ask. Don't guess across channels.

## Sending

**SMS:** needs `to` (E.164, e.g. `+15551234567`) and `content`. Optional `sender_id` if approved.

**WhatsApp:** needs `session_id`, `to`, and `content`. If you don't have a session id, call
`list_whatsapp_sessions` first. If there are none, tell the user to connect a number in the
SendAPI dashboard (or fetch the pairing QR with `get_whatsapp_session_qr` and have them scan it).

**Email:** needs `to` and `subject`, plus `html` or `text` (or a `template_id` with `variables`).
For production sending the `from` domain must be verified — `list_email_domains` shows status.

**OTP:** `send_otp` with `to` and `channel` (`sms`, `whatsapp`, `email`, or `auto`). Verify the
user's input with `check_otp` using the same `to` and the `code` they entered. Never invent or
guess a code, and never echo a code you generated.

## Bulk sends — be careful

`send_bulk_sms`, `send_bulk_whatsapp`, and `send_bulk_email` reach up to 500 recipients at once.
**Always** show the user the recipient count and the message, and get explicit confirmation
before calling a bulk tool. Suggest `get_usage` first to confirm there's enough quota.

## Handling errors

SendAPI returns a clear `error.code` you should act on, not just retry:

- `sending_limited` / rate limit — the account hit a cap. Tell the user; don't hammer retries.
- `content_blocked` — the message tripped abuse/spam filters. Revise the content with the user.
- `invalid_from_domain` / unverified sender — for email, the domain isn't verified yet.
- `4xx` validation — fix the field the error names (bad phone format, missing subject, etc.).

Surface the error plainly and propose the fix. Do not loop retrying the same failing call.

## Good practice

- Phone numbers in international E.164 format (`+` and country code).
- Keep SMS concise; long messages split into multiple segments and cost more.
- Only message people who expect to hear from you. Bulk blasts to cold lists get numbers
  flagged and accounts limited.
- After sending, you can confirm with `get_sms_status` / `get_email_status` / `get_whatsapp_message`.
