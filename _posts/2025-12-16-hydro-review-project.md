---
title: "Technical Deep Dive of a Newspaper Archival Project"
excerpt_separator: "<!--more-->"
toc: true
categories:
  - blogs
tags: 
  - howto
---

I took on a project to help a local non-profit bring their data online.

<!--more-->

**NOTE**: This is a **very** long post about a technical project that I started back in September. I tried to keep the narrative as linear as possible, but the truth is that it was a series of going down paths and returning back to the main task.

I included information about research paths that didn't work out, but not in great detail because I didn't keep exhaustive notes. This wasn't intended to be a Masters thesis, but it probably could've been.

---

In high school, I worked for our home-town newspaper. With the energy of a teenager, I ran about the office lifting boxes, composing headlines, developing film, printing photos, laying out pages, and stuffing mailers with each weeks' issue. I parlayed my earnings into the purchase of a Pentax K1000 35mm camera and a 50mm lens, which I still own. One of my former colleagues and her husband moved to a nearby town and set up their own weekly newspaper. I helped them set up the darkroom and showed her husband how to mix and use the chemicals, but it was too far from college to work there.

Time moved on and eventually the local newspaper shut down as a lot of small town newspapers did. About the time the Internet arrived, the final owners decided they couldn't keep it running. The newspaper operated by my former coworker and her husband continued up to her death in 2024. 

A few months ago, my former colleagues husband and I talked about bringing those newspapers online and that was the beginning of this project.

# The Data

The content for this project came from the [Oklahoma Historical Society](https://www.okhistory.org/). They provided us with PDF files containing scanned images of the Hydro Review from July 1904 thru December 1947. Each PDF contained 2 or 3 years of issues, with roughly 1,200 pages in each PDF file. There are a few missing issues that were indicated by a single blank page with the words "MISSING FILES" in large letters. 

# Big Picture

What we wanted was a site that would allow easy browsing and searching of the content. We imagined that most people finding the site want to search for a relatives' name or find a specific date. We wanted to make those user journeys as easy as possible, given several constraints...

1) We couldn't spend much money on the project, relying on grants and our own contributions. 

2) For handling of the data and converting files, I would need to rely on Free and Open-Source Software (FOSS) to the largest extent that I could. In situations where I couldn't use FOSS, I wanted to use products that do not require a subscription.

3) I can't be a system administrator. I can't spend time fighting off people trying to get into the administration panel or inserting malicious code into pages, so however the site operates needs to be as low-maintenance as possible.

4) The project must be automated as possible. While the project won't scale in the sense that I'll be adding content years from now, there were thousands of pages to be processed. Fortunately, there are plenty of frameworks and CI/CD processes to make this simpler.

## User Stories

I need to represent several user journeys through the site.

* The **front-first visitor** that finds the site by typing the domain into their browser or linked from somewhere that lands them at the top. I need them to find something of interest and hopefully find their time well-spent. This is not a commercial site, so I'm not going to spend a lot of effort reading visitor logs, although I will activate Google Analytics, mostly to make sure people are finding what they need. I'll look at high-level metrics like time on page and pages visited.

* Someone coming in from a **web search**. Over time, I hope that people looking for their ancestors in Google Search will find the site. This person likely wants a specific family name and to find out what became of a relative. In reading the early editions, it seems that a lot of people died of things we can now easily prevent. A ruptured appendix or tonsilitis was a life-threatening illness in 1904. One man, thrown in the town's jail for an evening, was found dead the next morning with a bottle of laudanum (liquid codeine) by his side. Those peoples' names aren't reflected in the current town's populace, but they mattered to someone, somewhere.

* Someone looking for a **specific date**. Historical dates such as statehood, the stock market crash, and the declaration of war. Those should be found with just a few clicks.

* Someone coming from a **social media** link, primarily Facebook. Like it or not, Facebook is the primary platform for people sharing genealogy research and searching for family connections. We want the site to operate properly when linked from these platforms.

If someone is looking for information on a site, the best experience is to have information that has a close affinity to what is being read immediately available without a search or going to the front page and navigating.

One important aspect is to get the link hierarchy correct from the start. Once the pages are picked up by search engines and linked from social media, I don't want to have to invalidate them or rely on redirections. For now, 

