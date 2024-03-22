---
title: 'We Have k8s At Home: how I run my personal cluster'
date: "2024-02-02T14:40:12-05:00"
categories:
    - "tech"
    - "k8s"
draft: false
---

# The Problem

For years I was running a Raspberry Pi Model B+ behind my desk as a "let me just dump scripts into my `rc.local` file" sort of server on my private network. It worked nicely enough but I wanted something powerful, efficient, and cool.

I wanted to get a working and persistent Kubernetes lab running on my personal network to practice with software I use at my day job, as well as use those tools to solve problems on my personal network. This leads nicely into...

# The Server

I initially was looking at the newest Raspberry Pi boards, but (1) the pricing and (2) availability immediately had me searching for alternative solutions. I poked around the old Internet + asked around and work and discovered Mini PCs: the newest craze in personal cloud-native lab technology.

After a bit of googling, this guide won me over: https://blog.kroy.io/2019/12/08/the-wyse-5070-a-perfect-little-vyos-device/

The Wyse 5070 is a nifty little box that appears to draw <15W in an absolute worst case scenario, which makes it a wonderful choice for my jacket closet along with my router and (eventually) my NAS. It has a x86 quad core CPU and room for expansion - a great platform for running a lightweight Kubernetes cluster off of. It's hard to argue against $50 for this level of power as well. 

# The Accoutrement 

[Rancher k3s](https://github.com/k3s-io/k3s) is what I'm running on this server. All I want is a lightweight, self-contained k8s setup that works closely enough to what I use at work. In that regard - it's fantastic, plus it lets me put on my full stack developer hat when I'm deploying applications to it. I didn't want to get bogged down in trying to set up some massive enterprise-grade deployment; I wanted to shorten my "setup to deploying useful apps" timeline as much as possible so I could focus on learning. 

![My beautiful server room](/server-room.jpg)
*Primary Computer Core*
# Getting things done

The main script I was running on my old Pi was [sponsorblockcast](https://github.com/nichobi/sponsorblockcast), which got deprecated at some point in the last five years since I set it up to skip in-video ads on my Chromecast (think "This video was sponsored by Squarespace!"). It was supplanted by an improved version called castsponsorskip https://github.com/gabe565/CastSponsorSkip, which I decided to switch to.

I created a [k8s manifest file](https://github.com/kylesliva/k8s/blob/main/castsponsorskip/castsponsorskip.yaml) to deploy castsponsorskip, and the rest is history. Now I'm using k8s on my personal network to run tools that improve my life - and let me work with heavyweight cloud tools. It's pretty fantastic! 

# Next Steps

I'd like to set up monitoring for this k8s node and its Pods, perhaps running on my old Pi so it's running on discrete hardware. I'd also like to work with service mesh solutions - maybe linkerd2 or istio. 

It's incredibly rewarding using the tools of my career to solve problems in my daily life! This is a symbiotic relationship that makes me look at my skills from another perspective, and shore up my personal technology posture.