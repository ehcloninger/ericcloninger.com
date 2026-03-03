---
title: "My Music Hosting Project"
excerpt_separator: "<!--more-->"
toc: true
categories:
 - blogs
tags: 
 - howto
 - proxmox
og_image: /assets/banners/seo-banner.png
---

With some time on my hands, I delved into organizing my digital music library for online streaming.

<!--more-->

**NOTE**: This is another long post about a technical project.

---

## The Boring Background Bits

Like many software people, I'm a bit obsessive about categorizing and organizing things. While my office is cluttered, my 
photo and music collections are not. I build hierarchies with detailed metadata about the content of the files. Photos are
tagged with information about where the photos were taken, the names of the people in them, and the subject matter, if
applicable. 

For Christmas, I made photo books for my children with pictures of them from their childhood with their mother and 
I. I was able to do this over the course of a few evenings by querying a database built from all that metadata using a tool 
called [Digikam](https://www.digikam.org/) to sort and select the images. The books ended up with about 2000 photos for each 
of them, grouped by year and sometimes highlighting special events like visiting Disneyworld or the 3-week trip to Europe we 
took in 2015. My kids loved the photos and the effort wasn't excessive because of the attention I paid to maintaining
accurate metadata through the years.

For my digital music collection, I have always had a fairly consistent strategy so that I could find and play the songs I
wanted on an iPod or my Android phone. The CDs were ripped to MP3 with as high a bit-rate as possible at the time. Through 
the years, I've added the relevant metadata using a variety of tools that have been built for the purpose, but not to the 
extent that modern "data hoarders" do. My collection grew organically as I added new physical media. Years ago, I removed 
the disks and their liner notes to sleeve cases and put the plastic jewel cases in the recycling bin. I revisit the physical
media occasionally, when new technology comes along that lets me get better quality playback.

## My Own Private Spotify

Which brings me to my next project. I want to replace the audio streaming service that charges a monthly fee with something
on my own hardware with the content that I paid for decades ago. While I won't have immediate access to nearly every song
recorded, I don't have to worry that my subscription is subsidizing hateful and harmful podcasts on that streaming service.
I'll just build my own Spotify, without Joe Rogan.

### You Can Take My Server When You Pry My Cold, Dead Fingers From the Keyboard

A few years ago, I built what is commonly referred to as a "homelab" server. This is a dedicated piece of computer hardware
that runs multiple versions of Linux simultaneously, each with a different purpose. These individual instances run without
knowledge of each other using a very cool open-source software product called [Proxmox](https://www.proxmox.com/en/). 

One of these servers runs my home automation system, while another hosts my photo gallery. I have my own self-hosted versions 
of many popular cloud-based commercial tools. I spent several years learning and improving this system, to the point where I 
can have an idea of something I want to do and I can have a new service up and running on a secure network in under an hour.
There is a great [community of developers](https://community-scripts.github.io/ProxmoxVE/scripts) that take open-source server 
projects and wrap them into easily-installed packages hosted on Proxmox.

### Sifting Through the Options

I trialed several self-hosted audio streaming projects, including [Plex](https://plex.tv) and Jellyfin. While I have a lifetime 
subscription to Plex, their audio playback setup isn't ideal. The PlexAmp app is nice, but the real hurdle was getting my playlists into their system. I wanted something that worked with open standards and didn't require me to convert into other formats. I settled on [Navidrome](https://www.navidrome.org/), which uses the Subsonic streaming protocol, allowing me to choose from dozens of mobile apps.

The web interface of Navidrome isn't Beautiful, but it's functional. It allows me to view my collection by a number of criteria. it allows me to use my existing playlists unaltered, and it lets me share my collection with my family. Once I had my music collection with playlists on the server, I quickly became aware of one notable problem for someone with an obsession for organizing...

## My Metadata is Crap!

As of this writing, I have 30,831 MP3 files from my own ripped CDs and others from download sites. The quality of the metadata from downloaded songs is very inconsistent and I find it difficult to enjoy my music when a song I'm expecting to hear doesn't play because the metadata is incorrect and the song is attributed to a different artist. So, I needed to do a massive cleanup operation on the downloads and some maintenance on the ones I had ripped to MP3 a decade earlier.

To get my metadata into order, I wrote some Python scripts to help. The metadata is in a format called [ID3](https://id3.org), which is not a recognized standard. Many different software tools have implemented ID3 their own way using their own conventions, so getting information on the right way to do things is a bit messy. 

Fortunately, there is a project called [Musicbrainz](https://musicbrainz.org) that is attempting to catalog every recording and they've made a very good start on things. They have a software tool called Picard that will attempt to match your music collection with the metadata they have in their catalog. In addition to setting values correctly, the software inserts special values called GUIDs (Globally Unique IDentifiers) into the metadata so that other tools/playback software can know more about the song. All for the ridiculous price of nothing. 

Something I did to make this process much faster was to host the MusicBrainz Server on my own hardware. The MusicBrainz project provides an [installable package](https://musicbrainz.org/doc/MusicBrainz_Server) that runs on Docker or raw hardware. I am experimenting with Docker on an old laptop running Ubuntu, so it drastically sped up the process of tagging music by querying a machine on my local network rather than the open Internet. I contributed a small sum to their project [via GitHub](https://github.com/ehcloninger?tab=sponsoring) after using the tool and server software for a few days.

After getting all those songs matched up, I wrote some Python code to remove the extra metadata tags that the download sites add that point back to their site. These fields provide no value and so I removed them. I also wrote some Python to "lint" the collection, looking for inconsistent dates and performer names. I used the Python [Mutagen](https://mutagen.readthedocs.io/en/latest/) library to process and write the updated tags.

### Can I Get in a Word or Two

As I was finishing the metadata cleanup, I came across someone else's [passion project](https://lrclib.net/) to bring in lyrics for songs. This is a nice feature that both Spotify and Youtube Music have but, as with ID3, there's no standard around it. There are some conventions that are widely adopted. This project has a simple app that runs on desktop and using the information from MusicBrainz to identify songs, will add the lyrics to a file if they can be found in their database. 

As with MusicBrainz' Picard, the LRCLIB project has a [self-hosted server](https://github.com/tranxuanthang/lrclib) and database to speed up the process. I was able to add lyrics to my entire collection of 30,000+ songs in under an hour using LRCGET and my self-hosted server. As with MusicBrainz, I donated a small amount to the author of LRCLIB to show my appreciation for their efforts.

## Play it Again, Sam

One of my problems has been that when I create a playlist and then move the songs to a new medium or server, the playlist itself is stale and no longer points to the correct directory. This isn't a "me" problem, it's universal when you have an external file to list the contents of the playlist. So, I decided to put the playlist membership into the song metadata itself. This solves the problem for me. Some songs belong to many playlists, so the way I implemented this was to re-use an existing field that was made for this purpose. I have Python code that will stuff multiple values into this field meant for a single value. Then, I have some code that will generate the playlist based on the current location of the song. It's not clean, but it works for me.

I have the scripts I wrote on Github, but the code quality is low. I need to clean up the scripts to properly use classes and have some documentation before I share them publicly.

## From My Server to My Ears

Like most everyone else, my phone is tethered to my body most of the time. I have a subset of my music collection on the phone and I can play it anytime and anywhere I want. I used software called [Poweramp](https://powerampapp.com/) on my phone for many years. I think I originally paid for it in 2010 or about that time. It works really well.

However, Poweramp does not connect to my personal server, so I did some research on different software products. I tried one called [Substreamer](https://substreamerapp.com/) for a while and I'll likely use it more, but it is not updated very often.

I trialed another product called Symfonium and eventually paid for it. However, it requires a bit of customization to get everything "just right". There are a lot of people on forums discussing their configurations and the developer is involved, but they are rather prickly about any comments that aren't praising their creation. I don't need this person as a friend, but when I have a question, I don't need their peevish comments that aren't helpful. So, I continue to look.

My iPod touch doesn't have much life left in it. I still have an original FireWire scroll-wheel iPod and I would love for that thing to make a comeback (with USB-C, of course). I bought a knock-off of the Sansa Clip on Amazon and it's everyone you might expect from a $30 piece of crapware. I have a few audiobooks on it, but it's mostly unusable as a music player.

### Cans

I probably have an addiction to headphones. I can't count the number that I actually have. On my desk, right this moment, I see 7 different things that I can listen to my music on. So, I'm far from unbiased. 

I generally don't like things **in** my ears. I tried Samsung's ear bugs when I was working for them and I found them uncomfortable to wear and the sound quality was bad. I have a cheap workout neck-band headset with buds that go in my ears. I use these while doing yardwork, mostly to keep the sound of power equipment from damaging my ears even more. They work well enough for that use case, but it's not exactly when I want to listen to music with high fidelity.

My favorites would have to be

* [Sennheiser HD 380 Pro](https://www.amazon.com/Sennheiser-HD-380-PRO-Headphones/dp/B001UE6I0G) wired headphones. The sound quality is exceptional and they are very comfortable for the long listening sessions. However, the coiled wire is a bit taut for my liking. I prefer a non-coiled option. I bought a Sennheiser wireless telephone headset a few years ago and was less impressed by it.

* [Plantronics Backbeat Pro](https://www.amazon.com/Plantronics-Wireless-Noise-Cancelling-Backbeat/dp/B01MY4P9EZ) wireless headphones. These big over-the-ear cans are comfy and they are smart enough to sense when I take them off and they will alert my music source to stop playing. They're a bit outdated now and charge with micro-USB, so I've stopped taking them on trips because I prefer everything to charge with USB Type-C.

* [Anker SoundCore Q30](https://www.amazon.com/Soundcore-Cancelling-Headphones-Comfortable-Bluetooth/dp/B08Q89DN6V). These are inexpensive noise-cancelling headphones by a company that is prevalent on Amazon. They have good battery life and decent noise cancelling. The controls are a bit difficult to operate, but at the price I paid, I don't worry overly much about them getting lost in an airport or broken in my backpack.

### Cones

As for music playback in an open enviroment, like when I'm home alone or we have friends over... that's an entirely different matter and I'm still trying to come up with a good solution. I have a Soundcore Bluetooth boombox that is waterproof and does an adequate job. It's a bit bassy, but the battery life is good and I don't worry if I left it out in the rain.

I had a bunch of old-school analog equipment that I gave to a guy that fixes and sells it to the retro market. I'm looking for something modern to play around the home that isn't tied to a commerical cloud service and hates their customers (ala [Sonos](https://www.engadget.com/2019-12-31-sonos-recycle-mode-explanation-falls-flat.html)). 

Time will tell.