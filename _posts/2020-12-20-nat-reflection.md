---
title: I just forwarded a port and it works from the internet, but not my network!
layout: post
categories: [networking, nat, firewall]
tags: [nat reflection,hairpinning,nat,pat,pfsense,proxy]
---

# NAT Reflection

![Wall of Fire](/assets/images/wall-of-fire-19885-1.jpg)

If this just happened to you and you're confused about why, read on as we cover NAT Reflection aka NAT Hairpinning or NAT Loopback.

## Port Forwarding

Lots of us have had to forward a port through our home router, but did you know that you're actually modifying a type of NAT (Network Address Translation) on your router called a PAT(Port Address Translation)? With only 32 million IP Addresses available, IP's on the internet are scarce and expensive. The point of a NAT is to allow all the devices on a home or business network to share a single IP address therefore reducing costs and conserving IP addresses. It can also sometimes be commonly referred to as a "firewall" when it is used to prevent outside connections from making it inside a local network.

Great! I have a NAT and I forwarded my port through it but now I can only get to it from outside my home network!

## The Gotcha

A NAT works great when connecting from the outside in, but not so much when you have to first go out, then back in the same network. In this case, the device on the private network won't be able to initiate the connection because the router will drop the packet when it sees it's destined for a forwarded port. This is called the "NAT traversal" problem.

So, how do we solve this problem? Enter NAT reflection (also known as NAT loopback or NAT hairpinning). NAT reflection is a technique that allows a device on a private network to initiate a connection to another device on the same private network using the public IP address and port of the NAT device. This means that devices on a private network can initiate connections to other devices on the same private network using their public IP address and port, even if the other devices are behind a NAT.

## Turn on NAT Reflection

To enable NAT reflection, the NAT device needs to be configured to forward incoming connections to the correct internal device based on the destination IP address and port. This is usually done using NAT rules or port forwarding rules, which specify the internal IP address and port of the destination device.

NAT reflection can be useful in all sorts of situations. For example, if you wanted to host a gaming, media or home automation server. In any of these three scenarios, you may need to connect from the external internet or from your local network and port forwarding alone is not going to be the total solution. You must also enable NAT reflection.

Now lets take a look at how you would do that in pfsense:

- Navigate to **System > Advanced** on the **Firewall & NAT**
- Locate the **Network Address Translation** section of the page
- Configure the NAT Reflection options as follows:

![Alt text](/assets/images/screenshot-from-2022-12-20-13-16-05.png)

There are three available choices for NAT Reflection mode for port forwards, they are:

**Disable** - NAT Reflection will not be performed, but it may be enabled on a per-rule basis.

**NAT + Proxy** - Enables NAT Reflection using a helper program to send packets to the target of the port forward. This is useful in setups where the interface and/or gateway IP address used for communication with the target cannot be accurately determined at the time the rules are loaded. Reflection rules for use with the proxy are not created for ranges larger than 500 ports and will not be used for more than 1000 ports total between all port forwards. This mode does not work with UDP, only with TCP. Because this is a proxy, the source address of the traffic, as seen by the server, is the firewall IP address closest to the server.

**Pure NAT** - Enables NAT Reflection using only NAT rules in pf to direct packets to the target of the port forward. It has better scalability, but it must be possible to accurately determine the interface and gateway IP address used for communication with the target at the time the rules are loaded. There are no inherent limits to the number of ports other than the limits of the protocols. All protocols available for port forwards are supported. If servers are on the same subnet as clients, the Enable automatic outbound NAT for Reflection option will mask the source of the traffic so it flows properly back through the firewall.

## Wrapping up

Once you identify the correct version of NAT Reflection to enable, you can apply the changes and voila! You will now be able to hit your external IP from within your local network. Of course these steps are different depending on what your home router or gateway manufacturer is, so some googling may be necessary if you don't have a pfsense router.

In summary, NAT reflection is a technique that lets devices on a private network initiate connections to other devices on the same private network using their public IP address and port, even if the other devices are behind a NAT. This can be really handy in lots of different situations, like hosting a server or running a peer-to-peer application.
