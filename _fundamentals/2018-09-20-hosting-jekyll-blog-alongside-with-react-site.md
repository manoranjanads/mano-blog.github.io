---
title: "Hosting Jekyll Website Alongside with React App on AWS"
permalink: /fundamentals/hosting-jekyll-website-alongside-with-react-app.html
excerpt: "Hosting Jekyll Website Alongside with React App on AWS."
header:
  overlay_image: /assets/images/fundamentals/2018-09-20-hosting-jekyll-blog-alongside-with-react-site/jekyll_react_aws.png
  teaser: /assets/images/fundamentals/2018-09-20-hosting-jekyll-blog-alongside-with-react-site/jekyll_react_aws.png
  overlay_filter: 0.5
last_modified_at: 2018-09-20T15:59:07-04:00
toc: true
author: Mano
---

## Why we love static sites

Simply, it far more easier to for a developer to write in `MarkDown` rather than using formatting in a text editor.
Also, it's easier to write on your favorite text editor or on ide and `git commit`/`git push` than, logging in to a remote dashboard and doing all the writing.
And, we do not have to worry about servers and databases at all.

## Why AWS

Simple answer is to avoid server management.
if we put our static content in an S3 bucket with `Cloudfront` configured, 
everything works perfectly and we do not have to worry about servers, system updates and errors.
And, it's cheapest way of hosting a static site.

## Gitlab and Continuous Integration
Without building the static site everytime we do a change, 
it's better to automate it. GitLab provides a build environment with docker container.
Therefore with `Gitlab CI/Cd` we can built the content and deploy with a single `git push`, we can have everything built and deployed 

## Why the blog in a Sub URL

Why we prefer `meetrix.io/blog` rather than `blog.meetrix.io` ?
Answer is simple `Search Engine Optimization".


## The React Caching issue

Now, here is the issue. Our landing page and other components are developed with React.
And for the blog, we use `Jekyll`.
The react app will not consider the content `/blog` inside directory as a part of the application.
React production build is optimized for local browser cache. Therefor, when you visit yoursite.com/blog after visiting yoursite.com, it would render an error page or a messy page.
Solution for this is unregistering the service worker.
In your index.js of the react app, use this

```js
import { unregister as unregisterServiceWorker } from './registerServiceWorker'

unregisterServiceWorker();
```

instead of 

```js
import registerServiceWorker from './registerServiceWorker'

registerServiceWorker();
```


