---
layout: page
title: Highlight & Hide (d3 Sunburst Tutorial 6)
date: 2017-08-29
categories: d3 sunburst
tags: d3 tutorial d3v4 javascript sunburst
excerpt_separator: <!--more-->

permalink: /sunburst-6/
png_url: ../images/sunburst-6.png
html_url: ../d3/sunburst-6.html
code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-6.html
blocks_url: https://bl.ocks.org/denjn5
---

![viz]({{ page.png_url }}){:style="float: left; margin-right: 20px; width: 400px;"}  In this tutorial we'll begin with our Tutorial 3 Sunburst and add 2 features: (a) interactive slice highlighting; (b) slice hiding. (_draft test)

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

## Live Example
To begin, let's take a look at our [reference]({{ page.html_url }}) visualization, and the [code]({{ page.code_url }}) behind it. I find it helpful to keep the code and this tutorial open side-by-side.

<span id="code-open">
    <a href="{{ page.code_url }}" target="_blank" title="open code">
        <i class="fa fa-code" aria-hidden="true"></i></a>
    <a href="{{ page.html_url }}" target="_blank" title="open viz">
        <i class="fa fa-external-link" aria-hidden="true"></i></a>
</span>

<iframe align="center" frameborder="no" marginwidth="0" marginheight="0" width="650" height="550" src="{{ page.html_url }}"></iframe>



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
