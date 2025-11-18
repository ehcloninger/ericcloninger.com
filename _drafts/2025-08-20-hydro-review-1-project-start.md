---
title: "Kicking off My Newspaper Project"
excerpt_separator: "<!--more-->"
toc: true
categories:
  - blogs
tags: 
  - howto
---

This is the first of a series of blog posts about a technical project that I undertook during my down time.

<!--more-->

As a high school Junior classman, I started working for our home-town newspaper, The Hydro Review. Able-bodied and with the enthusiasm of a teenager, I ran about the office lifting boxes, composing headlines, developing film, printing photos, laying out pages, and stuffing mailers with each weeks' issue. I eventually parlayed some of my earnings into the purchase of the owners' Pentax K1000 camera and a 50mm lens, which I still own and still works, but that's a different story.

Eventually, I left that job when I went to college. One of my former colleagues and her husband moved to a nearby town and set up their own weekly newspaper. I helped them set up the darkroom and showed her husband how to mix and use the chemicals, but it was too far from campus to work there.

Time moved on and eventually the Hydro Review shut down as a lot of small town newspapers did. About the time the Internet arrived to disrupt entire classes of jobs, the final owners decided they couldn't keep it running, even as a retirement gig. 

The newspaper operated by my former coworker and her husband continued up to her death in 2024. I saw her husband a few weeks ago at a community event and we talked about the newspapers and a project that I did for our local school a few years prior. In the course of this conversation, he mentioned that he had a flash drive of the first 43 years of the Hydro Review that he would share with me. 

# Content

The content for this project came from the Oklahoma Historical Society. They provided a flash drive containing 13 PDF files containing scanned images of the Hydro Review from July 1904 thru December 1947.

Each PDF contained 2 or 3 years of issues, with roughly 1,200 pages in each PDF file. There are a few missing issues that are usually indicated by a single blank page with the words "MISSING FILES" in large letters.

The PDFs have had an OCR process run on them and there is some text in the files, but it's not well aligned to the columnar nature of the newspapers of the time. But, it's a start.

# Big Picture

What I wanted was a site that would allow easy browsing and searching of the content. I imagine that most people finding the site don't want to just peruse the archive issue by issue. They probably want to search for a name or find a specific date. I wanted to make those user journeys as easy as possible, given several constraints...

1) I didn't want to spend much money on the project. I registered a domain name where this content will reside and that comes to $12/year on AWS. There will be hosting costs when it goes live and that will have to be figured out at some point. I suspect the final footprint of the site will be around 100GB, but it shouldn't have a lot of traffic.

2) For the handling of the data and converting files, I wanted to rely on Free and Open Source Software (FOSS) to the largest extent that I can. When I couldn't use FOSS, at least I didn't want to use Acrobat or any of the Adobe suite of products. Postscript and PDF are original Adobe products, but there are FOSS and proprietary alternatives that don't require surrendering your computer to their licenses.

3) I don't want to be a system administrator. I won't waste time fighting off people trying to get into the administration panel or inserting malicious code into the pages. For this reason, I am only going to allow myself to use a static site to present the content. That does have its challenges, which will be addressed later.

4) The project must be automatable as possible. While the project won't scale in the sense that I'll be adding content years from now, there are still thousands of pages to be processed. I need to rely on command line tools to the largest extent possible so that I can write scripts to manage them. While it's not yet a turn-key process, I can't spend months hand-editing files.

To underscore this last point, once I'm at the website building process, everything will use automated build and deployment steps, like I would use on a commercial project using devOps.

There is going to limit the scope of the project because the source data has some serious issues. There are a lot of misspellings, the pages are torn, many pages have pencil markings on them, and the original scans are of dubious quality. Trying to search for an entire phrase and getting to the specific spot on the page is well beyond the scope of what I'm trying to do here. I'm hoping it's sufficient that someone looking for a family member can search by their name and get some results.

# The Process

The files are a big collection of PDF pages. The first thing to do is to separate them all into individual pages and figure out how to map those into issues by date. 

## Separating the Files into Pages

