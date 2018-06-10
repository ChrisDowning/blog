---
title: "Minio"
date: 2018-04-22T22:00:00Z
draft: false
---

[Minio](https://www.minio.io) is a simple, S3 compatible object storage server which can be deployed in a Docker container or as a simple binary and provides built-in deduplication over objects.

The application design is very much aligned with the in-vogue microservices model - configuration is fairly minimal, and in the simplest case Minio can be launched using just a filesystem path in which the objects are to be stored. Each instance of Minio effectively operates in "single-user" mode, with one access key/secret key pair - in order to offer Minio to multiple users from the same server or cluster, the administrator need only deploy more Minio instances targeting a different directory per user (or "tenant"). The underlying directories can then be managed from the server just as they would if they contained regular files - with simpler setups, that is exactly what they are.

In our cloud-first world, it has been hard for me to see a good use-case for Minio so far - people sticking with on-premises infrastructure tend not to be in a position to re-architect for a new storage model, and if they are they would usually be doing so as part of a shift to the cloud.

The Minio team have done a good job of providing alternative usage models to a pure object storage server - the most interesting (to me, at least) is the local disk cache for the gateway mode. This performs exactly as it sounds, allowing cloud-based object storage which may or may not have an S3-compatible API to be accessed via a separately hosted (S3) API server. The gateway server can, in turn, be configured to use a local directory or disk as a cache for objects as they are pushed or retrieved.

Where this starts to sound even more interesting for me is if we can create a large, fast cache based on some kind of distributed storage like GlusterFS (or even Lustre?) - lo and behold, users have an unlimited amount of object storage with all of the benefits of the cloud, but with their most commonly used files retained locally for quick and free access. Solutions looking like this are readily available, but tend to be tied to a particular cloud ecosystem and may have significant cost.

Minio has been around for a couple of years now, and is well established in its particular niche. It will be interesting to see how the core developers end up pushing the project further, and also how much penetration the tool can gain into the research community thanks to its ease-of-use.
