---
title: An open source, secure, and easy to set up personal VPN
layout: post
categories: [talescale,wireguard, networking, security, firewall]
tags: [vpn,security,mfa,firewalls,encryption,MITM,tailscale,wireguard]
---

# An open source, secure, and easy to set up personal VPN

## How to Security

I honestly can't stand how VPNs are shilled almost everywhere on the internet as a way to completely secure yourself on the internet. The worst part is that they don't talk about any of the details about how they specifically keep you safe and how they may not. The ads are less about how the VPN prevents a hacker from exploiting your computer/connection and more about how this subscription is going to solve all the security problems you ever had.

Unfortunately, I hate to break it to you... You're just not going to fix your security posture with a $5.99 subscription to some random VPN provider...

There are several things to consider when thinking about security, such as:

- Password Security
- Multifactor Authentication
- Both Network and Endpoint firewalls
- Phishing Protection - phone/email/sms/URL
- Data encryption locally and in the cloud
- Sites that leak personal information
- Man in the Middle attacks

## What even is a VPN?

- By nature of a VPN being an encrypted tunnel, a man-in-the-middle attack becomes very unlikely. 
- If you wanted to get to something on your home network like a NAS or Media Server, it's much safer to use a VPN than to forward a port. Forwarding a port opens a hole in your home network's firewall which can create an attack vector. 

So if you decided you did need a VPN, which one do you choose? OpenVPN? Wireguard? Tailscale? IPSec? PPTP? (actually, forget I said that, PLEASE don't use PPTP in 2022...)

Some people prefer their money pits to look like boats, mine happens to be a homelab... So I obviously prefer free solutions. And I don't want to spend a bunch of time setting it up. And I want it to be really secure for years to come. And I want it to be so easy to use and reliable, I'd be dumb not to use it.

## Tailscale

You can cross off almost every option except Tailscale. I know, because I've used all the options above. Unlike traditional VPNs that require you to install and configure specialized software, Tailscale can be set up in just a few clicks. All you need is a Tailscale account and the Tailscale app, which is available for a variety of platforms including Windows, Mac, Linux, Android, and iOS.

You can see all of your connected devices in the Tailscale app, and you can even give your devices friendly names to make it easier to identify them. In addition to its ease of use, Tailscale also offers top-of-the-line security. Tailscale uses industry-standard encryption to protect your data, and it also includes features like end-to-end encryption and two-factor authentication to ensure that only authorized users have access to your network.

**Next up: Let's set this up in the lab!**
