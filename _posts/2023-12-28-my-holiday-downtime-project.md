---
title: "My Holiday Downtime Project"
excerpt_separator: "<!--more-->"
categories:
  - blog
tags: 
  - web
---

My employer shuts the entire company down over the holiday break, so I find myself with idle hands. Naturally, it's time to break something.

**Caution:** There is no TL;DR on this one.

<!--more-->

Our ISP has been running fiber in our neighborhood for about 6 months and they're close to having things wired up. I have a pedestal in the alley behind the house and as an existing DSL (yes, you read that right) customer, I'm one of the first to get the upgrade. In anticipation, I'm doing some upgrades to my home network.

### Back in Time

OK, first about the DSL. I had cable internet for about 18 years from the local cable TV company. In 2001, when we bought this house, the 20MB down/2 MB up connection was pretty amazing. There were always issues, but we learned to deal with them. Over time, that speed went up to 50/5 and then 100/10. Along that same period, the company changed from "Weatherford Cable" to "ClassicNet". Then from "ClassicNet" to "CEBridge". A few years later, it changed from "CEBridge" to "Suddenlink" and that's when things really started to go off the rails. 

When Suddenlink took over, I had monthly downtimes lasting hours. I could count on an hour-long outage on Monday at 11AM as the local employees brought down the network for maintenance. At least once a quarter, I had an outage of more than 1 day. A couple times, during ice and snow storms, the entire community were without internet for 4 or 5 days. This was beginning to have an impact on my ability to work. During those outages, I would tether my work laptop to my phone, but that's really not a reliable or scalable solution.

In 2019, I started looking at my options. There are several *fixed-wireless* ISPs that operate in the area. I know the owner of one such provider and I asked him to see if I could switch to his service. Unfortuately, there was no place on my property where we could locate my antenna so it could see his antenna. There are quite a few trees in our neighborhood and these were blocking line-of-sight to his tower.

The local telephone company also provides internet services. I asked about their packages and they offered both fixed wireless and DSL. The DSL came over the twisted-pair copper lines and could provide up to 25MB down/5MB up. I know that seems insanely slow these days, but I talked with their network engineers and they said their DSL hadn't gone down in over 3 years. I decided to roll the dice.

I kept my cable internet account for a few months while I set up the DSL and experimented with it. True to their word, I had no interruptions in service. In January 2020, I cancelled my cable internet.

Think about that. January 2020. What was happening about that time? People were saying stuff like *Have you heard about that virus going on in China?* Well, the cable company did 2 things at that point:

1. They sold themselves, once again, and became Optimum

1. Optimum shut down the local office, so trouble calls were now routed through a central office in Arkansas. I learned from friends and family that still used them that sometimes it took 3 weeks to get a problem fixed. Meanwhile, all those new telecommuters didn't have internet.

Talk about dodging a bullet.

### Back to Now

My ISP has a combo modem/router box. They do all the management on the box and they do not allow customers to modify settings. They made one small concession to me, which was to bridge one of the ethernet ports on the modem so that I could attach my own network to theirs. I bought a set of Google Home mesh routers and placed them around the house. I don't like the Google router, to be honest, but it sucked less than the Netgear Orbi mesh that I was replacing.

The problem with the Google router is that it can only be updated with a mobile app. That app hides some of the crucial settings in strange places and sometimes it would just refuse to connect. And, in typical Google fashion, there was no customer support.

