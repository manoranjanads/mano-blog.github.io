---
title: "Shrink a div horizontally and add 3 dots"
permalink: /general/shrink-a-div-horizontally-and-add-3-dots.html
excerpt: "Guide to CSS flex-grow, flex-shrink and overflow  "
header:
  overlay_image: /assets/images/general/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/coverpic.jpg
  teaser: /assets/images/general/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/css.png
  overlay_filter: 0.4
last_modified_at: 2019-05-25T11:00:07-04:00
toc: true
author: Madusara
# categories:
#   - css
#   - flex
# tags:
#   - html
#   - css
#   - flex
#   - flexbox
#   - shrink
#   - ellipsis
---

Today I’m going to guide you through a very important css technique that may come in handy in the development of responsive websites. Say you have two columns in a row, you want to make a column to hold a fixed width and let the other column to shrink and expand when the screen width is resized and you also want to add 3 dots at the end of that shrinking column to indicate that the text is not fully shown, then this is for you.

![Final Product](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/dots1.gif)

If you want to skip the description and jump to the final code, click [here](#final-code).

### Initial HTML Snippet

![Initial Snippet](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/static-columns.png)

The above image symbolizes a parent div with two child div’s. This is where we start.

```html
<div class=”parent”>
  <div class=”left”>This length is variable</div>
  <div class=”right”>This length is fixed</div>
</div>
```

Let’s break our path down into a few steps..

### 1. Force the ‘right’ div to always hang on the right end.

<br />
![Without Step One](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/if-not-forced.png)

We want to keep the right div on the right corner and adjust the left column according to the resizing screen.

To push the right div to the ‘right’ side, we write the following css,

```css
.left {
  flex-grow: 1;
}
.right {
  flex-grow: 0;
}
```

flex-grow property is actually a proportion between flex elements indicating the amount of the available space inside the flex container the *item* should take up. So in the above code, the ‘left’ div and the ‘right’ div will take up the available space in 1:0 proportion which means the ‘left’ div will take up the complete free space when parent is widened. Hence the right element is pushed to the right and will hang on the right end whatsoever. 

Now on resize, the ‘left’ and ‘right’ will behave like the following.

![Step One](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/step1.gif)

But wait..!  We didn’t achieve our goal yet.

+ The text on ‘left’ shouldn’t break into an another line.
+ The text on ‘right’ shouldn’t break into an another line either.
+ Three dots should appear at the end of ‘left’.

### 2. Limiting the text on ‘left’ to a single line.

<br />
Add

```css
white-space: nowrap;
```

to the ‘left’ and it will maintain the text within one line regardless of the screen width. But then when the reducing width reaches a limit, the ‘left will stop shrinking. It will stop at a fixed width. But as we don’t want that, we add the following css to ‘left’

```css
overflow: hidden;
 ```

Now the ‘left’ will shrink even smaller than its actual width.

![Step Two](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/step2.gif)

### 3. Limiting the text on ‘right’ to a single line.

<br />
Add 

```css
flex-shrink: 0
```

to the ‘right’ so that it’s shrink ratio becomes 0 which effectively means that the ‘right’ div will be having a fixed width and it solves our problem.

![Step Three](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/step3.gif)

If you are using bootstrap 4, without using explicit flex values we can achieve something little similar to the above result simply with no css by the following.

```html
<div class=”row”>
  <div class=”col”>This length is variable</div>
  <div class=”col-auto”>This length is fixed</div>
</div>
```

Snippet - [CodePen](https://codepen.io/madusara/pen/wbPWRX){:target="_blank"}


### 4. Bringing in the Three Dots

<br />
Add 

```css
text-overflow: ellipsis
```

to the ‘right’ and the 3 dots will start appearing when it’s shrunk smaller than its actual width.

![Final Product](/blog/assets/images/posts/2019-05-25-shrink-a-div-horizontally-and-add-3-dots/dots1.gif)


So we finally achieved the goal…! 

<a name="final-code"></a>

#### Final Code

```html
<div class=”parent”>
  <div class=”left”>This length is variable</div>
  <div class=”right”>This length is fixed</div>
</div>
```

#### And the Final CSS be,
```css
.parent {
  display: flex;
}
.left {
  flex-shrink: 1;
  flex-grow: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.right {
  flex-shrink: 0;
}
```

By default, a flex item has its flex-grow set to 0, otherwise we would have to add it manually to .right div.

Code Snippet - [CodePen](https://codepen.io/madusara/pen/gJXwMK){:target="_blank"}