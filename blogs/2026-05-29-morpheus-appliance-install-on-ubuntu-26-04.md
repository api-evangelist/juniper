---
title: "Morpheus Appliance Install on Ubuntu 26.04"
url: "https://community.hpe.com/t5/hpe-morpheus-enterprise-software/morpheus-appliance-install-on-ubuntu-26-04/m-p/7267827#M5266"
date: "2026-05-29"
author: "CraigMole"
feed_url: "https://community.hpe.com:443/hpeb/rss/Community?interaction.style=forum"
---
Morpheus install is not supported for 26.04 yet I don't think. When you go to install it, chef fails with a failure to pull freerdp2-dev, this has since been replaced with freerdp3. I was successful in faking out the appliance by creating a dummy package which simply points to freerdp3 instead.
