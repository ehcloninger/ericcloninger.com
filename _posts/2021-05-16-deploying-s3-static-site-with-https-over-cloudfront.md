---
categories :
  - blog
tags : 
  - howto
title: "Deploying a Secure (HTTPS) AWS S3 Static Site Over CloudFront"
toc: true
---

**TL;DR** - Don't use AWS Lambdas to try to fix what is a problem in the CloudFront settings. See the solution at the 
bottom of this page.

<!--more-->

There's a problem with AWS that has been a problem for years. It affects anyone that wants to deploy a secure static
site that's hosted on Amazon S3. Many people use Github Pages and that's great, but I already use AWS for my hosting
and I'd prefer to do new projects on AWS. If you try to search for solutions to this problem, you'll get steered
toward using Lambdas and that simply isn't necessary.

To get a S3 site to deploy with HTTPS, you have to create a certificate using the Amazon Credential Manager. I'm not
going to get into that because it's not germane to the problem and there are plenty of tutorials to help you do that.

To get HTTPS to deploy, you must use CloudFront and provide the certificate for your site. You can use AWS ACM
certificates and they work fine. After CloudFront deploys to its mirrors, you should be able to browse your site.

So long as you stay on the front page. If you click any link to pages within the site, you'll run into a CloudFront
404 error.

This is one of those subtle differences between hosting a static site on S3 with HTTP and hosting a static site on S3 
on the CloudFront distribution network with HTTPS. CloudFront doesn't treat HTML documents in sub-folders the same way 
that S3 does. With S3, the webserver can be configured to find index.html by default in all sub-folders. With 
CloudFront, only the top-most index.html is served by default. All the sub-folders will not serve up index.html by default.

One solution is to have your static site generate explicit index.html paths into every link. That's doable and I've
done it in the past. If you search for the solution using your favorite search engine, you'll likely come across the
"official" solution to use Lambda@Edge. You'll go down that rabbit hole for a few hours, scratching your head and
digging through the seemingly circular references in the AWS docs.

Then, you might discover, as I did, the real solution. I can't take credit for this, but I hope I can publicize it
and allow the real answer to the problem to propagate.

If you look at the [comments](https://commenting.awsblogs.com/embed.html?disqus_shortname=aws-compute&disqus_identifier=2895&disqus_title=Implementing+Default+Directory+Indexes+in+Amazon+S3-backed+Amazon+CloudFront+Origins+Using+Lambda%40Edge&disqus_url=https://aws.amazon.com/blogs/compute/implementing-default-directory-indexes-in-amazon-s3-backed-amazon-cloudfront-origins-using-lambdaedge/) on [this page](https://aws.amazon.com/blogs/compute/implementing-default-directory-indexes-in-amazon-s3-backed-amazon-cloudfront-origins-using-lambdaedge/), 
you'll find a comment by someone named 'ash1'. This is the correct answer...

> I found a much simpler solution that doesn't require a lambda. In your Cloud distribution, for the origin, instead of using the S3 bucket, use the S3 "endpoint url". That will automatically take care of the subdirectory default index.html issues. Changed all of my distributions, and they are all working now with subdirectory "/" endpoints going to the index.html files within the subdirectory.
> 
> 1. Go to S3, click on your bucket,
> 2. Click on properties tab.
> 3. Click on Static Website Hosting
> 4. At the top of the static website hosting page, it will show you the endpoint url for your S3 bucket. Use that url value for the CF distribution origin instead of the name of your S3 bucket.

## The Solution

To be clear on this, in the "Origin settings" page for your CloudFront distribution, change the "Origin Domain Name" from
something like **mysite.com.s3.amazonaws.com** to **mysite.com.s3-website-us-east-1.amazonaws.com**.

Once I made this change and allowed CloudFront to re-deploy my site, I was able to click on links to pages within the site.
