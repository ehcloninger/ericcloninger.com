---
title: "Self-Hosting Vaultwarden on Proxmox with Tailscale and HTTPS"
excerpt_separator: "<!--more-->"
toc: true
categories:
 - blogs
tags: 
 - howto
 - proxmox
---

Bringing my passwords to my local server, with secure mobile app support.

<!--more-->

**NOTE**: This is yet another long post about a technical project. I haven't found
a write-up on this topic in a single location, so I'm going to document what I
discovered and hopefully it will help the next person.

If you want to skip the boring background bits, 
[click here](/blogs/vaultwarden-on-proxmox-with-tailscale-https/#get-on-with-it-already).

---

## The Boring Background Bits

I've been using [Bitwarden](https://bitwarden.com) for about 7 years. I am happy to pay  
for their work and for using their infrastructure to keep my passwords secure. This year, 
there are two circumstances that had me looking into self-hosting a password server:

1. Bitwarden increased the annual cost of hosting by 20%
2. I'm currently unemployed and have time on my hands

The cost of the hosting wasn't bad. It increased from $40 to $48 per year for a family plan that 
my wife and I share. They hadn't increased the price in those previous 7 years, so it's not that 
they're being jerks to do it this year. They've earned a raise. But, $48 will buy a couple cases
of beer, so I decided to see if I could make this happen. (**Hint:** I did).

### Proxmox

My [Proxmox homelab](/blog/my-holiday-downtime-project/) server has been running great for more 
than 2 years. In the time since I started working on Proxmox, I've installed and tried out a bunch
of different container projects. Some I continue to use and others I've deleted after deciding they
didn't suit my needs. A great help in this has been the 
[Proxmox Community Scripts](https://community-scripts.github.io/ProxmoxVE/) project, which started
out as one person's attempt to make a stable and repeatable installation process and has since
grown exponentially. (RIP, TTeck)

Along the way, I have learned how to use a reverse proxy to make some of these services available on 
the open Internet. I have a custom domain and I use subdomains to route traffic to the different
services running on Proxmox. 

The reverse proxy I use is called [Nginx Proxy Manager](https://nginxproxymanager.com/). There are several 
other choices, such as [Caddy](https://caddyserver.com/) and [Traefik](https://traefik.io/traefik). 
I tried both of those, but found the Nginx proxy worked great and had the least amount of configuration hassles.

Nginx Proxy Manager (unfortunately abbreviated as **NPM**, but fortunately having nothing to do with Node.js), 
coupled with [CloudFlare DNS](https://cloudflare.com), allows me to create a subdomain and handle traffic to 
and from that domain. The admin panel in NPM allows me to request and install a LetsEncrypt SSL certificate so 
that communication between whatever device I'm using and the server is as secure as can be. I don't expose all 
the services on Proxmox to the open Internet, just those where I need to be able to access the information 
using a mobile app over secure connections. 

For this project, I'm NOT going to use Nginx and CloudFlare. I explicitly do NOT want to take a chance
that a stray CVE or missed setting will give someone my passwords. But, I want the type of convenience
they provide in having HTTPS and DNS. 

That's where Tailscale comes in.

### Tailscale

[Tailscale](https://tailscale.com) is a personal VPN that implements the WireGuard protocol. The folks that 
built Tailscale deserve a few beers for implementing something that "just works". With Tailscale installed 
on a server, workstation, phone, router, or NAS, you can access protected assets when you're away from your 
local network without exposing them to the open Internet.

This is exactly what I want for my password server. I don't want it facing down Internet randos on a daily 
basis. It needs to just sit there and run on my local network. When I need to sync with it, I will start 
the Tailscale client on my phone or laptop to connect to my home services. Otherwise, they remain out of reach 
to casual visitors and hackers.

### VaultWarden on Proxmox

As I mentioned earlier, I've been happy with Bitwarden from a user perspective. Of all the password
managers, it has had the fewest reported intrusions or CVEs logged against it. I feel comfortable
putting my passwords in there, but I'm aware that not every service is guaranteed to remain secure.

[VaultWarden](https://github.com/dani-garcia/vaultwarden) is an open-source alternative to the Bitwarden 
back-end server. The project exists with the blessing of the Bitwarden team. There is even a Bitwarden 
employee working on VaultWarden to ensure the servers are compatible. 

A Proxmox Community Script 
[installs VaultWarden](https://community-scripts.github.io/ProxmoxVE/scripts?id=vaultwarden) as an
unpriviliged Linux Container (LXC). The installer builds from sources, so it may take a little longer 
than other Proxmox projects or Docker instances that you have used. For my N100 mini-PC setup with
32GB of RAM, it took around 20 minutes.

## Get On With It, Already

At this point, I'm going to assume that you have the VaultWarden project installed and running as an LXC on
your Proxmox server. For the sake of this tutorial, I'm assuming the local network address of that instance is
**192.168.1.10**.

When you install VaultWarden with the default settings, the directory on your Linux server is **/opt/vaultwarden**. 
Within that directory is a config file called **.env** where you'll have to enter some values. Use your favorite 
text editor to make changes, which is almost certainly *[vi](https://en.wikipedia.org/w/index.php?title=Editor_war)* 
because you're too smart to use emacs.

Create a Tailscale account and install the clients on the devices that you need to access. This will take some
time, but won't be difficult if everything you're using falls into the broad category of "common computing platforms".
I won't explain how to configure Tailscale in this tutorial, because there are dozens of tutorials on 
[YouTube](https://www.youtube.com/@Tailscale) and [elsewhere](https://tailscale.com/docs/quick-guides) that do just that. 

In the end, you'll have a pseudo-domain on the Tailscale network, such as *"correct-horse"*, which I will use as an 
example. Each device on your *tailnet* will have a unique subdomain under this pseudo-domain. So, your phone might 
be **pixel9a.correct-horse.ts.net** and your vaultwarden instance might be **vw.correct-horse.ts.net**.

### Admin Access

By default, the VaultWarden admin token is empty in the configuration to force the administator to go through these steps 
to set it up. For these steps, you'll use the Proxmox console for your VaultWarden instance.

This [wiki page](https://github.com/kagiko/vaultwarden-wiki/blob/master/Enabling-admin-page.md)
describes how to set up a token. You'll want a passphrase like you do with your SSH key, so think
of something like ["correct horse battery staple"](https://xkcd.com/936/?correct=horse&battery=staple).
Feed that phrase into the key encoder using this command.

> `/opt/vaultwarden/bin/vaultwarden hash`

Copy the line beginning with *ADMIN_TOKEN* and paste it into **/opt/vaultwarden/.env**. Restart vaultwarden with 

> `servicectl restart vaultwarden`

If anything goes wrong in restarting the server, check the output of the service with 

> `journalctl -u vaultwarden` 

Go to the bottom of the output to find the most recent error messages.

Now, access your instance from a web browser and you'll be presented with a login form, where you will enter
your passphrase of **correct horse battery staple**.

> `http://192.168.1.10/admin`

Your browser will likely complain that the site is not secure. For now, you'll have to bypass this warning.
Each browser has a different process to get around this warning, which is there for perfectly valid reasons.

* [Firefox](https://superuser.com/questions/1298054/ignore-invalid-ssl-certificate-chrome-ff-or-other-browser)
* [Chrome](https://superuser.com/questions/27268/how-do-i-disable-the-warning-chrome-gives-if-a-security-certificate-is-not-trust)
* [Edge](https://en.wikipedia.org/wiki/Bless_your_heart)

<p align="center"><img src="/assets/images/2026-03-01/01-vw-admin.jpg"/></p>

We'll come back to the admin panel later. For now, just remember your passphrase.

### SSL with LetsEncrypt

The VaultWarden installer generates a self-signed certificate and key when it runs. These are good enough
to experiment with, but your browser is going to warn you that they aren't generated by a recognized
Certificate Authority (CA). You'll need to generate a recognizable certificate from a known authority
before you trust this setup with your passwords.

If you've worked with servers, you're no doubt familiar with [LetsEncrypt](https://letsencrypt.org/), so I 
won't bother going into detail about it other than to say it's a great service and I'm happy to not have 
to pay a corporation to generate a file of seemingly random digits.
[Tailscale will request LetsEncrypt](https://tailscale.com/docs/how-to/set-up-https-certificates) certs
for any device connected on a tailnet, with some caveats.

Using the Proxmox console for VaultWarden, type

> `tailscale cert --cert-file /opt/vaultwarden/vw.correct-horse.ts.net.crt --key-file /opt/vaultwarden/vw.correct-horse.ts.net.key vw.correct-horse.ts.net`

Once this is complete, which may take 15-30 seconds, you'll have the certificate and key files in the 
**/opt/vaultwarden** directory. 

Open **/opt/vaultwarden/.env** and edit the line that begins with **ROCKET_TLS=** to point to these 2 new files.

> ROCKET_TLS='{certs="/opt/vaultwarden/vw.correct-horse.ts.net.crt",key="/opt/vaultwarden/vw.correct-horse.ts.net.key"}'

Next, note the line that begins with **ROCKET_PORT=**. You'll want to set this to **443**, as that's the port that
secure traffic is routed to. You don't need to set the **ROCKET_ADDRESS** value, although you can. Save the file and
exit the editor.

Restart VaultWarden with 

> `servicectl restart vaultwarden` 

If everything works, you can now go to your VaultWarden instance on a machine connected to Tailscale with

> https://vw.correct-horse.ts.net/admin

### Admin Panel Change

From the admin panel, you'll enter your passphrase and now be able to start administering users. You may notice
a warning that your domain name doesn't match that value from when you installed VaultWarden. Go into **General settings** 
and change the Domain URL to your tailnet subdomain name (e.g. https://vw.correct-horse.ts.net)

The rest of the admin panel settings and users are for you to decide on your own. At this point, you have a
secure connection from your desktop browser to the server and you can start importing passwords from your
other services if you choose.

### New Accounts

The one item you should consider is if you want users that find your site to be able to create new accounts.
For me, that is absolutely not the case. I'm only creating accounts for my wife and I. You can control this behavior in 2 ways. 

1. Add `SIGNUPS_ALLOWED=false` to **/opt/vaultwarden/.env**
2. Uncheck the **Allow new signups** box in **General Settings**

In either case, the web UI will continue to show the new user form, but the create button will not be enabled 
and the back-end will not accept any attempts to create a new account.

### Set Up Mobile Devices

The VaultWarden service is set up to be a drop-in replacement for the Bitwarden back-end. 

> **NOTE:** I tried to take screenshot of the screens  I'm describing and the app (rightly) prevented me from doing
so. I didn't want to take a photograph, so you'll just have to make do with the descriptions I provide.

Install the [official Bitwarden app](https://bitwarden.com/download/) on your mobile device or PC. By default, these 
apps are configured to use the bitwarden.com back-end. There is a drop-down menu below the login credentials where you 
can choose to change from bitwarden.com (or bitwarden.eu) to *self-hosted*. Choose self-hosted from the list. 

The self-hosted server screen is a form with many fields. The only one that matters for now is the **Server URL** at the
top. For this field, enter **https://vw.correct-horse.ts.net** and press the **Save** button.

Once you are able to log in, find the *Sync* option to connect to your server. After a few seconds, you should
have passwords on your mobile device. 🎉

## Wait 90 Days

One detail that I deliberately skipped over earlier is the lifetime of the SSL certificate and key that gives us 
HTTPS access. LetsEncrypt certs expire after 90 days and must be renewed. 

Because Tailscale doesn't know where or how we're using the certificate they requested, they cannot issue renewals 
without us initiating the request. It's up to us to issue the `tailscale cert` command again before the old one expires
and then move or rename the files as necessary. This is described in more detail in 
[the Tailscale documentation](https://tailscale.com/docs/how-to/set-up-https-certificates).

I haven't needed to renew my certificate yet, so I can't say for a fact this will work. I tried to renew the cert
1 day after issuing it, but it said the cert was not yet expired. I need to figure out how often I need to request
renewal. For now, this is how I have configured my certificate renewal.

Attempt the job at 2:00 AM on the first of each month using [cron](https://en.wikipedia.org/wiki/Cron).
I added an entry in a new file called **/etc/cron.d/renew-tailscale-cert** with this information:

> `0 2 1 * * root tailscale cert --cert-file /opt/vaultwarden/vw.correct-horse.ts.net.crt --key-file /opt/vaultwarden/vw.correct-horse.ts.net.key vw.correct-horse.ts.net`

### Dead Ends

There were a couple of choices I nearly made that sent me down paths that I probably would've regretted later. Fortunately,
I caught myself before going too far down these paths.

1. **Making the service available on the open Internet**. Actually, this was never a goal, but I experimented by putting
vaultwarden.my-domain.com on CloudFlare with a empty server that returned a 500 status code. This server got a lot of
traffic from looking at the logs. I suspect miscreants watch for new DNS entries and pounce as quickly as they 
can when they find one with an interesting name.

2. **Trying to install the tun kernel module in my LXC**. It turns out that an unpriviliged
LXC doesn't have this module by default because it needs to run as root. I had to enable it by shutting the server down and adding the following line at the bottom of the LXC config file using the Proxmox console (e.g. **/etc/pve/lxc/123.conf**). After saving and starting the container, the Tailscale service would start and I could connect to the control server with `tailscale up`.
> lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file

3. **Installing the LetsEncrypt certbot tool on my server**. Normally, this is how you would get a certificate renewed, but
I couldn't figure out which webserver was being used by VaultWarden. It turns out to be neither Apache nor Nginx, but its
own server framework called [Rocket](https://rocket.rs/). While trying to decide how to handle this, I realized that Tailscale 
does the request for users with better results than I could do myself.

4. **Thinking I needed certs in PEM format**. When the VaultWarden server would fail, it was pointing to
the cert files in the error logs. I thought it was because the certs were in .crt format and the examples 
showed PEM files. I tracked down how to convert the files with openSSL, but it turns out that I had 
mis-typed the file name in the .env file and just needed to correct the file names. 😳

## Wrap It Up

I have all this working on my devices and haven't had any problems in about a week of use. I was able to reconfigure my
devices and my wifes' in a few minutes each. My wife doesn't understand what Tailscale is for, but trusts that I know
what I'm doing.

The process of installing and figuring out the configuration details took about 6 hours over the course 
of a few days. Hopefully, this post will help the next person around some of the potential missteps.

Once again, many thanks to the devs that create quality open-source projects. I hope that by documenting these
projects and linking to their sites that I'm able to help shine some light on great software.

## Talk to Me

I didn't start out trying to write this as a tutorial, so I had to go back in my console and browser histories 
to see what I was doing and why. If, for any reason, you find something incorrect or I need to update the instructions, 
I'll happily do so.

My GitHub profile is linked in the sidebar. I'll absolutely take PRs and merge them, but you can also just
send an email to the gmail account with the same user name as my GitHub profile. Similarly, with Discord.