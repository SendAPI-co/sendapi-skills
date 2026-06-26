# SendAPI REST reference (for when MCP tools aren't connected)

- **Base URL:** `https://sendapi.co/v1`
- **Auth:** header `Authorization: Bearer $SENDAPI_API_KEY`
- **Content-Type:** `application/json`
- **Responses:** success is wrapped as `{ "data": ... }`; errors as `{ "error": { "message", "code" } }`

## Send

| Action | Method + path | Body |
|---|---|---|
| SMS | `POST /sms/send` | `{ "to", "content", "sender_id?" }` |
| Bulk SMS | `POST /sms/send-bulk` | `{ "recipients": [...], "content", "sender_id?" }` |
| WhatsApp | `POST /whatsapp/send` | `{ "session_id", "to", "content", "type?" }` |
| Bulk WhatsApp | `POST /whatsapp/send-bulk` | `{ "session_id", "recipients": [...], "content", "type?" }` |
| Email | `POST /email/send` | `{ "to", "subject", "html?"|"text?", "from?", "template_id?", "variables?" }` |
| Bulk Email | `POST /email/send-bulk` | `{ "recipients": [...], "subject", "html?"|"text?", "from?" }` |
| Send OTP | `POST /verify/send` | `{ "to", "channel?": "sms|whatsapp|email|auto", "length?", "ttl?" }` |
| Check OTP | `POST /verify/check` | `{ "to", "code" }` |

## Read

| Action | Method + path |
|---|---|
| SMS status | `GET /sms/status/{id}` |
| Email status | `GET /email/status/{id}` |
| WhatsApp message | `GET /whatsapp/messages/{id}` |
| Validate phone | `GET /phone/validate?number=+15551234567` |
| Account usage / quota | `GET /account/usage` |
| List WhatsApp sessions | `GET /whatsapp/sessions` |
| WhatsApp pairing QR | `GET /whatsapp/sessions/{id}/qr` |
| List sender IDs | `GET /sms/sender-ids` |
| List email domains | `GET /email/domains` |
| List email templates | `GET /email/templates` |

## Example

```bash
curl -X POST https://sendapi.co/v1/sms/send \
  -H "Authorization: Bearer $SENDAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"to":"+15551234567","content":"Your document is ready."}'
```

Full machine-readable contract: [`api/openapi.yaml`](../../../api/openapi.yaml).
