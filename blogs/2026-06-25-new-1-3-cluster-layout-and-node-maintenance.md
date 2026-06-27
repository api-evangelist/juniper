---
title: "New 1.3 cluster layout and node maintenance"
url: "https://community.hpe.com/t5/hpe-morpheus-vm-essentials/new-1-3-cluster-layout-and-node-maintenance/m-p/7269226#M2398"
date: "2026-06-25"
author: "DA9"
feed_url: "https://community.hpe.com/hpeb/rss/Community?interaction.style=forum"
---
With the old 1.2 cluster layout and VME 8.X, when you put a compute node into maintenance mode from the GUI the node was also put into PCS cluster maintenance. This automatically stopped any locking datastore concerns with any GFS2 disks. The node would reboot cleanly and quickly.
