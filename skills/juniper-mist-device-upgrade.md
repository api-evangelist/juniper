---
name: juniper-mist-device-upgrade
description: Upgrade firmware on Mist-managed devices at site or org scope, track the job, and cancel or restore if it goes wrong.
api: Mist API
provider: juniper
generated: '2026-09-18'
method: generated
source: openapi/juniper-mist-api-openapi.yml + conventions/juniper-conventions.yml
operations:
  - listSiteDevices
  - searchSiteDevices
  - getSiteDevice
  - upgradeSiteDevices
  - upgradeOrgDevices
  - getSiteDeviceUpgrade
  - listSiteDeviceUpgrades
  - cancelSiteDeviceUpgrade
  - cancelOrgDeviceUpgrade
  - restoreSiteDeviceBackupVersion
  - searchSiteDeviceEvents
---

# Upgrade Mist-managed device firmware — with a way back

This is a **device-affecting write**. Read `conventions/juniper-conventions.yml` first: no dry-run, no idempotency key, and reversal paths exist but **no window is documented** for any of them.

## Steps

1. **Enumerate targets** — `GET /api/v1/sites/{site_id}/devices` (`listSiteDevices`) or `searchSiteDevices` with `model`/`mac` filters. Confirm each device's current `version` via `getSiteDevice`.
2. **Check for an upgrade already in flight** — `listSiteDeviceUpgrades`. Because there is no idempotency key, a retried upgrade request queues a second job.
3. **Start the upgrade** — `POST /api/v1/sites/{site_id}/devices/upgrade` (`upgradeSiteDevices`) with the target `version` and the device list; `upgradeOrgDevices` for org-wide. The response carries an `upgrade_id`.
4. **Track** — `GET .../devices/upgrade/{upgrade_id}` (`getSiteDeviceUpgrade`) until the job reaches a terminal state. Poll gently: 5,000 calls/hour/token.
5. **Watch the device** — `searchSiteDeviceEvents` for restart/disconnect events, or better, have a `device-updowns` webhook already registered (`skills/juniper-mist-webhook-subscribe.md`).

## If it goes wrong

- **Cancel** — `POST .../devices/upgrade/{upgrade_id}/cancel` (`cancelSiteDeviceUpgrade` / `cancelOrgDeviceUpgrade`). The docs do not say how far into a job a cancel is honoured; treat it as best-effort and check the job state afterwards.
- **Restore configuration** — `POST /api/v1/sites/{site_id}/devices/{device_id}/restore_backup_version` (`restoreSiteDeviceBackupVersion`). Retention of backup versions is not stated. Do not promise a rollback you have not verified exists via the device's backup list.

## Do not

- Do not upgrade without a registered `device-updowns` webhook or an equivalent out-of-band check — the API gives you no push signal otherwise.
- Do not retry a `POST .../upgrade` on a network timeout without first calling `listSiteDeviceUpgrades`.
