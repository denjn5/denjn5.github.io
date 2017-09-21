---
layout: post
title: Sunburst Labels, Part 2 (Tutorial 4)
date: 2017-08-15
categories: d3 sunburst
tags: d3 tutorial d3v4 javascript sunburst
excerpt_separator: <!--more-->


permalink: /sunburst-4/
png_url: ../images/sunburst-4.png
html_url: ../d3/sunburst-4.html
code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-4.html

old_permalink: /sunburst-3/
old_html_url: ../d3/sunburst-3.html
old_code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-3.html

blocks_url: https://bl.ocks.org/denjn5
---

![viz]({{ page.png_url }}){:style="float: left; margin-right: 20px; width: 300px;"}  In this tutorial we'll start with our labelled sunburst (Tutorial 3) and make some important improvements: better font and several label formatting additions (wrap, avoid leaking over edges, and "no select") to our Tutorial 2 sunburst.

<!--more-->

The [tutorials page](/tutorials/) includes an overview of all tutorials on this site.

I hope this tutorial helps you deepen your d3 visualization skills. If it does that, or if you have an idea about how to improve something below, let me know in the comments section. Do good!

<cite>—David Richards</cite>


## Tutorial Contents
- [Summary](#summary)
    - [Just the Code](#just-the-code)
- [Google Font](#google-font)
- [Make Labels 'Non-Selectable'](#make-labels-non-selectable)
- [Format the Path](#format-the-path)


## Summary
In this tutorial, we move from a plain sunburst, to one that has labels. (Tips: Keep the raw code open in a separate tab. Do a code compare between tutorial 1 and 2.)

<table class="center">
<tr>
    <td class="center">Old: <b>Labels</b>&nbsp;&nbsp;
            <a href="{{ page.old_permalink }}" target="_blank" title="open previous tutorial">
                <i class="fa fa-graduation-cap" aria-hidden="true"></i></a>
            <a href="{{ page.old_code_url }}" target="_blank" title="open code">
                <i class="fa fa-code" aria-hidden="true"></i></a>
            <a href="{{ page.old_html_url }}" target="_blank" title="open viz">
                <i class="fa fa-external-link" aria-hidden="true"></i></a></td>
    <td></td>
    <td class="center">New: <b>Label Improvements</b>&nbsp;&nbsp;
            <a href="{{ page.code_url }}" target="_blank" title="open code">
                <i class="fa fa-code" aria-hidden="true"></i></a>
            <a href="{{ page.html_url }}" target="_blank" title="open viz">
                <i class="fa fa-external-link" aria-hidden="true"></i></a></td>
</tr>
<tr>
    <td class="center"><iframe width="325" height="325" src="{{ page.old_html_url }}"></iframe></td>
    <td class="center"><i class="fa fa-arrow-circle-right" aria-hidden="true"></i></td>
    <td class="center"><iframe width="325" height="325" src="{{ page.html_url }}"></iframe></td>
</tr>
</table>

### Just the Code
The tl;dr version of this tutorial:




---
## Format with CSS
CSS (Cascading Style Style) is a powerful set of formatting and animation features. Most web pages have a `<style>` section towards the top used to set formatting. We've actually already been using CSS, maybe without realizing it: anytime we call the `.style()` method in d3, it's CSS.

Imagine we have an svg circle that we'd like to size and color: `<circle class='circles' id='myCircle'></circle>`.  We'd typically link a block of CSS key:value pairs to our page in 1 of 3 ways, easier shown than described:
1. 

When to  we use a `<style>` block

### Google Font
This section is optional; it updates the font for our sunburst.

``` css
@import url('https://fonts.googleapis.com/css?family=Raleway');

body {
  font-family: "Raleway", "Helvetica Neue", Helvetica, Arial, sans-serif;
}
```

We'll use `<style>` blocks for CSS a lot in the future. So it's helpful if you get used to them now. Plus, I'm not a fan of the default font for our sunburst labels. Let's add a style block to our html to gussy it up. I've
* `@import` gets the Raleway font from [Google](https://fonts.google.com/).
* `font-family` tells our page to use Raleway first, but provide alternatives if it's not available.

---


### Make Labels 'Non-Selectable'
In Tutorial 3, the label text was selectable.  That'll get annoying as we get users to interact with our viz. We've added a line to our CSS to avoid that.

``` css
text { pointer-events: none; }  /* Make text 'non selectable' */
```

The new style directive `text { pointer-events: none; }` tells our page that whatever is in the `<text>` element is not selectable with the mouse-pointer


### Format the Path
Common styling is often handled in a `<style>` block at the top of our code. That lightens up our JavaScipt / d3 and makes consistent styling between our visualizations easier to manage.

``` css
path { 
    stroke: #fff; 
    stroke-width: 1px;
}
```

We can use the style section to replace any fixed formatting that we've placed in `.style()` commands (unless it's programmatically driven).

These lines style any `<path>` elements in our code (unless they are overwritten by higher-priority styling). The path is the line between our slices. We color the path white (#fff).  This should replace the `.style('stroke', '#fff')` that was in our `slice.append('path')` block. We also set the width of the stroke to 1px.


---


https://stackoverflow.com/questions/15506269/d3-sunburst-clip-path-of-text

![finish]()<br>
