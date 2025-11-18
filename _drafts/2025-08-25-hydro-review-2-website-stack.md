---
title: "Website Decisions"
excerpt_separator: "<!--more-->"
toc: true
categories:
  - blogs
tags: 
  - howto
---

This is the second in a series of posts about a down-time project that I undertook.

<!--more-->

If you haven't read the [first article](), you may want to read that first to understand the scope of the project. This post is mostly focused on the technical decisions and processes that I ran to collate the data into something that can be used to create a website.

# User Stories

I need to present several user journeys through the site. 

1) First is the front-first visitor that finds the site by typing the domain into their browser or linked from somewhere that lands them at the top. I need them to find something of interest and hopefully find their time well-spent. This is not a commercial site, so I'm not going to spend a lot of effort reading visitor logs, although I will activate Google Analytics, mostly to make sure people are finding what they need.

2) Next is someone coming in from a web search. Over time, I hope that people looking for their ancestors in Google Search will find the site. This person likely wants a specific family name and to find out what became of a relative. In reading the early editions, it seems that a lot of people died of things we can now easily prevent. A ruptured appendix or tonsilitis was a life-threatening illness in 1904. One man, thrown in the town's jail for an evening, was found dead the next morning with a bottle of laudanum (liquid codeine) by his side. Those peoples' names aren't reflected in the current town's populace, but they mattered to someone, somewhere.

3) Someone looking for a specific date. Historical dates such as statehood and the declaration of war. Those should be found with just a few clicks.

If someone is looking for information on a site, the best experience is to have information that has a close affinity to what is being read immediately available without a search or going to the front page and navigating. This is a fuzzy issue and will require some tweaking along the way. For the short term, I'm going to make sure that there are plenty of short-cuts to issues by year with drill-down links to specific issues.

One important aspect is to get the link hierarchy correct from the start. Once the pages are online and picked up by search engines or linked from social media, I don't want to have to invalidate them or rely on redirections.

# Technical Stack

As I mentioned earlier, once I'm done with the site, I don't want to spend my evenings and weekends making sure people aren't trying to attack the site. For that reason, I'm going to forego traditional CMSes like WordPress, Joomla, and Drupal. Like my personal sites, I want the attack surface to be as small as possible and the potential gain for the attackers to be zero. They might disable the site by flooding my host, but they won't add spurious files and malware. They can scrape the content, but honestly I'd prefer they just ask and I can provide them with a zip archive.

The site should be capable of being installed on any server without regard to domain. All the links should be relative to the site itself. While a top-level domain exists for this site, someone could take my site and put it on their own server and it should continue to work.

## Hosting

I'm going to put this on AWS because that's where my skills lie. I've played with Azure for a few days, but as in the old days of "nobody ever was fired for recommending IBM", I'll build and deploy on AWS because it's the most likely host for anyone picking up the reins if and when I'm gone.

This will eventually reside as a static site served up in an S3 bucket. There will be no EC2s or Lambdas running. The only expenses should be storage and bandwidth.

The content is going to be somewhere around 100GB of PDFs and JPEGs and I don't want to put these assets into my Github account, I'll store them in a parallel S3 bucket from the site code itself and reference it for any of these images. This should allow me to deploy the text content of the site quickly without having to copy a bunch of files that will never change.

Hopefully. TBD.

## Jekyll 

I've chosen to work with [Jekyll](https://jekyllrb.com). This is arguably the most well-known of the static site generators (SSGs) and Github itself provides a lot of support for it. There are hundreds of themes and tools made to work with Jekyll. I can host the site locally, on Github.io, as well as in the cloud. There are many CI/CD scripts to support Jekyll build and deploy models. Depending on the budget for this project, I may deploy to Netlify or AWS. I have experience running Jekyll sites on AWS and I like the fact that I can easily control my costs there. Netlify has a great build and deploy model for Jekyll sites, but they have a bit of a bad reputation among hobbyists these days due to well-publicized problem that happened in 2024. Given that this site has a large quantity of static images, I'll have to be careful about CDN hosting and bandwidth costs.

