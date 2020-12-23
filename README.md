# Mano's Blog

## Categories and Directories

There are six categories in the blog.
All the posts should be crated in the respective directory as described bellow.

1. AWS : `_aws`
2. Genaral : `_general`
3. Products : `_products`
4. Services : `_services`
5. WebRTC : `_webrtc`
6. XMPP: `_xmpp`

## How to write a post

1. Select a title. eg `Best Practices in WebRTC`. Make sure the title is unique.
2. For each post, a separate file should be created under parent directory.
3. Format of the file name should be `<INDEX>-<POST-TITLE_SEPERATED-WITH-DASHES>.md`.
eg: `10-best-practices-in-webrtc.md`
1. For each post, a separate assets directory should be created under `assets/images/<CATEGORY-NAME>/`.
Directory name should be the same as the post file name, except `.md` extention. 

eg: `assets/images/weight-loss/110-best-practices-in-webrtc`
1. Image name also should be in ``<INDEX>-<IMAGE-NAME-SEPERATED-WITH-DASHES>` format

eg: `01-webrtc-mesh.png`

### Sample Header

```ruby
---
title: "How to Create an OpenCV Filter for Kurento Media Server"
permalink: /weight-loss/10-best-food-practices.html
excerpt: "WebRTC based Machine Vision"
header:
  overlay_image: /assets/images/webrtc/01-introduction/introduction.png
  teaser: /assets/images/weight-loss/10-best-food-practices/01-breakfast-image.png
  overlay_filter: 0.5
last_modified_at: 2020-12-23T14:08:57-05:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---

```

#### Title

Should use Title Case. All the appropriate letters should be Capitalized. Take a look at the above example.

#### Permalink

This is the link that the users use to visit the site. This should be `unique` for the article.
use the same name as article file name with `.html` extension.

eg: `best`

#### Navigation

Add the article appropreately in `_data/navigation.yml`

### Development run

### To Setup Jekyll development environment.

Follow the link  ->[Jekyll instalation guide](https://jekyllrb.com/docs/installation/)

### To install plugin for make images responsive.

Follow the link -> ['jekyll-responsive-image' plugin instalation guide](https://www.ratanparai.com/jekyll/Responsive-image-on-jekyll/)

## Development

## Install Dependencies

`bundle install`

if there are erros, please take a look at the errors carefully. Knowns issues

Nokogiri might give an erro if the necessary libraries are missing. Need to install them prior to run the `bundle` command.

In Ubuntu: `sudo apt-get install zlib1g-dev libmagick++-dev`
OSX : `brew install imagemagick`

In osx you need `brew install pkg-config`. Install imagemagick v 6 `brew install imagemagick@6 && brew link imagemagick@6 --force`

### Blog

run `bundle exec jekyll serve` that will open up the dev server at `localhost:4000`

### Landing Page

run `gulp dev` inside `landingpage` directory this will open up the dev server at `localhost:3000`

## Production

run `JEKYLL_ENV=production bundle exec jekyll build` in parent dir and then `gulp` inside landingpage

## Indexing

export ALGOLIA_API_KEY =<your_admin_api_key> && bundle exec jekyll algolia

## Upload to S3

`aws s3 cp  --recursive ./_site/ s3://meetrix.io`

## Theme

[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)