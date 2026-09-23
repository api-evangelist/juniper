---
name: juniper-mist-site-onboard
description: Create a Mist site, claim devices into the organization inventory, assign them to the site, and configure a first WLAN.
api: Mist API
provider: juniper
generated: '2026-09-18'
method: generated
source: openapi/juniper-mist-api-openapi.yml + data-model/juniper-data-model.yml
operations:
  - getSelf
  - listOrgSites
  - createOrgSite
  - claimOrgLicense
  - addOrgInventory
  - searchOrgInventory
  - updateOrgInventoryAssignment
  - listSiteWlans
  - createSiteWlan
  - getSiteWlan
---

# Onboard a new Mist site

Mist is a strict MSP → Org → Site hierarchy (`data-model/juniper-data-model.yml`). Devices are **claimed into an org** and then **assigned to a site**; WLANs hang off the site.

## Steps

1. **Confirm scope** — `GET /api/v1/self` (`getSelf`) returns the orgs and privileges the token carries. Everything below needs write privilege on `org_id`.
2. **Avoid a duplicate site** — `listOrgSites` and match on `name`/`address`. There is no idempotency key; a retried create makes a second site.
3. **Create the site** — `POST /api/v1/orgs/{org_id}/sites` (`createOrgSite`) with `name`, `address`/`latlng`, `timezone`, `country_code`.
4. **Claim devices** — `POST /api/v1/orgs/{org_id}/claim` (`claimOrgLicense`) with the claim codes, or `addOrgInventory` for MAC/serial-based adds. Then `searchOrgInventory` to get each device's `id`/`mac`.
5. **Assign to the site** — `PUT /api/v1/orgs/{org_id}/inventory` (`updateOrgInventoryAssignment`) with `op: assign`, `site_id`, `macs`.
6. **Create the first WLAN** — `POST /api/v1/sites/{site_id}/wlans` (`createSiteWlan`) with `ssid`, `auth`, `enabled`. Read it back with `getSiteWlan`; `listSiteWlans` shows the site-level set (`listSiteWlansDerived` includes template-inherited WLANs).

## Reversal

- Unassign with `updateOrgInventoryAssignment` `op: unassign`.
- Site delete and WLAN delete are **irreversible** — no restore endpoint exists.

## Errors and limits

Flat `{ "detail": "…" }` envelope; 403 means the token authenticated but lacks org privilege — do not retry. 5,000 calls/hour/token.
