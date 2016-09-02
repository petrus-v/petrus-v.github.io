---
layout:     post
title:      "How do I made this blog?"
subtitle:   "Jekyll and github's pages pretty well integrated"
date:       "2016-06-25"
author:     "Pierre Verkest"
header-img: "img/bg-jekyll.png"
---

disclaimer:

Here, I won't explain how to setup your blog using jekyll and *GitHub*'s
pages, only give the big picture how it works and give some resource
that was useful for me!

This blog is hosted on [github pages](https://pages.github.com) that
service allows to host a website for *GitHub*'s projects.

[Github](https://github.com) is a web-based Git repository hosting
service and aims developers to collaborate efficiently to build
software.

[Jekyll](https://jekyllrb.com/) allow to write blog post in
[Markdown](https://daringfireball.net/projects/markdown/) format
and generate this static web site.

> **Note**: There are other formats supported by *Jekyll* I'm happy with
> *Markdown* format as I use it a lot around my *GitHub* projects.

So adding a new post is basically create a new *Markdown* file in my
[blog repository](
https://github.com/petrus-v/petrus-v.github.io/tree/master/_posts)

*Jekyll* is [integrated with Github pages](
https://help.github.com/articles/
using-jekyll-as-a-static-site-generator-with-github-pages/) so as soon
I push to my repo *GitHub Pages* service build my blog and publish it!

To setup every things that can take some times, in fact (as often) the
most consuming time was to know what I wanted to do! As suggest in
[Barry Clark's post](https://www.smashingmagazine.com/2014/08/
build-blog-jekyll-github-pages/) I have started from an existing project
which was hard to choose. After trying a lot of websites I got this
short list:

* [Kun Jia's blog](http://www.jack003.com) found on [jekyllthemes.org](
  http://jekyllthemes.org/)

* [clean blog template](
  http://blackrockdigital.github.io/startbootstrap-clean-blog-jekyll/)
  found on [jekyllthemes.io](https://jekyllthemes.io)

* [Monica Dinculescu's blog](http://meowni.ca) I knew that one before.

After a long hesitation between those 3 templates I've chosen the 2nd
one as it fits your device (thanks bootstrap and its responsive design)
and still quite simple and basic.

By chance to illustrate this blog I've found [Pixabay](
https://pixabay.com) with a pretty good library and as a lot of people
use Google image filtering by licences.

You can do a lot of thing that a [CMS](
https://en.wikipedia.org/wiki/Content_management_system) can do like
let user comments under post using service like [Disqus](
https://disqus.com/), follow activity on [Google Analytic](
https://www.google.com/intl/fr_fr/analytics/) and much more...

The hardest things is when you receive build errors email from *GitHub*:

```
The page build failed with the following error:

There was a YAML syntax error on line 7 column 1 in
`<unknown>`: `could not find expected ':' while scanning a simple key`.
For more information, see https://help.github.com/articles/
page-build-failed-invalid-yaml-in-data-file.

For information on troubleshooting Jekyll see:

  https://help.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can contact us by replying to this email.
```

Some informations are missing to let you know what to fix exactly. If you
just add a file that will probably fine but if your are setting up your
website you have to guess which key and file needs to be fixed.

So as I'm not a ruby developer I've chosen to use docker container
to build my blog and find out what's wrong

To build the blog locally from the blog root directory I've used
[Graham christensen docker image](
https://github.com/grahamc/docker-jekyll) (I guess jekyll/jekyll image
would do something similar I haven't tried):

```bash
docker run -it --rm -v "$PWD:/src" grahamc/jekyll build
```

to serve it locally:

```bash
docker run -it --rm -v "$PWD:/src" -p 4000:4000 \
            grahamc/jekyll serve -H 0.0.0.0
```

All of this takes me a short day to decide, setup, update an existing
template and start to write some posts!
