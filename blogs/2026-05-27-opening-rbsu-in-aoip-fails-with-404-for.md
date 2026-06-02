---
title: "Opening RBSU in AOIP fails with 404 for BiosAttributeRegistryU32.v1_2_90"
url: "https://community.hpe.com/t5/hpe-proliant-servers-ml-dl-sl/opening-rbsu-in-aoip-fails-with-404-for-biosattributeregistryu32/m-p/7267809#M194941"
date: "2026-05-27"
author: "hpe-lover"
feed_url: "https://community.hpe.com:443/hpeb/rss/Community?interaction.style=forum"
---
My HP DL360 Gen 10 is on with ROM U32 3.64 iLO 3.19 SPSGen10_04.01.05.201 IP 3.92.5 So, it's all on the latest / newest. And when trying to manage RBSU / BIOS from AOIP, i get the following error: StatusCodeError: 404 - {\"error\":{\"code\":\"iLO.0.10.ExtendedInfo\",\"message\":\"See @Message.ExtendedInfo for more information.\",\"@Message.ExtendedInfo\":[{\"MessageArgs\":[\"/rest/v1/Registries/BiosAttributeRegistryU32.v1_2_90\"],\"MessageId\":\"Base.1.18.ResourceMissingAtURI\"}]}} Note the version mismatch, my bios is on 3.64. It used to be on 2.90 so there seems to be some stale data somewhe
