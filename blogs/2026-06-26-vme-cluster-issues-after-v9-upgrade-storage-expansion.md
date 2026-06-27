---
title: "VME Cluster Issues after v9 upgrade / storage expansion"
url: "https://community.hpe.com/t5/hpe-morpheus-vm-essentials/vme-cluster-issues-after-v9-upgrade-storage-expansion/m-p/7269197#M2397"
date: "2026-06-26"
author: "odavies"
feed_url: "https://community.hpe.com/hpeb/rss/Community?interaction.style=forum"
---
Hi all, I'm hoping someone can point me in the right direction as we've run into some issues following an upgrade from HPE VM Essentials v8 to v9. Environment 3 x HPE ProLiant hosts running HPE VM Essentials Hypervisor (HVM) Single VME Manager instance HPE Alletra 6000 storage 2 x shared GFS2 datastores Prior to upgrading, I expanded both GFS2 datastores from 500 GB to 750 GB on the Alletra storage and performed the grow operation to make the additional capacity available within the cluster. Upgrade Process I then: Upgraded the VME Manager from v8 to v9 using the provided .deb package.
