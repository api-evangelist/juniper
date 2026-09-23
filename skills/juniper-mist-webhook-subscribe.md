---
name: juniper-mist-webhook-subscribe
description: Subscribe an organization or site to Mist events over a signed HTTP webhook, prove delivery, and audit what was sent.
api: Mist API
provider: juniper
generated: '2026-09-18'
method: generated
source: openapi/juniper-mist-api-openapi.yml + asyncapi/juniper-mist-webhooks.yml
operations:
  - listWebhookTopics
  - createOrgWebhook
  - createSiteWebhook
  - pingOrgWebhook
  - pingSiteWebhook
  - searchOrgWebhooksDeliveries
  - countOrgWebhooksDeliveries
  - updateOrgWebhook
  - deleteOrgWebhook
---

# Subscribe to Mist events with a signed webhook

Mist pushes 30 event topics (alarms, audits, device up/down, client joins, NAC events, location…) to a URL you register. Webhooks exist at **two scopes** — org and site — with the same object shape.

## Before you start

- Authenticate with `Authorization: Token {apitoken}` (see `authentication/juniper-authentication.yml`). An org API token is preferred; an admin token expires after 90 idle days.
- You are limited to **5,000 calls per hour per token**; a 429 carries `Retry-After`. Webhooks are the way to *avoid* polling, so this skill spends very few calls.
- There is **no idempotency key**. A retried `createOrgWebhook` makes a second webhook. List before you create.

## Steps

1. **Discover the topic list from the API itself** — `GET /api/v1/const/webhook_topics` (`listWebhookTopics`). Use `for_org` to pick topics valid at org scope, and `allows_single_event_per_message` before setting that flag.
2. **Check for an existing subscription** — `listOrgWebhooks` / `listSiteWebhooks`, matching on `url`. Do not create a duplicate.
3. **Create the webhook** — `POST /api/v1/orgs/{org_id}/webhooks` (`createOrgWebhook`) with:
   - `type: http-post`
   - `url`: your HTTPS receiver
   - `topics`: e.g. `["alarms","device-updowns","audits"]`
   - `secret`: a random string — Mist will then sign every delivery with `X-Mist-Signature-v2: HMAC_SHA256(secret, body)`.
   - `verify_cert: true` (default) — keep it.
4. **Prove delivery** — `POST .../webhooks/{webhook_id}/ping` (`pingOrgWebhook`). Your receiver should get a `ping` topic message; verify the HMAC before trusting it.
5. **Audit** — `searchOrgWebhooksDeliveries` / `countOrgWebhooksDeliveries` return `webhook_delivery` records. Delivery search is only available for topics flagged `has_delivery_results` (alarms, audits, device-updowns, occupancy-alerts, ping).
6. **Change or remove** — `updateOrgWebhook` (PUT, full object) or `deleteOrgWebhook`. Delete is **irreversible**; there is no restore.

## Receiver rules

- Multiple events arrive per message (`events[]`) unless `single_event_per_message: true`.
- Verify `X-Mist-Signature-v2` (SHA-256); ignore the SHA-1 `X-Mist-Signature`.
- Rogue-AP events are rate-limited by Mist to once per 10 hours; rogue-client/honeypot to once per 10 minutes.

## Errors

`{ "detail": "…" }` on 400/401/403/404/429; webhook 400s add `reason`. No machine-readable codes — branch on status. See `errors/juniper-problem-types.yml`.
