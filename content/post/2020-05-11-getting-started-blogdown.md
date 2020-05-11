---
title: Getting Started with R Blogdown on GitHub Pages
author: Alex Labram
date: '2020-05-11'
slug: getting-started-blogdown
categories:
  - R
tags:
  - R Markdown
  - R Blogdown
  - infrastructure
---

# Your mission, should you choose to accept it...

I recently stumbled back across an [essay](http://varianceexplained.org/r/start-blog/) that I remember caught my attention a couple years back.  Its premise: if you want to be a data scientist, start a blog.  It'll give you practice, help you build a portfolio, and attract constructive critical feedback.

As an aspiring data scientist, what else could I do but take this excellent advice?  Although I think I may be on the wrong Internet for that last point :-/

Now, when I start a project in a new domain, I'm generally pretty conservative about the toolchain and options I use.  I want to get a minimum viable product with as few new buzzwords as possible, and *then* I can pivot to using the sixteen different pieces of whizz-bang software that all the thought leaders are raving about.

This is a problem because, the last time I did any serious blogging, the typical choice was between LiveJournal or hand-coded HTML, and something called "Wordpress" was the new hotness.  My domain knowledge has not aged well.  It's even *more* of a problem than it would usually be because, for various lockdown-related reasons, I'm attempting to blog from my work laptop, which won't let me install arbitrary software.  No Wordpress for me :(

# Blogdown + GitHub Pages = Win?  Maybe?

Fortunately, I have R/RStudio and Git installed on my work laptop, with the necessary permissions to install R packages via the former.  This leads me to the natural choice of using [GitHub Pages](https://pages.github.com/) to host my site, and [R Blogdown](https://github.com/rstudio/blogdown) as my static content generator.

R Blogdown is part of the same series of [literate programming](https://en.wikipedia.org/wiki/Literate_programming) packages as [R Markdown](https://rmarkdown.rstudio.com/) and [R Bookdown](https://bookdown.org/).  They're all produced by the same person, [Yi Hui](https://yihui.org/), who is up there with Hadley Wickham in the R pantheon.  Literate programming, which I only stumbled across recently via the JHU Data Science [specialisation](https://www.coursera.org/specializations/jhu-data-science) on Coursera, is an alternative programming paradigm for producing truly self-documenting analysis.  It works incredibly well in a data science context, and R Markdown in particular is currently my go-to tool for any shallow analysis.

Less fortunately, R Blogdown + GitHub Pages is not *entirely* a supported configuration.  The [Blogdown manual](https://bookdown.org/yihui/blogdown/github-pages.html) quite heavily steers you towards an apparently-more-sophisticated alternative called [Netlify](https://www.netlify.com/).  Which I've never heard of before, and don't have the first clue how to take full advantage of, so have promptly [filed](https://en.wikipedia.org/wiki/File_13) under Not This Time Buddy.

# R-ing TFM

Being a typical techie, my first attempt was without any reference to the written documentation.  I ran `install.packages("blogdown")` and, after one scary moment when the progress bar went up to ~4000%, it installed perfectly.  I then created a new repository on GitHub called "AlexLabram.github.io", cloned it locally via R Studio's New Project / Version Control dialog option [^1], and ran `blogdown::new_site()` to populate it.  Committed changes, pushed to GitHub, checked the [site link](https://alexlabram.github.io) and... [that's not a website](https://www.youtube.com/watch?v=8Nho44lGVV8), that's a README file.

The key problem, as [outlined](https://bookdown.org/yihui/blogdown/github-pages.html) in the Blogdown manual, is that GitHub Pages expects the site index to be in the root folder, whereas Blogdown expects it to be in the /public subdirectory.  According to the *GitHub Pages* [manual](https://help.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site), it should be possible to change this... but, where there's supposed to be a dropdown in the GitHub settings page, I just see the message "User pages must be built from the master branch."  Helpful.

Plan B, after reading the Blogdown help page, is to split my work into two repositories.  The first, imaginatively named "blog_source", contains the Blogdown R project, as produced via New Project / New Directory / Website Using Blogdown.  The second, named "AlexLabram.github.io", contains the actual static site.  A Blogdown configuration option, `publishDir = "../AlexLabram.github.io"`, is used to point the one at the other.

There are a couple of subtle annoyances here.  Firstly - and this may well be unique to me - I had great fun convincing RStudio that I wanted to create a Blogdown site, apply version control *and* link it to an existing (empty) GitHub repository.  This is the first and only time in my life I've genuinely needed to drop down into the Git shell, in order to run `git remote add origin https://github.com/AlexLabram/blog_source`.

Secondly, for Reasons&trade;, GitHub Pages expects a site generated by the tool Jekyll, to which it needs to apply various post-processing.  To *stop* this happening, you have to include a special indicator file called ".nojekyll".  Which in Windows is impossible to create via the GUI (thanks, Bill).  Running `file.create(".nojekyll")` in R works fine though.

After fixing those issues up, I pushed the static site repository to GitHub, fired up my web-browser, and... nope.  The [GitHub repository](https://github.com/AlexLabram/AlexLabram.github.io) is full of relevant files, but attempting to *view* the site just gets me a 404 error.

Back to the drawing board. 

# The Great Bug Hunt

On investigation, I found that specifying the webpage in my URL meant everything Just Worked - it was only when I dropped the "/index.html" suffix that things fell apart.  That at least meant the issues was a GitHub Pages problem rather than a Blogdown problem.

A bit of digging brought me to [this thread](https://github.community/t5/GitHub-Pages/index-html-not-working/td-p/1266) in GitHub's help forums, wherein people have identified about a dozen different potential causes for precisely the problem I'm seeing.

Some of the suggested solutions would require invasive changes to my Blogdown installation.  In the spirit of the [drunken man and the streetlight](https://en.wikipedia.org/wiki/Streetlight_effect), I first tried [an option](https://github.community/t5/GitHub-Pages/index-html-not-working/m-p/42858/highlight/true#M2473) that seemed fairly easy to implement: adding an empty config file.  Worth a try, but no obvious reason why it would work...

...so of course it worked perfectly.  What.

# Kicking the tyres

After getting the demo site up and running, there were of course a few things I needed to test.  Most importantly:

1. Could I regenerate the site without messing up the bugfix?
2. Could I create blog posts within the site and publish them?
3. Could I [wire it up](https://help.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site) to my existing personal domain (which has been without website for about a decade)?
4. Could I produce interesting and engaging content?

I'm pleased to confirm that #1 seems to be working OK (breath: held).  I'm going to leave #3 for a later date - had enough technical turmoil for one evening.  And #4 is *clearly* a pipe dream :-P

Item #2, creating a new page, is a bit more convoluted.  This is largely because Bookdown is built on top of another static site generator called Hugo - it's basically a wrapper that adds R Markdown support.  This means that you have to know at least a little about Hugo.

In particular:

* Content goes in the /content folder (quelle surprise).  You can apparently lay this out however you damn well please.  Hugo will then rifle through its pockets looking for loose .md files.
* There's a special markup language (key/value store) for configuration options.  The admissible keys are different for each Hugo theme.  Be afraid.
* The Hugo documentation is somewhat terrifying.  Take note of Yi Hui's [comment](https://bookdown.org/yihui/blogdown/a-quick-example.html#fn3) on the subject, and then recall that he is almost certainly smarter than you.  Abandon all hope, ye who RTFM here.

Speaking personally, I stumbled across the following minor issues in creating a blog post (this one!) from an [R Notebook](https://github.com/AlexLabram/blog_source/blob/master/content/post/2020-05-11-getting-started-with-blogdown.Rmd).

* R Blogdown's site preview tool doesn't like the whole "split across two repositories" concept.  At all.  After some coaxing (i.e. manually locating the index.html file and opening it in Chrome), it would display the site without any formatting or images.  Meh, good enough for checking.
* The "date" parameter in a Notebook's YAML header is [load-bearing](https://www.youtube.com/watch?v=QRVExJZKIT8).  I initially tried just specifying it as "2020-05-11"; it filed the blog post under date 0001-01-01.  A subsequent attempt using the full UTC date string ("2020-05-11T20:26:00+00:00") caused the post to be entirely ignored for rendering purposes, which I ultimately traced down to the year value but still can't explain.  Eventually I gave up and used RStudio's "New Post" tool (in the Addins menu on the toolbar).
* The New Post tool doesn't add a ".md" suffix to a Markdown file unless you tell it to.  It's still better than creating pages manually, though.
* The site builder tool doesn't delete previously-generated content, even if you've deleted the corresponding .md or .Rmd file.  You have to delete the contents of the static content folder before rebuilding the site.
* Said purge should not extend to the .gitkeep, .nojekyll and _config.yml files, nor to the .git subfolder.  D'oh.
* Attempting to commit the static site files in Git gets me the following error: `warning: LF will be replaced by CRLF in 2020/05/11/getting-started-blogdown/index.html. The file will have its original line endings in your working directory`.  I'm chalking this up to Windows being Windows, and trying not to breathe too heavily near it in case it falls over.
* The markdown used in .md files (processed by Hugo) is subtly different in places from the markdown used in .Rmd files (digested by knitr).  For example, Hugo's syntax apparently doesn't permit the use of ^superscript^ marks.

Having ploughed through all those issues, I *finally* seem to have a working site.  Now I just need to customise the site's appearance and metadata.  Joy.

# Lessons learned

* Setting up new infrastructure is never *ever* easy, no matter how straightforward it's alleged to be.  Budget your time, [SAN points](https://tvtropes.org/pmwiki/pmwiki.php/Main/SanityMeter) and alcohol accordingly.
* Learning how to use the Git shell is probably worth the effort, even if your IDE includes a pretty front-end that provides *most* of the same functionality.
* Just because a thousand other people are using a content management system, doesn't mean it won't have really dumb issues.

[^1]: I've so far managed to avoid getting to grips with Git's command line.  Not sure how far I'll be able to stick to that policy, but I'm holding out as long as I can.