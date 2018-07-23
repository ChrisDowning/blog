---
title: "Building a Raspberry Pi Cluster"
date: 2018-07-23T22:00:00Z
draft: false
---

People have been building clusters using the Raspberry Pi family of devices for years now - the first example I came across was [“Iridis-pi”](http://www.southampton.ac.uk/~sjc/raspberrypi/), which serves as a demonstrator system for clustered computing and a classic example of how adults will find almost any excuse to play with Lego as part of their day job.

In the past, I have built a 4-node cluster using RPi2 devices - within that build, I used the official Raspbian OS and set up the SLURM job scheduler and GlusterFS distributed storage service - both from official repositories. Nothing too complicated, but a persistent mistake I made was to give into the temptation of making manual tweaks by SSH’ing around the cluster to make changes. While I did try to maintain the habit of using PDSH to make uniform changes wherever possible, this turned into a bit of a blunt instrument. A better approach would’ve been to use a configuration management tool like Ansible to make changes to the cluster.

Earlier this year, the Raspberry Pi Foundation released the new RPi 3B+, which improves on the CPU and networking capabilities of the RPi3. Given my hope to eventually use a RPi cluster as an [OpenHPC](http://openhpc.community/) testbed, the new hardware release seemed like a good opportunity to refresh my build, both to get higher-performance 64-bit CPUs and to give myself a slightly more useful platform from which to make the move towards more repeatable processes using configuration management and more general DevOps tools.

The equipment needed to replicate the setup described here and in future guides is as follows:

 - 2+ Raspberry Pi (5x RPi3 B+ in my case)
 - Gigabit ethernet switch
 - Ethernet cables (1 per RPi plus 1 to connect the switch to your LAN)
 - SD cards (1 per RPi)
 - Multi-USB power adapter (enough ports for 1 per Pi, or more if using HDDs which need additional power)
 - Micro-USB cables (1 per RPi)
 - Stackable cases or a custom rack (Lego obviously preferred…)

Some general advice on hardware choices:

 - To keep the setup compact, aim for an ethernet switch which can accommodate any future growth in your mini-cluster (or being reused elsewhere), but which can also be made to fit with whatever case(s) you go for.
 - SD cards do not need to be large, but bigger sizes allow for flexibility in what you do with the cluster once it has been built - 128 GB cards are the best value at the time of writing, but this is almost certainly overkill, so go for 16/32/64 GB depending on your budget.
 - USB storage media is optional, and can be added after-the-fact for projects rather than incorporated at the start. Going without it completely means incorporating your distributed storage solutions into the SD cards themselves, which is perfectly do-able but may feel less “neat”. My previous cluster iteration used USB devices for testing GlusterFS and so 8 GB sticks were fine, but connecting multiple spinning disks to each RPi is equally valid as long as the drives can be powered properly. This time though, I’m going to stick to just using spare capacity on the SD cards.

### Operating Systems
Before moving on to usage, we need to consider the OS installed across the cluster. As mentioned previously, I had hoped to use the cluster with OpenHPC packages, which would realistically have required a switch from Raspbian to CentOS (SLES is also an option for OpenHPC, but I have no particular interest in running a distribution which requires enterprise support in this context).

Unfortunately, the CentOS 7 64-bit ARM release does not support the RPi directly - ARM images of the RPi3 are still restricted to 32-bit (the same as Raspbian). While it is definitely possible to set up an OpenHPC cluster using RPi devices as evidenced by [this](https://opensource.com/article/18/1/how-build-hpc-system-raspberry-pi-and-openhpc) blog, the author has discussed [elsewhere](https://groups.io/g/OpenHPC-users/topic/centos_aarch64_on_raspberry/16303024?p=,,,20,0,0,0::recentpostdate%2Fsticky,,,20,0,0,16303024) that getting the RPi image working required manually blending the CentOS Aarch64 packages into a working 64-bit RPi Fedora image. While this does actually sound like fun, I don’t want to dedicate that much time to something which will inevitably be rather fragile, not to mention the potential for disruption to other projects I want to work on using the cluster.

In the end, I decided to stick with Raspbian as it is likely to offer access to the greatest quantity of  RPi-compatible software (and guides), allowing me to progress with more of the other things I’m interested in, like Ansible and Kubernetes.

### Getting Started
First, install the latest Raspbian OS - plenty of guides already exit detailing how to do this. This installation will need to be done independently for each RPi - setting up network boot (which will allow multiple RPi nodes to use a single SD card over the network) is described [here](https://www.raspberrypi.org/blog/piserver/), but I decided against this method as it still requires booting each node from a physical SD card in order to do parts of the configuration.

Each node should be configured with a standard root user and password, as well as a standard non-admin user account for testing (sudo on the user account is optional). It is generally best to do the initial setup just once and copy the image to multiple SD cards to save time, but be aware that some node-specific changes will need to be made on or before the first boot (such as setting the hostname and static IP address).

### Issues
Going into this, I was already aware of the limitations of the Raspberry Pi, and in particular the restrictions imposed by using a 32-bit operating system. While the reduced addressable RAM and other technical limitations of 32-bit systems are well known, the more nuanced (and frankly, far more important) aspect which isn’t discussed enough is the reduced software ecosystem a user can access when working with an armhf-based OS.

Lots of tools are available for the Raspberry Pi, often associated with a neat little guide or blog post - the issue here is that while lots of these tools do technically work, they tend to only work with a very narrow set of package version combinations, and can rapidly become out of date.

### Next Steps
Now I have a cluster running again, there are a series of tools I want to experiment with, each of which will receive its own write-up:

 - Using configuration management to speed up package deployment and admin tasks
 - Configuring Docker Swarm to manage services across the cluster
 - Installing SLURM to perform batch processing of long-running tasks
 - Installing the GlusterFS distributed file system
 - Installing Minio, a distributed object storage server
