---
layout: post
title: Sunburst Labels (Tutorial 3)
date: 2017-07-15
categories: d3 sunburst
tags: d3 tutorial d3v4 javascript sunburst
excerpt_separator: <!--more-->

permalink: /sunburst-3/
png_url: ../images/sunburst-3.png
html_url: ../d3/sunburst-3.html
code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-3.html

old_permalink: /sunburst-2/
old_html_url: ../d3/sunburst-2.html
old_code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-2.html

compare_url: https://www.diffchecker.com/to3YZfR3
blocks_url: https://bl.ocks.org/denjn5
---

![viz]({{ page.png_url }}){:style="float: left; margin-right: 20px; width: 300px;"} In this tutorial we'll add properly-rotated labels to our Tutorial 2 sunburst. This will require us to update a few lines of existing code and add a new rotation function.

<!--more-->

The [tutorials page](/tutorials/) includes an overview of all tutorials on this site.

I hope this tutorial helps you deepen your d3 visualization skills. If it does that, or if you have an idea about how to improve something below, let me know in the comments section. Do good!

<cite>—David Richards</cite>


## Tutorial Contents
- [Summary](#summary)
    - [Just the Code](#just-the-code)
- [Draw a Slice for Each Node](#draw-a-slice-for-each-node)
- [Add Labels](#add-labels)
- [computeTextRotation Function](#computeTextRotation-function)


## Summary
In this tutorial, we move from a plain sunburst, to one that has labels. (Tips: Keep the raw code open in a separate tab. Do a code compare between tutorial 1 and 2.)

<table class="center">
<tr>
    <td class="center">Old: <b>Data from a JSON file</b>&nbsp;&nbsp;
            <a href="{{ page.old_permalink }}" target="_blank" title="open previous tutorial">
                <i class="fa fa-graduation-cap" aria-hidden="true"></i></a>
            <a href="{{ page.old_code_url }}" target="_blank" title="open code">
                <i class="fa fa-code" aria-hidden="true"></i></a>
            <a href="{{ page.old_html_url }}" target="_blank" title="open viz">
                <i class="fa fa-external-link" aria-hidden="true"></i></a></td>
    <td></td>
    <td class="center">New: <b>Now we have labels</b>&nbsp;&nbsp;
            <a href="{{ page.code_url }}" target="_blank" title="open code">
                <i class="fa fa-code" aria-hidden="true"></i></a>
            <a href="{{ page.html_url }}" target="_blank" title="open viz">
                <i class="fa fa-external-link" aria-hidden="true"></i></a></td>
</tr>
<tr>
    <td class="center"><iframe width="325" height="325" src="{{ page.old_html_url }}"></iframe></td>
    <td class="center"><a href="{{ page.compare_url }}" target="_blank" title="open code compare">
        <i class="fa fa-arrow-circle-right" aria-hidden="true"></i></a></td>
    <td class="center"><iframe width="325" height="325" src="{{ page.html_url }}"></iframe></td>
</tr>
</table>

### Just the Code
The _tl;dr_ version of this tutorial (in 4 easy steps):
* Replace this old line of code with two new lines of code:

``` javascript
// Old code:
var vSlices = g.selectAll('path').data(vNodes).enter().append('path');

// New code:
var vSlices = g.selectAll('g').data(vNodes).enter().append('g');
```

* Append a path element to our g elements:

``` javascript
// Old code:
vSlices.
    .attr('display', function (d) { return d.depth ? null : 'none'; })

// New code:
vSlices.append('path')
    .attr('display', function (d) { return d.depth ? null : 'none'; })
```

* Add a new block at the end of `drawSunburst()` that appends our labels:

``` javascript
vSlices.append('text')
    .filter(function(d) { return d.parent; })
    .attr('transform', function(d) {
        return 'translate(' + vArc.centroid(d) + ')rotate(' + computeTextRotation(d) + ')'; })
    .attr('dx', '-20')
    .attr('dy', '.5em')
    .text(function(d) { return d.data.id });
```

* Insert this function (outside of the `drawSunburst()` function) that calculates the proper rotation of the labels:

``` javascript
function computeTextRotation(d) {
    var angle = (d.x0 + d.x1) / Math.PI * 90;

    // Avoid upside-down labels; labels as rims
    return (angle < 120 || angle > 270) ? angle : angle + 180;
    //return (angle < 180) ? angle - 90 : angle + 90;  // labels as spokes
}
```

All of the juicy details about what and why are below.

---

## Use <g> instead of <path>
Beginning in our _Layout_ code block, we append a "container" `<g>` element for each node in our data (previously we'd appended `<path>` elements).  Then we'll stash our path elements with the g elements.

``` javascript
// Old code: Append sunburst <path> elements
var vSlices = g.selectAll('path').data(vNodes).enter().append('path');

// New code: Append sunburst <g> elements
var vSlices = g.selectAll('g').data(vNodes).enter().append('g');
```
 
In the previous tutorial, we selected our non-existent `<path>` elements below and used the d3 Update Pattern to add a `<path>` for each node in our data. The problem is that we cannot attach a `<text>` element to path.  So instead of beginning with the <path> element, we'll add container `<g>` elements (we talked about these a bit in Tutorial 1). Then we'll add both the `<path>` and our new `<text>` elements to the `<g>` element.

Our updated vSlices variable now represents a list of all of our slices as <g> elements. So when we act on vSlices, we need to append a <path> to each, like so:

``` javascript
// Old code: vSlices are <path>s, so we work on them directly
vSlices.
    .attr('display', function (d) { return d.depth ? null : 'none'; })

// New code: vSlices are <g>s, so we append <path> first
vSlices.append('path')
    .attr('display', function (d) { return d.depth ? null : 'none'; })
    // everything else in this block remains the same
```

With these changes, the code now produces html that looks like this:

``` html
<g>
    <path d="..." style="..."></path>
</g>
```

The other lines of this block remains the same as Tutorial 1. 

---

## Add Labels
Now we add our labels to each of the g elements that we created above. Then we rotate and position them properly.

``` javascript
vSlices.append('text')  // <--1
    .filter(function(d) { return d.parent; })  // <--2
    .attr('transform', function(d) {  // <--3
        return 'translate(' + vArc.centroid(d) + ')rotate(' + computeTextRotation(d) + ')'; })
    .attr('dx', '-20')  // <--4
    .attr('dy', '.5em')  // <--5
    .text(function(d) { return d.data.id });  // <--6
```

Now we'll add and populate the `<text>` elements with our data-driven titles.

1. `vSlices.append("text")` like the last block, starts with the variables that we created above that points to all of our node-based <g> elements. Then We append an empty `<text>` element to each `<g>` element.

2. As with our <path> statements, we filter out the root node (since, for our purposes, we don't want text there).

3. `.attr("transform", function(d) { return ...; })` adds a transform attribute to each our newly created `<text>` elements.
    * `"translate(" + arc.centroid(d) + ")"` moves the reference point for this `<text>` element to the center of each arc (the variable we defined above). The centroid command from d3 computes the midpoint [x, y] of each arc.
    * `"rotate(" + computeTextRotation(d) + ")"` then we'll rotate our `<text>` element a specified number of degrees. We'll do that calc in a separate function below.

4. `.attr("dx", "-20")`  // Moves the text element to the left, which makes our labels look centered.

5. `.attr("dy", ".5em")` // Pulls our text element in closer to the center of the Sunburst, which makes our labels look centered.

6. `.text(function(d) { return d.parent ? d.data.name : "" })` returns the "name" attribute for each node, unless that particular node has no parents (the "root" node).  In that case, it returns an empty string.
    1. This `d.parent` check is an alternative to the `d.depth` check that we did in Tutorial 1 for finding our _root_ node. 

Putting this block all together, each data node has an entry that looks like:
``` html
<g>
    <path d="..." style="..."></path>
    <text transform="translate(105.54,-66.97)rotate(-32.39)"
        dx="-20" dy=".35em">Topic A</text>
</g>
```

This is the end of changes to our `drawSunburst()` function.

---

## computeTextRotation Function
The computeTextRotation function calculates the correct amount of rotatation for each label based on its location in the sunburst. It also avoids upside down labels. It takes a single argument, "d", which represents a single d3 node (this function is called one time for each `text` element).

``` javascript
function computeTextRotation(d) {
    var angle = (d.x0 + d.x1) / Math.PI * 90;  // <-- 1

    // Avoid upside-down labels; labels aligned with slices
    return (angle < 90 || angle > 270) ? angle : angle + 180;  // <--2

    // Alternate label formatting; labels as spokes
    //return (angle < 180) ? angle - 90 : angle + 90;  // <-- 3
}
```

Three math-rich lines in this function:

1. `var angle = (d.x0 + d.x1) / Math.PI * 90` calculate the angle (in degrees) of the label. However, it'll draw 1/2 of the labels upside down.
    1. d.x0 = the beginning angle of this node / slice (in radians).
    2. d.x1 = the end angle of this node / slice (in radians).

2. `return (angle < 90 || angle > 270) ? angle : angle + 180` handles rotation to avoid any upside-down labels: If rotation angle is in the 1st or 2nd quadrants (top 1/2), leave the already calc'd angle. Otherwise, flip the text over so that the text appears right-side-up.

3. `return (angle < 180) ? angle - 90 : angle + 90` Alternatively, this line rotates the labels so that they appear in the traditional "spoke" formation. And it avoids any upside-down labels. It's currently commented out.

_Very good!_ You've made it through 3 tutorials. Stick with it.

---

![jamestown-ships.png](../images/jamestown-ships.png)<br>
_In December 1606, 144 people set sail for a 4-month, cross-Atlantic trip from London to Virginia. One of my boys and I visited the Jamestown settlement and the recreations of the ships that carried the explorers: Susan Constant, Godspeed, & Discovery._