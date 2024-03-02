---
title: "Goodbye, Netlify"
excerpt_separator: "<!--more-->"
categories:
  - blog
tags: 
  - web
---

In the tech world, the saying goes "*If the service is free, you are the product*".

<!--more-->

Or, as our parents' generation might've said "TANSTAAFL" (*There Ain't No Such Thing As A Free Lunch*).

A few years ago, I switched my hosting option for this site from Amazon AWS to a service called Netlify.
I didn't mind the few dollars per month it took to host the site on Amazon, but the process of getting
my updates posted required a number of manual steps, that if done incorrectly, would render the site
unreachable.

After screwing up a few times, I realized I needed to spend some time researching and implementing a
modern, automated workflow. That's when someone on the Jekyll forums mentioned Netlify. Netlify wraps up
all the automation in a smooth package for the low, low price of nothing for non-profits and vanity
projects. 

Netlify made their money getting nerds like me to use it for our personal projects and then hoping
we'd recommend it to our bosses when we need such a service. No harm in that, companies do it all
the time with students and educators.

I was happy using Netlify and I really have nothing bad to say about the service as I used it.

## However

This week a rather disturbing post came across [Reddit](https://old.reddit.com/r/webdev/comments/1b14bty/netlify_just_sent_me_a_104k_bill_for_a_simple/). A free-tier user with a hobby 
site, like mine, was sent an invoice for $104,000 because of a traffic spike that sent hundreds of
terrabytes of requests to his site. The user, who is a web developer, said the requests were all
after a copy of a 20-year-old song by a Cantonese singer. Hmmm.

The developer contacted the company about the invoice and they offered to reduce the charge to 
$21,000 and after some pushback, they offered "only" $5,000. There is no argument that there
was a traffic spike, although there is some speculation about why this happened at this time.
The CEO came onto HackerNews [and said](https://news.ycombinator.com/item?id=39520776) they've 
cancelled the invoice and they do that occasionally when this specific problem arises. So, they've 
known this can happen, but did nothing to prevent it.

The problem is that there are currently no controls on Netlify to prevent these kinds of traffic 
spikes that can disable a website and lead to crazy invoices. Amazon AWS, on the other hand, has 
fine grained controls over such activity. The account I used when I previously had this site on AWS
was configured to send me an email if my monthly bill ran over $30 and to stop all activity if
it hit $40. 

That was the driver behind me moving back to Amazon AWS. Now, to spend a few hours at the keyboard.

## Getting There

Here I am on a Friday night, trying to remember all the steps to getting a static site running on AWS
with SSL enabled. I've done this a dozen times, but it's been a few years. When it was all done, I'd 
spent about 4 hours going through all these steps several times for several domains:

* Transferring the domain ownership to the AWS account for my hosting for 3 domains. I could've left them with the existing domain hosting account, but I prefer to have it all in one place with the content.
* Getting all the Route53 records pointing to the the same DNS servers as the zone records.
* Requesting ACM certificates so that I could present my site with HTTPS. This took WAY too long for my liking, but fortunately I had other steps I could work on while I waited.
* Creating S3 buckets with the right permissions for static website hosting without leaving them wide open.
* Creating roles and users that can access the S3 bucket from Github Actions
* Creating a CloudFront distribution to serve up the content from S3 worldwide with HTTPS.
* Getting the Github Actions to push my updated website to AWS. This took about 45 minutes for this domain and 10 for the second. I decided not to publish the third domain just yet and I have a subdomain that I'm not sure I want to put online any longer.
* Shutting down my Netlify account

If you're interested, here's a [good summary](https://pagertree.com/blog/jekyll-site-to-aws-s3-using-github-actions) of the Github Actions setup. 
There was one change I had to make in AWS that is mentioned in [this StackOverflow comment](https://stackoverflow.com/a/36272287/296758) 
regarding ACLs. In addition to that change, there is a setting in S3 buckets to re-enable ACL access to the bucket. 
I don't know why AWS has all these settings with similar purposes, but all I can do is write it down here
for the next person to discover. Good luck.

## Welcome Back

If you're reading this, it means that I got everything moved over to AWS and configured correctly.
I'll go back to paying $4/month knowing that I won't ever get a $104,000 invoice.