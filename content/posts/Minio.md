---
title: "Minio"
date: 2018-04-22T21:00:00Z
draft: false
---

[Minio](https://www.minio.io) is a simple, S3 compatible object storage server which can be deployed in a Docker container or as a simple binary and provides built-in deduplication over objects.

The application design is very much aligned with the microservices model - configuration is fairly minimal, and in the simplest case Minio can be launched using just a filesystem path in which the objects are to be stored. Each instance of Minio effectively operates in "single-user" mode, with one access key/secret key pair - in order to offer Minio to multiple users from the same server or cluster, the administrator need only deploy more Minio instances targeting a different directory per user (or "tenant"). The underlying directories can then be managed from the server just as they would if they contained regular files - with simpler setups, that is exactly what they are.