What I wanted to get was a modern router similar to the old workhorse [Cisco/Linksys WRT54G](https://www.amazon.com/Linksys-WRT54G-Wireless-G-Router/dp/B00007KDVI) that I had years ago with a custom [Tomato](https://tomato.groov.pl/?page_id=164 or [DD-WRT](https://dd-wrt.com/) firmware. I could tweak a lot of settings to my liking and it always worked.

* House network for personal devices with adblocking across entire network.

* Separate network segment for IOT devices that can't see house network. I'm not comfortable with the amount of telemetry some of these shitboxes send home and I'd like to throttle it without killing my home network. Would run on 2.4ghz because a lot of IOT stuff doesn't work on 5ghz.

*  Have a lab segment for work.

* A guest network that can be accessed up to 24 hours without me having to hand out a password, but isn't so convenient that neighbors just leech off my network.

As it happens, my director at my new employer is formerly from Cisco. He filled my head with ideas on how to achieve my goals. Naturally, it involved spending a bit of money. Toward this goal, I've purchased:

* (1) 4-port [mini-PC](https://cwwk.net/collections/frontpage/products/12th-gen-intel-firewall-mini-pc-alder-lake-i3-n305-8-core-n200-n100-fanless-soft-router-proxmox-ddr5-4800mhz-4xi226-v-2-5g) with 1TB storage and 32GB RAM. You can get these on Amazon or Ali-Express (if you're willing to wait for international shipping). I bought the barebones model and installed Corsair memory and NVME modules.

* (1) [TP Link 8-port](https://www.tp-link.com/us/home-networking/8-port-switch/tl-sg108pe/) managed gigabit switch that supports VLAN and Power-over-Ethernet (POE).

* (2) [TP Link EAP 610s](https://www.tp-link.com/us/business-networking/omada-sdn-access-point/eap610/) with POE for indoor use

* (1) [TP Link EAP 610](https://www.tp-link.com/us/business-networking/omada-sdn-access-point/eap610-outdoor/) (Outdoor sealed) with POE 

### Let's Get it Started

I didn't want to disrupt internet access over Christmas while our kids were home, so I started trying to work on this away from our regular internet infrastructure. My wife is was on holiday last week and this week and will be back to work next week while I'm still not back until January 8th. I wanted to make sure I had everything working in the garage before tearing out the existing connections. I told her that this is like trying to grow a tree from the limbs out to the leaves without having a trunk.

So, imagine me in the cold garage, sitting on a folding chair with a 3 foot (1 meter) piece of lumber for a worktable. I had a portable heater at my feet to hold off the chill as I fastened things together and configured software.

The day after Christmas, with the kids back at their own homes, I installed [Proxmox VE](https://www.proxmox.com/en/) on the mini-PC. Proxmox is a virtualization environment that will let me run a bunch of small services that normally would go on a Raspberry Pi. In the first VM, I installed [OPNSense](https://opnsense.org/), which is an open-source firewall.

I had the wireless APs attached to the switch, which was in turn attached to the mini-PC in port 2. Port 1 (aka ETH0) will eventually go into the ISP box, where it will get a dynamic IP address via their DHCP server.

I was having a lot of problems understanding how to configure things like virtual adapters in Proxmox, as well as bridging them in a way that traffic could move about. I watched hours of YouTube videos from alleged experts in this field. I'm not sure if they're experts in virtual environments or just really good at getting their videos to the top of searches for virtualization. Regardless, I did a lot of experimenting using a couple of older PCs that worked well in the confines of the garage. I thought I had everything working right and was ready for the moment to plug it into the ISP box.

### The Point Where Everything Fell Apart

I asked my wife if she minded me turning off the internet for an hour or two and she said she didn't mind, as she was reading [a book](https://www.amazon.com/Salt-World-History-Mark-Kurlansky/dp/0142001619) I bought her for Christmas. So, off I went to disconnect the existing network and plug in my new network.

I'll save you the suspense. Nothing worked. As in *didn't work and didn't work badly*.

I tweaked settings and nothing would send traffic out of my local network. After 2 hours of this, I was frustrated and I disconnected the new network. I made so many changes trying to get it to work that I couldn't even connect to the Proxmox environment at that point. I decided the best thing to do was to wipe everything and start over, taking my learnings the last 2 days to maybe do things right. So, I factory reset the mini-PC and then set it down for the day.

When I plugged the old network back to the ISP box, nothing worked there, either. I powered everything down and restarted it all back in order, so that the ISP box would renew its lease to the ISP DHCP server. That didn't work, either. I factory reset the Google router and the mesh nodes as well. Nothing would work.

**&lt;img src="this-is-fine.jpg" alt="image of man screaming at a tree"&gt;**

At this point, it was 5:30 PM and I was tired and angry. I was definitely ready for an adult beverage. I knew my ISP office was closed, but I didn't know what else to do, so I called their technical support line. They have a 24-hour rollover to a remote management company, so I knew I could at least maybe get them to reset things from their side.

That's when I heard *the message*.

*We apologize for the inconvenience, but our network connection is not working. Please be patient while we fix the problem.*

That's right. For the first time in 4 years, my ISP had an outage. And it just so happened on the very afternoon that I was making big changes to my network.

As the kids these days say, **FML**. The ISP network came back on about 9 PM and I finished reseting my Google router. Which, it turns out is another aggravating thing about the Google setup. If you don't have an upstream connection, you can't configure their router with their app. I suppose it's made for dummies, not anyone wanting to do anything remotely technical.

### Let's Try This Again

On Wednesday, I reinstalled all the software on the mini-PC and made sure the APs could communicate. I left everything alone until Thursday, when my wife was leaving for the day to go see her best friend.

On Thursday, I made sure everything still worked correctly when not on the main network. With that done, I repeated the process from 2 days prior. I removed the Google router from the ISP box and unplugged it from power. I plugged in the mini-PC and tested the connection. I had a few minor problems that I needed to update to get the firewall and router to work correctly. It's amazing how things work when they actually work.

At this point, I disassembled everything and started running ethernet cabling to 2 of the places where the indoor APs will go. This involved digging in the attic, which has fiberglass insulation. Yay. Once I had the cabling ran to those points and the mounting brackets installed, I attached the APs to the ethernet and... Voila! I had 5GH Wifi-6 served throughout the house. 

With everything working, I backed up the server and stored the backup image on my local PC. Then, I installed the AdGuardHome ad-blocker on the OPNSense firewall. In doing so, I didn't pay attention to the installation options close enough. I ended up overwriting the port ID of the main administration tool with the AdGuardHome tool. Fortunately, this was easily fixed with a couple of edits. I spent about 30 minutes digging through directories with the command line to find the configuration setting that controls which port does what.

So now, I have ad-blocking on the entire house network. Woot.

![](/assets/images/2023-12-28/woot-300x200.png)

### Loose Ends

There's still work to do, but I'm going to take a few days off until I start mucking about with things. 

* At this point, I have 2 SSIDs serving the house. One is for IOT traffic (Roomba, TVs, thermostats, etc.) and the other is for our personal devices. These are not on a VLAN, so both networks are getting the full firewall treatment. The IOT network is on 2.4GHz and the house network is on 5GHz. I will need to set up a VLAN on the firewall, switch, and APs to get these separated.

* The outdoor AP is not installed where it needs to go. I'll wait until we're not getting precipitation and 30 MPH (50 kph) winds before I install the outdoor AP on top of the 30' (10m) antenna mast. For now, it sits in the garage, serving up internet to our vehicles.

* I have a lot to learn about routing in the days ahead. I have the simplest routing setup now, but I know I want to do more complicated things. But, routing is one of those things that if you screw it up, you have to start over from scratch. Best to learn more concepts before experimenting on a live network.

* I want to install a second VM with some Linux distros that I want to try out. I want to play with NextCloud and a few other tools. 

* Eventually, I want to replace my dedicated Arlo wifi camera system with one that uses POE ethernet cameras. The Arlo is OK, but it's very limited.

Finally, the point of all this work is in preparation for fiber internet. I see the work trucks at the nearby phone offices and their trucks are still pulling fiber bundles along the streets. I'm hoping to get set up in the next few months.