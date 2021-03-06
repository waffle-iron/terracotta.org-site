---
---
# Blocking Cache and Self-Populating Cache {#blocking-cache-and-self-populating-cache}


{toc|2:3}

## Introduction
The `net.sf.ehcache.constructs` package contains some applied caching classes which use the core classes to solve everyday caching problems. Two of these are BlockingCache and SelfPopulatingCache.

## Blocking Cache {#blocking-cache}
Imagine you have a very busy web site with thousands of concurrent users. Rather than being evenly distributed in
what they do, they tend to gravitate to popular pages. These pages are not static, they have dynamic data which
goes stale in a few minutes. Or imagine you have collections of data which go stale in a few minutes. In each
case the data is extremely expensive to calculate.
Let's say each request thread asks for the same thing. That is a lot of work. Now, add a cache. Get each thread
to check the cache; if the data is not there, go and get it and put it in the cache.

Now, imagine that there are so
many users contending for the same data that in the time it takes the first user to request the data and put it in
the cache, 10 other users have done the same thing. The upstream system, whether a JSP or velocity page, or interactions
with a service layer or database are doing ten times more work than they need to.
Enter the BlockingCache.
It is blocking because all threads requesting the same key wait for the first thread to complete. Once the first
thread has completed the other threads simply obtain the cache entry and return.
The BlockingCache can scale up to very busy systems. Each thread can either wait indefinitely, or you can specify a timeout
using the `timeoutMillis` constructor argument.

For more information about Blocking Cache, refer to this [Javadoc](http://www.ehcache.org/apidocs/2.8.5/index.html).

## SelfPopulatingCache {#selfpopulatingcache}
You want to use the BlockingCache, but the requirement to always release the lock creates gnarly code. You also
want to think about what you are doing without thinking about the caching.
Enter the SelfPopulatingCache. The name SelfPopulatingCache is synonymous with Pull-through cache, which is a
common caching term. SelfPopulatingCache though always is in addition to a BlockingCache.
SelfPopulatingCache uses a `CacheEntryFactory`, that given a key, knows how to populate the entry.
Note: JCache inspired getWithLoader and getAllWithLoader directly in `Ehcache` which work with a `CacheLoader` may be used as an alternative
to SelfPopulatingCache.

For more information about Self-populating Cache, refer to this [Javadoc](http://www.ehcache.org/apidocs/2.8.5/index.html).