The first part is simple enough. There are a set of tools on Linux to handle PDF files. These tools are collectively called ["poppler"](https://poppler.freedesktop.org/).

I used `pdfseparate` to save all the files using the original file name as a base and a sequential number for each separate page. This resulted in 13,224 individual PDF files, most of which were right at 1MB in size. There was a small glitch in the software that about every 500 pages, I would end up with a file that was only 1 page but was the size of the entire PDF archive. Fortunately, when I created a script to just convert these specific pages, they were saved with an expected file size.

## Creating an Index of Pages

Now, I need to figure out how to get those 13,224 sequentially numbered files into file names that reflected their content. The first stab at this resulted in 3 1/2 years of issues which required an entire evening of copying and pasting in Excel. I track the year of the issue, volume number, edition numbers, month, day, and file name, as below:

```
Year    Volume    Edition    Month    Day    Page    File                            Notes
1904    3         35         7        15     1       1904-07-15-1905-06-23-001.pdf    
1904    3         35         7        15     2       1904-07-15-1905-06-23-002.pdf    
1904    3         35         7        15     3       1904-07-15-1905-06-23-003.pdf    
1904    3         35         7        15     4       1904-07-15-1905-06-23-004.pdf    
```

The file names came from doing a directory listing and just pasting the output into Excel. The years and volumes are easily copied for hundreds of rows. The edition, month, and day are the same for each issue, while the page has to be entered for each file. I'm a pretty fair hand at editing large amounts of data with Excel, but this is still a lot of work.

One thing that helped this process is that I'm using a desktop PC and 2 monitors. I created thumbails of the pages and put my image viewer on the second monitor. This helps me see quickly how many pages each issue has. Most of the early issues had 6 pages, that eventually grew to 8 pages. However, there would be special issues that had a sales insert or perhaps legal notices from the county. So, I couldn't rely on dozens of issues having the same number of pages. To make it slightly more complicated, probably 1 issue each year was missing entirely or had pages that were missing or damaged in the archives. Each issue needed to have the number of pages visually verified by looking at the thumbnails.

After an evening of copying and pasting, I made it from July 1904 to December 1907. That's when I decided to start on another part of the project and give my hands a rest.

## Creating a Search Index

Now comes the time for some big thinking.

The PDFs from the Oklahoma Historical Society had some rudimentary Optical Character Recognition (OCR) done on them. The PDFs had some words in them that were pulled off the page, but it was mostly line by line across the entirety of the page. The early editions all had 8 to 10 columns of text on the pages, so the results had little beyond words. I was looking for sentences or at least snippets of sentences.

## OCR

I tried a variety of Open Source tools to do OCR. I initially started with a tool called [PDF24](https://tools.pdf24.org/en/), a proprietary product that's free for personal use. PDF24 has a website where you can upload a single PDF or a group of them and perform some magic. While I could do this, I didn't want to babysit this process for 13,224 files.

Fortunately, PDF24 offers a downloadable version of their software so that I could save bandwidth. Unfortunately, their GUI version starts having errors after you've selected more than 20 files at a time. Again, not wanting to babysit my tools for days, I discovered that I can create a batch file to call the command line version of their program. I fed this the list of files and let it go.

`"c:\Program Files\PDF24\pdf24-Ocr.exe" -outputFile <output> -removeExistingText -deskew -applyProfile High <input>`

We had a dinner party that evening and I went to bed when we got home. The next morning, I discovered the OCR process had managed to do only about 1,500 files. 

At that point, I decided it would be worth the investment to learn how to use the open-source [Tesseract](https://tesseract-ocr.github.io/) software to do the OCR. This tool is completely command line driven and offers a large number of options.

I built a set of bash scripts to automate the task and let it run that day. It took about 8 hours to process all of the 13,224 images.

## Text Extraction

Now that I have files with a reasonable OCR job, I want to figure out just how many words are in them. To get the text out of the OCR'ed files, I used the poppler utilities again. For this, the tool was called `pdftotext`. I fed it the name of each PDF file and it returned an output file with all the text that it found. It wasn't a great result, to be honest. The OCR process was better than what was originally in the files, but it still doesn't follow the articles around the page as well as could be hoped. But, it's a start.

`pdftotxt <input> <output>`

I thought it would be useful to have an index of words that exist in each page. I think there are some easily integrated tools that can do some keyword-based searches on static text sites. So, I took the text files containing the gibberish sentences and generated files containing one unique word per line, sorted alphabetically with no regards to case. The one caveat was that I didn't want any 1 and 2 letter words because those really aren't very useful for searching and will drastically increase the size of the search index.

For this, I used this arcane command on my Linux environment...

``` bash
tr -sc '[:alpha:]' '[\n*]' < input.txt | tr -dc '\0-\177' | tr '[:upper:]' '[:lower:]' | sort -uf | awk 'length < 3 { next } { print }' > output.txt
```

As before, I feel like I need to pivot a ways before committing a large amount of effort into something that might not work as I expect. So, with 3 1/2 years of content, I started on figuring out what the website would look like and how it would operate.

For now, I'll pause and start [another post](/blogs/2025-08-25-website-ideas) for the website once I've got something working...