The drawback (and benefit) of Jekyll and every other SSG is that everything about the site must be known in advance. There is a robust scripting language to build pages and link between them, but every new piece of content requires rebuilding the site. Given that the Hydro Review ceased publishing in the 1990's, this doesn't seem like that big of an impediment once it's all running.

The open issue, at least as of today, is search. The OCR is poor, at best, and the word list contains an excessive amount of garbage. I can use the [Lunr](https://www.stephanmiller.com/static-site-search/) search engine to build an index that will allow on-site navigation. Of course, there's always the major search engines.

## Jekyll Themes

There used to be a somewhat thriving market in Jekyll themes when I first started building sites with this tool in 2012. It would seem that at least one of these marketplaces has gone belly up. But, there plenty of others. My own personal site and another for a hobby I play with use a theme called [Minimal Mistakes](). 

Minimal Mistakes is the poster child for aftermarket Jekyll themes, due to its' popularity. Part of the reason is it's still actively maintained and it looks good on desktop and mobile devices. The drawback is that any customization requires writing code in a somewhat obscure dialect called Liquid.

I searched for themes that were meant for newspaper or literary archives. I found one, but it was on one of those defunct sites and the code had not been maintained. If I'm going to start this project, I hope to have a theme that is maintained to deal with future technical hurdles. Who knows when we will have in-eye monitors, but it may be in my lifetime. I hope not, but one never knows.

I'm not 100% convinced that I will stick with Minimal Mistakes, but for now, I'm going to begin with it. If I change, I'll need to modify the structure of the site, but that shouldn't involve too much technical debt. I hope.

## Images

I converted all the PDF pages to images because images have a much better choice for display than PDFs. I used the poppler tools to convert the pages, one by one, into JPEG images at 60% quality and 1280 pixels high. This is fine for viewing on a screen to get an understanding of the content on the printed page. For actual reading, the full issue PDFs are available on the same web page.

The command I used to do this work was:

`pdftocairo -singlefile -jpeg -jpegopt quality=60,optimize=y <file> <output>`

The `pdftocairo` tool has some odd behaviors that I had to work around. The biggest one was that it assumed the PDF input was multipage, so no matter what I gave for the output, it always put a "-1.jpg" as a suffix. I would rename the files with a bash script, but finally I read the docs in more detail and found the `-singlefile` parameter to suppress that behavior.

I realized I didn't need to push something 4000px tall to a screen. I could probably do better with an image sized 1280px, which would save space and bandwidth. I duplicated my images folder and used the ImageMagick `mogrify` command to do the resizing in place. Mogrify is nice for this, but it does overwrite the input, so make sure you have a backup.

`mogrify -quality 60 -resize x1280  -path . *.jpg`

After converting to JPEG, I had another script to make smaller thumbnail images that could be shown on each issues' post page. For this, I used the ImageMagick suite of image tools. ImageMagick is great because if something can be done to an image, there's a tool or option to do it. However, in order to do the simplest of operations, you have to wade through a dozen pages of examples and StackOverflow questions, each not quite what you need.

`convert <input> -thumbnail '450x>' -gravity North -extent 450x450 <output>`

This command creates an image that consists of the entire width of the image and as much of the page length so as to make a square to show in the gallery of printed pages at the bottom of each issues' post.

# Putting It All Together

With 3 1/2 years of data converted into the file types I wanted, I started figuring how how to lay it out in the framework of the Jekyll theme. Jekyll expects the builder to put things in known locations so that its' scripts can do their work. There are specific names and formatting the system expects and there isn't much deviation allowed.

I wrote a Python script that takes the index spreadsheet (as CSV) and generates a set of static pages based on the contents. It optionally copies the image files into the Jekyll project structure. I'll be honest, by the time I finished with it, I realized that it is not the best code I've written and I wouldn't want anyone judging me based on it. But, it works and it has plenty of exception handling and error reporting to allow me to pick this project up again if we get the remaining issues someday.

# Next Steps

Once I had the site working locally with 3 1/2 years of data, I spent a couple hours just going through the site and looking for names I knew. I showed my wife, who immediately started looking for the names of her kinfolk. Seeing someone else use the site made me rethink the Data Architecture part a bit and give me ideas for improving the layout and navigation over time.

At this point, I went back to the task of sorting through the remaining 40 years of issues.
