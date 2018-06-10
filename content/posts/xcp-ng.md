---
title: "XCP-ng"
date: 2018-04-23T22:00:00Z
draft: false
---

Earlier this year, the [XCP-ng](https://xcp-ng.org/) project completed a round of crowd-funding to produce a fully open version of XenServer without any paid feature lock-down.

This is a project I was pretty excited about; although I had never tried XenServer or any VMWare tools common to enterpriste IT, I was keen to have a more flexible "home lab" setup than the bunch of RPi's and regular servers I'd accumulated. [ProxMox](https://www.proxmox.com/en/) was my first attempt at running a virtualised setup at home, but I hadn't enjoyed the experience much - I wanted something which would require less tinkering, ideally with a more modern interface.

Unfortunately, XCP-ng didn't live up to the hype for my use-case. The main issue wasn't actually the virtualisation software itself, which seemed to work without issue. Instead, it was the dashboard and GUI tool [Xen Orchestra](https://xen-orchestra.com/#!/xo-home) that was the problem.

XO was developed by the team who kicked off the XCP-ng crowdfunding, and was originally developed for the original XenServer before being adapted to the open source alternative. While it might be a good tool (I don't have enough points of comparison to judge fairly), the feature lock-down for the free version was very distracting - I felt like I was using a small fraction of a real product. I was left with the impression that the whole crowd-funding thing was just a cunning wheeze to get more sales for their third-party management software, which seems rather hypocritical given that the main motivation for the split from XenServer was ostensibly to provide an alternative to an overly restrictive freemium model.

It only took a few hours for me to conclude that XCP-ng wasn't the right approach for me for the time being. At the moment, a simple CentOS + Vagrant/KVM server is likely to be my preferred solution for getting a few VMs running. Revisiting XCP-ng or ProxMox just to get a nice dashboard on a local server is seeming less and less likely as my public cloud usage increases - but I'll keep an eye on both projects to figure out if they are worth the hassle at a later stage.
