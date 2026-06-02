---
title: "ilorest: troubles creating a virtual drive on DL325"
url: "https://community.hpe.com/t5/server-management-remote-server/ilorest-troubles-creating-a-virtual-drive-on-dl325/m-p/7267879#M10490"
date: "2026-06-01"
author: "blauerrauch"
feed_url: "https://community.hpe.com:443/hpeb/rss/Community?interaction.style=forum"
---
I just can't create a virtual volume on my HPE servers with ilorest. Find the Box,Bay,Slot 3-tuples: for n in 0 1 2 3 4 5 6; do ./ilorest.sh rawget /redfish/v1/Chassis/DE041001/Drives/${n} | grep ServiceLabel; done for n in 0 1 2 3 4 5 6; do ./ilorest.sh rawget /redfish/v1/Chassis/DE041000/Drives/${n} | grep ServiceLabel; done ilorest --nologo --verbose createvolume volume RAID6 1:1:22,1:2:22,1:3:22,1:6:22,1:7:22,1:8:22,2:2:22,2:6:22 --controller=0 --storageid=DE041000 --DisplayName YUGATHREE --ReadCachePolicy Off --WriteCachePolicy ProtectedWriteBack --capacitybytes 5000000000000 iLORest retu
