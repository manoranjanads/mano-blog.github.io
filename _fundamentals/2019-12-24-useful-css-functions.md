---
title: "Useful CSS functions"
permalink: /fundamentals/useful-css-functions.html
excerpt: "Mobile responsive web design"
header:
  overlay_image: /assets/images/fundamentals/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/coverpic.jpg
  teaser: /assets/images/fundamentals/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/css.png
  overlay_filter: 0.4
last_modified_at: 2019-12-24T11:00:07-04:00
toc: true
author: Mano
# categories:
#   - css
#   - html
# tags:
#   - html
#   - css
---

## Cubic-bezier() Function

CSS timing function is very useful when you are making an animation. It allows you to customize the animation by changing the speed of the process.  For this we can use cubic-bezier keyword in CSS.

This function needs 4 parameters. These parameters are integers between 0 and 1 for x and any integer for y, which represent points on a graph.

```css
.class {
    transition: all cubic-bezier(x1,y1,x2,y2) 2s;
}
```

## calc() Function

This function allows you to perform basic math operations on values, and it’s especially useful when you need to add or subtract a length value from a percentage. This will be useful for responsive web design.

```css
.class {
    Width: calc(100vw - 200px);
}
```

## var() Function

This allows you to manage common variable values in a single place.  This is useful for customizable themes.

```css
:root { 
--common-color: red;
}

.class-1 {
background -color: --common-color;
}

.class-2 {
color: --common-color;
}
```