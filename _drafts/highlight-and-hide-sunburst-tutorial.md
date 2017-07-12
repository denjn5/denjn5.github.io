---
layout: page
title: Highlight & Hide (d3 Sunburst Tutorial 4)

date: 2017-07-10
categories: d3 sunburst
tags: d3 tutorial d3v4 javascript sunburst
excerpt_separator: <!--more-->

pngurl: ../images/sunburst-4.png
htmlurl: ../d3/sunburst-4.html
codeurl: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-4.html
blocksurl: https://bl.ocks.org/denjn5
---

![viz]({{ page.pngurl }}){:style="float: left; margin-right: 20px; width: 400px;"}  In this tutorial we'll begin with our Tutorial 3 Sunburst and add 2 features: (a) interactive slice highlighting; (b) slice hiding. (_draft test)

<!--more-->
<!--- Sunburst Tutorial (d3 v4), Part 3 -->

Each tutorial builds on the previous one, adding new features. I strive to explain every line, and each concept within the line. If I don't explain it, or explain it well, it may be covered in a previous tutorial. Titled sections begin with a code block and then the explanation. You can also view this series on [bl.ocks.org](https://bl.ocks.org/denjn5):


1. Sunburst 1: A "No Frills" Sunburst
2. Sunburst 2: Add Labels & an external json file to our basic sunburst.
3. Sunburst 3: Add smooth updates and sorting
4. Sunburst 4: Highlight & Hide Slices

I hope that these posts will help you extend your skills, solve a problem in your own code, or build something that you're proud of. I welcome your ideas.

Do good!  _—David Richards_


## Tutorial Contents
- [Live Example](#live-example)
- [Make Labels "Non-Selectable"](#make-labels-non-selectable)
- [Format Our Page](#format-our-page)
- [Sort the Slices](#sort-the-slices)
- [Store Begin States](#store-begin-states)
- [Slice Variable](#slice-variable)
- [Get User Input Recalculate the Sunburst](#get-user-input-recalculate-the-sunburst)
- [Redraw the Sunburst](#redraw-the-sunburst)
- [The "Tween" Factory that Animates the Arc Update](#the-tween-factory-that-animates-the-arc-update)
- [The "Tween" Factory that Animates the Text Location and Rotation](#the-tween-factory-that-animates-the-text-location-and-rotation)

## Live Example
To begin, let's take a look at our [reference]({{ page.htmlurl }}) vizualization, and the [code]({{ page.codeurl }}) behind it. I find it helpful to keep the code and this tutorial open side-by-side.

<span id="codeopen">
    <a href="{{ page.codeurl }}" target="_blank" title="open code">
        <i class="fa fa-code" aria-hidden="true"></i></a>
    <a href="{{ page.htmlurl }}" target="_blank" title="open viz">
        <i class="fa fa-external-link" aria-hidden="true"></i></a>
</span>

<iframe align="center" frameborder="no" border="0" marginwidth="0" marginheight="0" width="650" height="550" src="{{ page.htmlurl }}"></iframe>



## A Section
A section preface.
 
``` html
<head>
    <script src="https://d3js.org/d3.v4.min.js"></script>
</head>
```

What just happened?



_Congratulate_ and encourage.

<hr>

Possibly include a small diversion...
