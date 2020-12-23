---
title: "Key elements of responsive web design"
permalink: /general/key-elements-of-responsive-web-design.html
excerpt: "Mobile responsive web design"
header:
  overlay_image: /assets/images/general/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/coverpic.jpg
  teaser: /assets/images/general/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/css.png
  overlay_filter: 0.4
last_modified_at: 2019-10-15T11:00:07-04:00
toc: true
author: Kaveesha
# categories:
#   - css
#   - html
# tags:
#   - html
#   - css
#   - media query
#   - mobile responsive
---

Responsive web design is an approach that allows design and code to respond to the size of a device’s screen. That means the web site should be very clear and fit to all screen sizes. Nowadays most of the people browse internet using their mobile devices. Therefore, responsiveness is a main feature in a website. 
There are main key elements that we should know when developing a responsive website. Those are,

* Layout
* Fluid grids
* Flexible images

## Layout

There should be at least 3 breakpoints according to different browser widths.
* Small (600px and below)	– 	for low width mobile devices.
* Medium (600px – 900px)	–	for large screen mobile phones and tabs.
* Large (over 900px) 		– 	for personal computers and lager screens.

You can use CSS media queries for this.

```css
    @media only screen and (max-width: 600px){
        //Styles for small screens
    }
    @media only screen and (min-width: 600px) and (max-width: 900px){
        //Styles for medium screens
    }

    @media only screen and (min-width: 900px){
        //Styles for large screens
    }
```

## Fluid Grids

In traditional web designing uses fixed sizes to define the sizes of elements. but in present there so many different screen sizes. So, element sizes should be fit to them. To solve this issue, you can define sizes using a percentage or relative size.
Example for relative units:
* em	- Relative to the font-size of the element
* ex 	- Relative to the x-height of the current font
* ch	- Relative to width of the "0"
* rem	- Relative to font-size of the root element
* vw	- Relative to 1% of the width of the viewport
* vh	- Relative to 1% of the height of the viewport
* %	- Relative to the parent element

## Flexible images

Working with images is a major issue in responsive web designing. We need to make sure image’s quality is always the same. If an image exceed it’s maximum width, it begins to stretch and lose quality.  We can limit the image size using following,

```css
.class{
    max-width: 100%;
}
```

As long as no other width-based image styles override this rule, every image will load in its original size, unless the viewing area becomes narrower than the image’s original width.