---
layout: post
author: Protik Das
title:  "The case for SSD cache in a NAS"
date:   2021-07-24
---

> I started writing this post on 25 September 2020. When I finished the post on July 2021, DSM 7 came out. Synology forked flashcache which is a deprecated cache library from Facebook. Apparently Synology improved the caching mechanism quite a bit. So other than the basics, the timing data may be a bit outdated.

I recently upgraded my home NAS to DS 920+ from a DS 718+. The newer versions comes with 2 M.2 NVMe slots. They are meant to be used as an SSD cache. This year Synology have been including more of these slots to be used for SSD cache. I have nothing but good things to say about Synology devices. So I have been thinking about this from a system design perspective. 

The use of an NVMe drive as cache have been a contentions issue on the [/r/synology](https://www.reddit.com/r/synology/) sub-reddit. I have been reading about the cache more to understand the design. I have come to learn about the over-provisioning issue in the NVMe drives. That limits the use of an NVMe drive as a cache device. Even before that, another question is, why not use the SSD as a boot drive rather than a cache? Let's go through them step by step.

The second question is why not use the NVMe drive as a boot drive? Thinking about it, the purpose of an SSD type flash device is to reduce start up time for the device. For a NAS, the device usually sits in a corner at the home, serves files. The device is rarely restarted. Longest period of time I haven't restarted my NAS is for 7 months. So an SSD as a boot drive doesnt help much. 

So where will it help? Apparently Synology thinks it will help if the SSD is used as a read only or a read-write cache. As I write mostly large files to teh NAS, read-write cache doesn't make much sense. So I went with a read only cache. Synology suggests their name brand SSD for this, I went for a consumer grade SSD from Western Digital. In DSM 6, there's an option to avoid caching of large block files. So the caching only happens for random small files.

First the elephant in the roam. It have been pointed out with scary detail why this is a bad idea. Diggin deeper, the concern comes from over-provisioning of an NVMe drive. In simpler terms, when the SSD needs to write one page and the SSD is almost full, it has to delete a block, which is larger than the page size. The SSD have a mechanism to copy the excess data to a cache. Then it writes back the data to SSD. Meanwhile the OS is waiting for confirmation from the SSD. Often times the write times out and crashes a volume.

All of this is a valid concern. But this only happens if over-provisioning needs to happen. If the data needed for read cache is way smaller than the SSD, the read cache works fine. After I started using the cache, largest I have seen the cache size is 40% of the cache.

On the positive note, the cache improves random reads quite a bit. I ran some tests. For a cold load a web app takes about 2 seconds. But after the small files are on a cache, the same app takes about 0.02s. So when the cache need is smaller than the cache SSD size, the use of SSD cache is not that bad.