# Converting and Processing the Data

All of the work described here was done on a Windows PC with Python scripts running under Powershell. All of the image and file processing is written in platform-agnostic Python code. Some of the image processing steps used GUI tools on Windows, but the process of converting images and building the website can easily be done on Mac OS or Linux.

On a few occasions, I found issues with open-source tools or libraries and reported errors back to the maintainers, typically via Github issues. The original files were about 13GB of PDF pages. The first thing was to separate them all into individual pages and figure out how to map those into issues by date. 

There are a set of tools to handle PDF files collectively called ["poppler"](https://poppler.freedesktop.org/). I used `pdfseparate` to save all the files using the original file name as a base and a sequential number for each separate page. This resulted in 13,224 individual PDF files, most of which were roughly 1MB in size. 

There was a small glitch in the software that about every 500 pages, I would end up with a file that was only 1 page but was the size of the entire PDF archive. Fortunately, when I created a script to just convert these specific pages, they were saved with an expected file size. I didn't try to understand the nature of this glitch or report an issue back to the project.

## Creating an Index of Pages

Now that I had individual pages, I need to figure out how to get those 13,224 sequentially numbered files into file names that reflected their content. The first stab at this resulted in 3 1/2 years of issues which required an entire evening of copying and pasting in Excel. I tracked the year of the issue, volume number, edition numbers, month, day, and file name, as below:

|Year|Volume|Edition|Month|Day|Page|File|
|----|------|-------|-----|---|----|----|
|1904|3|35|7|15|1|1904-07-15-1905-06-23-001.pdf|
|1904|3|35|7|15|2|1904-07-15-1905-06-23-002.pdf|    
|1904|3|35|7|15|3|1904-07-15-1905-06-23-003.pdf|    
|1904|3|35|7|15|4|1904-07-15-1905-06-23-004.pdf|    

The file names came from doing a directory listing and just pasting the output into Excel. The years and volumes were sequential and I was able to build formulae to automatically number them. Similarly with the month and days, I built formulae that would check the values of previous rows and highlight if certain conditions occurred, such as an odd number of pages, which would not likely exist in a newspaper with content on both sides of the page. The exception to this was the 30+ issues that were missing and contained a single "missing issue" page.

One thing that helped this process was using a desktop PC and 2 monitors. I created thumbails of the pages and opened them with [Irfanview](https://www.irfanview.com/) on the second monitor. This allowed me to quickly see how many pages each issue has. Most of the early issues had 6 pages, that eventually grew to 8 pages. However, there would be special issues that had a sales insert or perhaps legal notices from the county. So, I couldn't rely on dozens of issues having the same number of pages and had to visually inspect them all.

Even though this was a manual process, I was able to build out the spreadsheet over the course of 2 evenings while watching baseball playoff games. Once I had the spreadsheet built, I wrote a Python script that read in the spreadsheet and renamed the files to indicate which issue they are in. In the end, I had a folder structure that looked like this...

```
+ -- 1904
|    + -- 1904-07-15
|    |    + -- HR-1904-07-15-01.pdf
|    |    + -- HR-1904-07-15-02.pdf
|    |    + -- HR-1904-07-15-03.pdf
|    |    + -- HR-1904-07-15-04.pdf
|    + -- 1904-07-22
|         + -- HR-1904-07-22-01.pdf
|         + -- HR-1904-07-22-02.pdf
|         + -- HR-1904-07-22-03.pdf
|         + -- HR-1904-07-22-04.pdf
...
+ -- 1905
|    + -- 1905-01-06

```

## Converting to Raster Images

PDF is fine for documents, but it's not a good storage choice for image data that needs to be manipulated. I converted the PDFs to JPEG in high quality for all further activities using the poppler tools.

`pdftocairo -singlefile -jpeg -jpegopt quality=92,optimize=y <file> <output>`

The `pdftocairo` tool had several odd behaviors that I had to work around. The biggest one was that it assumed the PDF input was multipage, so no matter what I gave for the output, it always put a "-1.jpg" as a suffix. I would rename the files with a script, but finally I read the docs in more detail and found the `-singlefile` parameter to suppress that behavior.

After converting to JPEG, I had another script to make smaller thumbnail images that could be shown on each issues' post page. For this, I used the ImageMagick suite of image tools. ImageMagick is great because if something can be done to an image, there's a tool or option to do it. However, in order to do the simplest of operations, you have to wade through a dozen pages of examples and StackOverflow questions, each not quite what you need.

`magick <input> -thumbnail '450x>' -gravity North -extent 450x450 <output>`

This command creates an image that consists of the entire width of the image and as much of the page length so as to make a square to show in the gallery of printed pages at the bottom of each issues' post.

## OCR Investigations

This is the part of the project that is the most technically complex and the part that still doesn't have a great solution. I have spoken to half a dozen people for whom this is their profession and they all admit it's an area where they struggle. Given the budget and the lack of access to commercial scanning tools, we're having to rely on open-source or free-tier tools.

I started by running the [tesseract](https://tesseract-ocr.github.io/) tool on the images to see what it would produce. Tesseract is the most commonly used OCR library with a rich support for C++ and Python. The initial results were not great because of several issues:

* There are a lot of scanning artifacts that aren't part of the printed page. This includes dust specks, folds in the pages, and overlapping pages
* Inconsistent image layout due to these pages being scanned by many different people over many decades of archiving
* The number and size of columns changed over the course of publication

Some times there would be a vertical border between columns and other times there would be scarcely 1/8" (3mm) between the columns, so that the OCR could not reliably determine where the boundaries of the content were. Advertisements were typically enclosed within a border, but the columns of text tended to run together, especially in the earlier years of publication.

I read articles about how to improve OCR results. The most consistent pieces of advice were:

* Reduce the images to grayscale and reduce the number of shades of gray to 256
* Remove the content of the pages from the black borders surrounding the pages
* Properly align the images and remove any *skew* (distortion) introduced by the scanner not being perpendicular to the material (aka paralax)

The first issue was easily solved in bulk using Python scripts, requiring a few hours of processing time. The other two issues began a research project that could've easily turned into a Masters thesis, should I have wanted to do that again. The most promising avenue was to use OpenCV, a Computer Vision project that is widely used. 

## OpenCV

I hung out for several days on Slack, Discord, and Reddit groups devoted to the OpenCV project and its myriad of tools. I asked general questions and eventually more detailed questions about my specific use cases. Forums are great for gathering ideas, but everyone their has their own projects and none of them were similar to this one. Unfortunately, there were no turn-key solutions already built for newspapers. At least, none that the budget for this project would allow.

I spent several days experimenting with purpose-built tools to do object detection using [YOLO](https://www.darknetcv.ai/api/index.html) (You Only Look Once). This had promise, but the inconsistent nature of the scans made it difficult to handle more than a few weeks worth of issues at a time. Each batch of issues required me to adjust my code and determine what markers to base the image extraction on. This was leading into a deeper investment in time that I had to give, and I ended up abandoning these tools.

One thing I noticed when looking at OpenCV is there is a vast amount of plagiarism across blogs and sites discussing the project. One particular method with Python code for finding contours and using the results to identify the borders of an image was reproduced dozens of times across sites. Some of these sites were selling their code as original products, yet they didn't even change the names of their variables or the comments in the code. As much promise as CV has, it's clear there are frauds presenting themselves as experts.

In the end, I resorted to a manual process. I built automations in Photoshop to roughly determine the edges of the pages and crop them. This was a labor-intensive process that took over a week of evenings and a cold, rainy weekend while watching football games. During this process, I identified approximately 175 pages that were duplicated scans and removed them from the archives.

## Image Improvement

Now that I had nicely cropped images, I looked for ways to determine the rotation and skew of each page. Those old scans weren't perfectly aligned to the scanner and could be off by several degrees. This has a huge impact on the quality of the OCR process, so I used what I learned from the OpenCV investigations to build Python code to determine how much each image needed to be rotated with ImageMagick. 

As part of this, I realized the ImageMagick uses EXIF/JFIF image metadata rather than the image data itself when rotating images. I asked on a forum about how to correct this and I ended up in a long conversation with someone that is a maintainer of [exiftool](https://exiftool.org/). They said they had great experience using a tool called [ScanTailor](https://scantailor.org/) to re-orient and deskew images of printed pages.

ScanTailor required a bit of a learning curve. Its' history has seen it worked on and abandoned a few times, only to be taken up by someone needing additional functions it could provide. The UX is crude at best, but underneath, it has a great ability to operate on large numbers of images and do the right things. I spent a day running this tool on all 13,100 images and it did a wonderful job cleaning up the remaining borders, rotating the images properly, and removing any skew.

ScanTailor saves its output as TIFF images, which are 4x-5x larger than the JPEG images and uncompressed, which is fine for the purpose of this project. The PDFs from the historical society used compression, so there's no way to restore that lost image quality, but these are newspaper scans and not high-fidelity photos. Either JPEG or TIFF are fine to continue on.

## Back to OCR

At this point, with nicely cropped, rotated, and deskewed images, I started playing with tesseract and reading on how to improve the results. One particular value that I could manipulate is the 'page segmentation mode' which tells the tool to treat the page contents in different ways. I experimented with a number of different combinations of this value and none provided better results than `--psm 3`.

I set up a long-running Python script to run OCR against every image and store the results in a text file. This process ran for almost 14 hours on my 13th generation Intel i9 workstation. I gave the process as much of the CPU and RAM as it needed during this process.

## Restricting Search 

With a bulk of text to be indexed, I took a look at what the OCR process thought was on the page. Of course there were a lot of mistakes and words that didn't make sense. I went back to the original goals for the project and determined that it's best to think about what people would be searching for--names of people and places.

*What constitutes a name*? For places, I could rely on lists culled from Wikipedia. I extracted the name of countries, US States, city and county names in Oklahoma, cemeteries, post offices, and physical features, such as bodies of water, mountains, and valleys. All these went into a file that I would use later.

I found lists of surnames and given names that are common in the US. I primarily focused on surnames from Europe, Central America, and native tribes, although there are names from Asia and the Middle East in there as well. I want to give credit here to Rhett Butler for the work published at [probablyhelpful.com/](https://probablyhelpful.com) for putting it into easily used tables. All data is derived from David L. Word, Charles D. Coleman, Robert Nunziata and Robert Kominski (2008). *Demographic Aspects of Surnames from Census 2000*. U.S. Census Bureau.

In addition, I added a file containing place names and family names from this region that I knew from memory.

All of these words were combined into an *allow list* containing over 150,000 entries.

Conversely, I have a *block list*. I struggled briefly with this from an ethical perspective, but it wasn't a difficult decision to make. There are words that were used openly in the past that are now slurs. I did not redact the pages containing those words because they are part of the historical record, but I don't want them being searched for on a site that I've created. There is a text file containing these words in the repo for the tools to create the site and that repo is not public.

I ran an analysis on the words that Tesseract found and realized that anything longer than 17 characters was not a name. Usually, it was a concatenation of other words or scanning errors. Words less than 3 characters would increase the search index size without providing any helpful searches for the user, so I removed those as well.

# Building a Website

With the input data as clean as it can be, the next phase of the project was to build a website.

I ran a series of Python scripts on the input images to generate appropriate sized images for web viewing and to combine the individual page images into a single PDF for each issue. Toward the end of the project, I realized the images and PDFs only had basic EXIF information in them. I chose to add additional values, such as the GPS location and a link to the website. I also put in a title describing the image based on the publication date and the page numbers, where applicable. I asked the archivists that I met online whether they watermark or put other digital signatures on the images to prevent abuse and they say they did not, so I followed their lead.

As I mentioned earlier, I can't spend evenings and weekends making sure people aren't trying to attack the site. For that reason, I chose to forego traditional CMSes like WordPress, Joomla, and Drupal. Like my personal site(s), I want the attack surface to be as small as possible and the potential gain for the attackers to be zero. They might disable the site by flooding the domain with requests, but they won't add spurious files and malware. AWS CloudFront should mitigate this risk. Bots can scrape the content, but honestly I'd prefer their operators [just ask](https://thehydroreview.com/contact/) and I can provide them with a link to an archive.

All the links on the site are relative to the site itself. While a top-level domain exists, someone could take the site code and put it on their own server. In fact, that's exactly what I do when I go to local groups to show it off--I take my laptop and don't bother connecting to the Internet.

## Technical Stack

I chose to use [Jekyll](https://jekyllrb.com) for the publishing system. This is arguably the most well-known of the static site generators (SSGs) and Github itself provides support for it. There are hundreds of themes and tools made to work with Jekyll. I can host the site locally, on Github.io, as well as a cloud provider. There are CI/CD scripts to support Jekyll build and deploy models. I have experience running Jekyll sites on AWS and I like the fact that I can easily control my costs there.

The drawback (and benefit) of Jekyll and every other SSG is that everything about the site must be known in advance. There is a robust scripting language to build pages and link between them, but every new piece of content requires rebuilding the site. Given that The Hydro Review ceased publishing in the 1990's, this doesn't seem like that big of an impediment once it's all running.

Another slight drawback is that there isn't a lot of new development for Jekyll. What you can find today is probably all you will get in terms of turn-key tools and themes. If you need more than what is available, you'll have to build it yourself. I wouldn't build a new commercial product using Jekyll, but for this kind of personal interest/community project it works fine.

## Jekyll Themes

There was a somewhat thriving market in themes when I first started using Jekyll in 2012. I initially searched for themes that were meant for newspaper or literary archives. I found one, but it had not been maintained or updated in over 5 years. I decided to go with a well-maintained, general-purpose theme with good support for desktop and mobile browsers.

The theme I'm using is called [Minimal Mistakes](https://mademistakes.com/), which is widely used and is still supported by the author. It looks good on desktop and mobile devices. For the initial phase of this project, I chose to use this theme, but I didn't go to great lengths to customize it, hoping that eventually we'll have funding for a full-featured site with managed hosting.

Jekyll supports Javascript on pages. For most of the site, I used only what is included in the theme. However, the page that displays the scanned JPEG images of pages has JS that I wrote to handle keyboard input and moving between pages, similar to what you might see on an image-sharing site like Flickr or Instagram.

## Hosting

I used AWS because that's where my skills lie, having launched at least a dozen projects there. The only recurring expenses should be storage and bandwidth, aside from the annual renewal of the domain through Route 53.

The code for the site is in a public [Github](https://github.com/thehydroreview/thehydroreview-site) repo. The repo has an [automated CI/CD action](https://github.com/marketplace/actions/build-jekyll) to build and deploy the site to AWS whenever there is a check-in to the `main` branch. I chose to use a separate account for this project from my personal projects so that I could properly hand it over to the non-profit that is supporting the work. I gave my personal account the ability to submit code into the repo and manually initiate a build action, but in all other aspects I kept the accounts separated. It's just good housekeeping.

On AWS, the site is stored in one S3 bucket while the image and PDF data are stored in another. While developing the site, the changes have nothing to do with the image data, which was static once processed. Having the images in with the website code increased build times considerably. I assigned a subdomain to the image data to keep the two buckets separated. I could've used configuration settings in CloudFront to make the image data bucket seem to be /content of the site, but I chose to keep them separated for now. Perhaps in a future phase, I'll clean up that bit of technical debt.

CloudFront and Certificate Manager serve up the site with HTTPS without the cost of a recurring SSL certificate. I'm keeping an eye on the hosting costs through the AWS Billing Manager. While I have a `robots.txt` file to keep known crawlers out, I do want Google and others to index the site. AI tools are causing headaches for other projects because they don't honor robots.txt, so I put a fairly low cap in AWS on hosting costs. If this cap is exceeded, the site will shut down until the end of the billing cycle. I'll get an email from AWS with this information and we can re-assess how much the non-profit is willing to spend on hosting.

## Search Indexes

Having an on-site search function is crucial to help users find the information about the people they are interested in finding. Google search isn't sufficient for this task, although it will certainly be a component.

The Jekyll theme that I'm using has built-in integrations for a search engine called [Lunr](https://lunrjs.com/), a derivative of [Apache Solr](https://solr.apache.org/). This is a client-side only implementation that wasn't going to scale well for our purpose. I tried it out on a local build of this project and it did not give good results.

The other integration that's built into Minimal Mistakes uses the [Algolia](https://www.algolia.com/developers/code-exchange/integrate-jekyll-with-algolia) search engine. I've used Algolia enterprise search at a previous job and the results were good. I created a free tier account at Algolia in the name of this project and requested API keys. I'm going to pause for a bit and talk about how I architected the site navigation because it has bearing on how search is done. I'll get back to this topic a bit later, but for now I'll divulge that Algolia powers the search function on the site.

# Project Layout, Navigation, and Data Architecture

Jekyll is a publishing system, not a Content Management System (CMS). I'm not going to go into great detail on the differences in these because that's not germane to the goal of this document. Jekyll, like a CMS, allows us to create a interlinked site with nice visuals by following its requirements around file naming and placement. The task of organizing the content is on the site creator.

## Issue Pages

Each issue of the newspaper has a page devoted to its contents. At the top is a thumbnail image of the masthead that links to a PDF containing the full issue. This PDF has OCR ran on it using a tool called [OCRmyPDF](https://ocrmypdf.readthedocs.io/en/latest/), which also uses the Tesseract engine. I ran this tool on all the generated PDFs after they were built from the cropped and straightened images. When the link is clicked, the PDF opens in a new browser tab and a text search will work from within the browser. Again, this search is limited by the ability of the Tesseract engine to recognize text.

---
![](/assets/images/2025-12-17/issue-page.png)

---

Beneath the PDF masthead for each issue are thumbnails of the individual pages as reduced JPEG images, scaled down to 2560px in height at 70% quality by the build scripts. These images do not have OCR text embedded in them because that is not supported by the JPEG standard. The viewer page for images supports basic keyboard navigation using a bit of Javascript that I wrote.

Each page has a hidden element, containing the filtered words from the scanned images. Someone wanting to find a specific family name using the search function can land on an issue page by virtue of these words being on the HTML page, albeit hidden from casual view. The intent is that the visitor will click on the PDF and use the search function on that file to find the specific location on the page where the name exists. This isn't perfect, in fact, it's barely rudimentary, but it costs nothing to implement beyond the CPU cycles to create the files.

Each issue page has "Next" and "Previous" buttons to move between issues.

I wanted several ways to navigate by browsing, so the left-hand sidebar provides a list of issues by year. Also, there is a full listing of all 2,200+ issues accessible from the top menu named 'Issues'. The index of years shows how many issues exist for each year using the YAML metadata embedded in each issues' post.

## Blog Pages

Each issue exists as a separate blog entry, but the dates of the blog are the publication date of the issue itself. This allows us to write *spotlight* and *howto* [articles](https://thehydroreview.com/categories/#blogs) with recent dates showing an interesting aspect of a particular issue. For example, the [declaration of war in World War II](https://thehydroreview.com/blogs/spotlight-dev-7-1941/).

# Putting It All Together

Another script walks the list of images and PDF files and then generates an output folder that contains the Jekyll website pages. The website is in a separate directory from the image sources because the Jekyll theme and scaffolding for the website won't work with files that aren't the website itself. It's easier and less error-prone to generate the website pages based on the content of the folders that the script finds. This script also generates OpenGraph metadata for each page, so social media links will include information about the page.

With the Jekyll site files generated, I run the Jekyll build process on my workstation. This takes about 1 minute, starting from a clean state. If I'm only updating a few files, it takes only a few seconds until I can review it with `http://localhost:4000`.

Once I've tested the local instance, I check in the changes to Github. Github notices that I've checked something into the `main` branch and uses that to build the site. Assuming the site builds with no errors, it is sent to the AWS hosting via Github Actions. The last step is for the Github Action to invalidate the site cache in AWS CloudFront. This ensures that anyone viewing the site will see the latest updates and not stale content. From the time that I submit the changes to Github until I can see the results in a live browser is usually about 5 minutes.

To make the search engine work, I run another build of the website locally using a different set of parameters to activate the Algolia search plugin. This plugin reviews all the blog posts and issue pages, looking for text that isn't part of the page markup. Since I have the text for each page in the hidden <div> on each issue page, the Algolia plugin treats it as part of the text of the page. For the user, they won't see the text unless they deliberately expand the toggle arrow underneath all the page thumbnails. If they do, they'll see a bunch of words with a note indicating why they are there.

## Re-running the Process

When working with this much data, there will always be a need to go back and re-process a subset of the data.

For example, while showing off the site to the board of the non-profit, one of the members noticed there was an issue with 5 pages. "*Shouldn't a newspaper always have an even number of pages?*". I replied that it is indeed odd, but there were issues that were poorly scanned and didn't have the full set of pages. 

After the meeting, I searched for issues with an odd number of pages. Sure enough, about 25 issues (out of 2,262) had duplicated pages. When I wrote the OCR and conversion scripts, I deliberately added an option that allows me to limit processing to a single issue, month, or year. So, for those 25 issues, I re-ran the scripts with those specific dates and then rebuilt the website contents. Rather than taking 14+ hours, it took about 45 minutes once I had identified the duplicates.

# On the Web

With the site [on the web](https://thehydroreview.com), I spent time making sure that social media links to the site looked nice. The scripts I wrote generates appropriately-sized [OpenGraph](https://ogp.me/) images for each issue. At first, the images didn't work correctly and I had to rely on web tools to debug the process. I tried several and the least horrible was [opengraph.xyz](https://www.opengraph.xyz/). The [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug) was useful here. One thing I had to do was allow the Facebook web scraper to access the site, via putting an explicit `allow` statement into `robots.txt`.

I have the site connected to Google Analytics, but I'm not seeing the types of results that I want to see there, so I need to do more investigating why page views aren't triggering the analytics code on the pages. The non-profit doesn't care about the analytic data, per se, they will see the results in people sharing on social media. However, I want to know that the site is working correctly and Google Analytics is one of the tools. I'm not sure if my specific location is not triggering because I run privacy tools on my workstation and phone or if no devices are triggering the analytics. I need to let the site operate a while and do more research over the holidays.

# Lessons Learned

I'll be clear that the results of this project are a mixed bag. It was a fun project and I learned better ways to write and run Python. I also learned about the OpenCV project and how it might be applied to other work I might want to do in the future. I also wrote a bunch of really bad Python code that I ended up throwing away. That's part of the path to discovery, so it wasn't wasted.

These are a completely non-ordered list of the things that stick in my mind about what I've learned.

* I used git submodules for the first time in the website project. The Jekyll theme needs to be in the same folder with the web code, but it is tracked in its own repo because I want to continue to apply upstream changes. I would make changes to my fork of the theme and test them out on my workstation. When they were ready, I would check in the changes to the theme first, then check in changes to the website repo for everything to show up with the theme changes. That made updating the site much simpler.

* The RegEx search/replace functions in VSCode are extremely useful when handling large data sets. In the past, I would record a macro in Notepad++ and run it repeatedly on a large set of files. The VSCode implementation is nice in that it highlights a preview of every replacement. You can even apply a RegEx replace across an entire project, which is great [until it isn't](https://www.rexegg.com/regex-humor.php#twoprobs). Notepad++ is still great for its ability to copy/cut/paste in columns--I wish VSCode had this ability.

* The initial scripts used subprocess.Popen() to call Exiftool and ImageMagick tools. When I would run those scripts on all 13,000+ images, one particular script would take 3 hours to complete. I learned that there is a wrapper class for ImageMagick called [Wand](https://pypi.org/project/Wand/) and another for [Exiftool](https://pypi.org/project/PyExifTool/) that operate in the same process space as the calling tool. Further, if I instantiated the instance of those classes at launch rather than for each invokation, I gained even more time. After refactoring the code and using the wrapper classes, the process took about 20 minutes to process all those same files again. Clearly, external process calls are expensive in terms of CPU cycles. I similarly found a wrapper class for [OCRmyPDF](https://pypi.org/project/ocrmypdf/).

* StackOverflow was more useful for Python code questions than AI summaries from Google. I recognize that this could be my own bias, so I continue to look at the summaries.

* Similarly, the CoPilot code generator did not provide a working example the one time I tried to use it for something moderately complex. Copilot itself wants too much autonomy in VSCode and I eventually grew tired of its recommendations. To be clear, I absolutely rely on intelligent code highlighting, hints when filling out API calls, and static analysis in the editor. What I don't want while trying to solve a problem is a window popping up and offering unprompted suggestions.

* The things that I've mentioned in this post that made it into the final site are mentioned on the [Technical Details](https://thehydroreview.com/details/) page of the site. I am grateful for the work of those that went to the trouble to create and maintain those tools and I hope this document helps someone else that might be looking at a similar project. You are certainly welcome to contact me and I'll share what knowledge I have.

# Future Investigations

* There is a project called [Open ONI](https://open-oni.github.io/) that is connected to the Library of Congress' Chronicling America project. I've talked with the developers and archivists using this project and it has promise as a web platform. I've installed their code base under Docker on one of my spare machines, but it has a very steep learning curve.

* The [Oklahoma Historical Society](https://www.okhistory.org/) has their [Newspaper Gateway](https://gateway.okhistory.org/explore/collections/ODNP/) that is based on a project from the [University of North Texas](https://digital.library.unt.edu/). I'm meeting with the OHS archivist in January 2026 to see if we might be able to get our issues into their portal.

* The person I've been working with on this project is in possession of the archives (aka *morgue books*) for the newspaper. For those issues that the Oklahoma Historical Society doesn't have on microfilm, we might consider building a photography rig to digitize the pages ourselves. This would be a pretty significant step, but there are [homebrew rigs](https://www.instructables.com/Book-Scanner-Low-cost-easy-to-make-1000-pages-an-h/) online and I've bookmarked them for future consideration.

# Technical Debt

There's always things that could be done better and features that aren't fully implemented. The site is live and I'll make small changes as requested by the non-profit, but until we get new issues from the Oklahoma Historical Society or start digitizing the archive books, I won't be spending as much time on it. However, I am keeping these things in mind.

* **Naming of files** - The processing scripts rely on the files being named a certain way, namely **HR-YYYY-MM-DD-PP**. This isn't so bad, but it might be better to use a manifest to indicate what files should be processed.
* **Better exception handling** - I have basic exception handling in the scripts, but I have run into exceptions because files didn't exist where I expected them to and the scripts crashed.
* **Better search** - The site search isn't going to find "Bob Smith" as two words in order. It will find if Bob exists on the page and if Smith exists on the page. If both are there, it will return the result. But if Bob is on page 1 and Smith is on page 4, it's still going to return a match. One change I could easily make is to keep every word in the allow list in the search index for each page in the order it is found, even if the word is already found on the page. This is probably one of the first things I will investigate.
* **Monolithic python scripts** - Initially, I had 2 scripts that did a lot of work. I've refactored the processing so that I can restart specific parts as needed, but they still tend to be long sections of 100-200 lines of code without a lot of reuse. I can do better.
* **Python requirements** - I pulled in a lot of libraries into the Python Virtual Environment for these projects. I need to isolate them and put them as a single include in the tools repo documentation.
* **Manual intervention** - There are still times when I have to manually enter a command or move a file. This is better than it was, but always room for improvement.
* **Content and website the same domain/bucket** - Right now, all the images and PDFs are in content.thehydroreview.com and in a separate S3 bucket. This isn't a strong requirement, but I may move them in the future. However, the PDFs and images are publicly linked in the site, so I could break social media links if people have shared the PDF link rather than the page link.
* **Refine the dictionary** - Update the allowed list to include more surnames specifically from this area, common diseases and their historical names, and other relevant names.

# Final Thoughts

This project was a labor of love, obviously. I had a personal connection to the newspaper having worked there in the 1980s. The newspaper documented the lives of the early settlers, which included my own great-grandfather and his mother, who moved here in 1899. I never knew them, but I found mentions of them in these pages and it connects me to them.

Similarly, my wifes' family moved here prior to statehood and those family names show up frequently in the pages of the newspaper as well. We've shared at least a dozen snapshots of articles mentioning their ancestors with her mother and siblings, which has prompted them to question if the stories they always heard happened the way they were described. 

Finally, I was able to reconnect with someone I knew almost 40 years ago while studying for my Masters degree. His grandfather had an unfortunate connection to my town and his death was documented in the pages of the newspaper. Even though we haven't spoken in at least 20 years, I still had his phone number. I shared this history with him and he was grateful. We had a good chat and plan to meet again in the future. So, all-in-all it's been a good project.
