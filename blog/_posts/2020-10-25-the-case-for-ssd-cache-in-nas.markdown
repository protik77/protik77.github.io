---
layout: post
author: Protik Das
title:  "The case for SSD cache in a NAS"
date:   2020-10-25
---

I recently upgraded my home NAS to DS 920+ from a DS 718+. The newer versions comes with 2 M.2 NVMe slots. They are meant to be used as an SSD cache. This year Synology have been including more of these slots to be used for SSD cache. I have nothing but good things to say about Synology devices. So I have been thinking about this from a system design perspective. 

The use of an NVMe drive as cache have been a contentions issue on the [/r/synology](https://www.reddit.com/r/synology/) sub-reddit. I have been reading about the cache more to understand the design. I have come to learn about the over-provisioning issue in the NVMe drives. That limits the use of an NVMe drive as a cache device. Even before that, another question is, why not use the SSD as a boot drive rather than a cache? Let's go through them step by step.

First the over-provisioning of an NVMe drive. In a simpler term, 

The second question is why not use the NVMe drive as a boot drive? Thinking about it, the purpose of an SSD type flash device is to reduce start up time for the device. For a NAS, the device usually sits in a corner at the home, serves files. The device is rarely restarted. Longest period of time I haven't restarted my NAS is for 7 months. So an SSD as a boot drive doesnt help much. 

So where will it help? 
