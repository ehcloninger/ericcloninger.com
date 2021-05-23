---
categories :
  - blog
tags : 
  - web
title: "Hooray for Netlify"
---

This is a quick post to test a site change that will hopefully make life much easier on me in maintaining
the site. In the past, I played with other "auto deploy" tools to take this site from GitHub, through the
build process, and finally to my website without having to push extra buttons. The rationale for this is
that I want to be able to add blog posts under Jekyll as easily as it is with database-driven CMSes with
web front-ends. The other tools always had complications that made me not interested in using them for my
personal site, much less a commercial project.

This new deploy method uses [Netlify](https://netlify.com), which receives notifications from GitHub when
a repository has changed. On change, it pulls a clone of the repo, builds it, and if successful, it 
deploys it to their servers. For my use, it's free, but they have paid tiers for commercial use.

I'm editing this document with the GitHub editor, which is rudimentary, but basic. It's not as useful as
a full-featured editor, like VS Code or Atom, but for a quick note it's just fine. If you see this post
with a lot of extra edits below, then you'll know it worked...

---

**5 minutes later...**

It worked on the first try. The real test will be when I can do this using my Samsung S6 tablet
sitting in [my 72 MGB](https://my72mgb.com). A quick shout out to 
[Michael Currin](https://talk.jekyllrb.com/t/how-to-specify-a-theme-in-config-yml-thats-on-local-file-system/6059/7) 
on the Jekyll Talk forums for convincing me to move into the modern era.
