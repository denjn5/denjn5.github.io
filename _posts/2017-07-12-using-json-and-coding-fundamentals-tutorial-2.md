---
layout: post
title: Using Json & Coding Fundamentals (Tutorial 2)
social_title: d3 Using Json & Coding Fundamentals
date: 2017-07-12
categories: d3 sunburst
tags: d3 tutorial d3v4 javascript sunburst
excerpt_separator: <!--more-->


permalink: /sunburst-2/
png_url: ../images/sunburst-2.png
html_url: ../d3/sunburst-2.html
code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-2.html

old_permalink: /sunburst-1/
old_html_url: ../d3/sunburst-1.html
old_code_url: https://github.com/denjn5/denjn5.github.io/blob/master/d3/sunburst-1.html

compare_url: https://www.diffchecker.com/UycnfVx8
blocks_url: https://bl.ocks.org/denjn5
---

![viz]({{ page.png_url }}){:style="float: left; margin-right: 20px; width: 300px;"} In this short tutorial we'll begin with the "no frills" sunburst from Tutorial 1. But now we'll load our data from a separate JSON file _and_ move move our core d3 logic into a function. We're beginning to think _d.r.y._

<!--more-->

The [tutorials page](/tutorials/) includes an overview of all tutorials on this site.

I hope this tutorial helps you deepen your d3 visualization skills. If it does that, or if you have an idea about how to improve something below, let me know in the comments section. Do good!

<cite>—David Richards</cite>

## Tutorial Contents
<blockquote class="narrow">Not all treasure's silver and gold, mate.<br><cite>–Jack Sparrow, Pirates of the Caribbean</cite></blockquote>

- [Summary](#summary)
    - [Just the Code, Please](#just-the-code-please)
- [Get the Data](#get-the-data)
- [Function Wrapper](#function-wrapper)
    - [Functions (Coding Fundamentals 1)](#functions-coding-fundamentals-1)
    - [Docstrings (Coding Fundamentals 2)](#docstrings-coding-fundamentals-2)
    - [D.R.Y. Coding (Coding Fundamentals 3)](#dry-coding-coding-fundamentals-3)


## Summary
In this tutorial, we move from _No Frills_ to _Pulling data from a JSON file_. Both files look the same on the outside; but the new version has some neat upgrades on the inside. (Tips: Keep the raw code open in a seperate tab. Do a code compare between tutorial 1 and 2--see icons below.)

<table class="center">
<tr>
    <td class="center">Old: <b>Sunburst in 60 Lines</b>&nbsp;&nbsp;
            <a href="{{ page.old_permalink }}" target="_blank" title="open previous tutorial">
                <i class="fa fa-graduation-cap" aria-hidden="true"></i></a>
            <a href="{{ page.old_code_url }}" target="_blank" title="open code">
                <i class="fa fa-code" aria-hidden="true"></i></a>
            <a href="{{ page.old_html_url }}" target="_blank" title="open viz">
                <i class="fa fa-external-link" aria-hidden="true"></i></a></td>
    <td></td>
    <td class="center">New: <b>Pull data from a JSON file</b>&nbsp;&nbsp;
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

### Just the Code, Please
The _tl;dr_ version of this tutorial:
* We moved the contents of our `vData` variable to a file named `data-simple.json`, deleted the `vData` declaration, and inserted the following block:

``` javascript
d3.json('data-simple.json', function(error, vData) {
    if (error) throw error;
    drawSunburst(vData);
    });
```

* Then we wrapped our sunburst drawing code (the part that's driven by our specific data) in a function. The code is exactly the same, except for a small change to the `var vRoot` declaration:

``` javascript
function drawSunburst(data) {
    var vRoot = d3.hierarchy(data).sum(function (d) { return d.size});
    // the rest of our original code...
}
```

See below for a discussion of these changes.

---

## Get the Data
In the first tutorial, our data was embedded in the `vData` declaration.  In this sunburst, we'll store the data in a separate file named `data-simple.json` and added the following block:

``` javascript
d3.json('data-simple.json', function(error, vData) {
    if (error) throw error;

    drawSunburst(vData);
    });
```

[d3.json](https://github.com/d3/d3-request/blob/master/README.md#json) is a simple d3 function that allows us to pull our data from a json file (d3 has other similar functions for csv, tsv, etc. files). We include 2 arguments:

1. `data-simple.json` (since we don't have any folder references, it assumes that this file is in the same directory as the current file)
2. A special _anonymous_ function that returns hopefully returns our data as a variable (vData) or an error. All of our code that processes and presents the data could fit within this anonymous function block. However, we've placed this code in a separate function.

_NOTE_ The `d3.json` above runs _asynchronously_. That means that d3 works on pulling the requested data, but it simultaneously allows the next block of code (whatever is right after the `d3.json` call) to begin executing immediately. So wrapping our data-driven code in a function ensures that it doesn't execute until we have either data or an error. (If I had a nickel for each time I've written a data-get without thinking about asynchronisity, I'd have at least a dollar. And copious amounts of confusion.)


---

## Function Wrapper
Let's wrap our sunburst code in a function call. Then we can send it some data and build a sunburst. We'll use functions more often as our code gets feature-rich, so it's a good skill to build. In this section, we'll also talk about _docstrings_ and _DRY coding_.

``` javascript
/**
 * Draw our sunburst
 * @param {object} data - data in hierarchical format
 */
function drawSunburst(data) {
    var vRoot = d3.hierarchy(data)
        .sum(function (d) { return d.size});
    
    // Our old d3 code here: the var arc & g.selectAll('path') blocks.
}
```

We've wrapped out data-driven logic in a function call (`function drawSunburst(data) { }`) and updated `d3.hierarchy()` to look for `data` (our function parameter) instead of `vData` (our original json-variable).

### Functions (Coding Fundamentals 1)
We'll encapsulate all of our d3 logic that requires data within this function call.
* A _function_ is a block of code designed to perform a specific task; other languages might call it a _procedure_ or a _subroutine_.
* A function is in the form of `function functionName(var) { }`. Our logic fits between the curly-braces.
* JavaScript function names are [camelCase](https://www.w3schools.com/js/js_conventions.asp).
* Function names should be meaningful. Ideally, you'll use them again-and-again. You'll remember them when writing a new viz and want to find them again quickly-and-easily.
* We'll send in a single parameter or _argument_ (`data`). It should also be camelCase and descriptively named.

We'll use functions for all kinds of specific tasks, especially those that we'll want to call repeatedly--think animation or user interaction, where we might redraw our visual repeatedly.

### Docstrings (Coding Fundamentals 2)
A _docstring_ is the comment block just before a function call that describes what the thing does and what it needs in order to run properly. While you know that today, your future self will thank you if you're a bit obvious about what you're doing and why. (Some development tools are able to pick up properly formatted docstrings and remind you of the requirements when you call this function in other parts of your code. It's very cool.) [Docstrings](http://usejsdoc.org/about-getting-started.html) usually tell readers 3 things:
1. A short statement about the goal of the function.
2. A list of the parameters expected, their type, name, and meaning.
3. If something is returned, include @return and a description of what is returned.

### D.R.Y. Coding (Coding Fundamentals 3)
Coding can be hard. And the best way to learn is often by experimenting. But, like cleaning the kitchen or the garage when we're done with the project and putting our utensils and tools away for our next job, a bit of organization will make future work easier (and quicker) to build and maintain.

<blockquote>First, your return to shore was not part of our negotiations nor our agreement so I must do nothing. And secondly, you must be a pirate for the pirate's code to apply and you're not. And thirdly, the code is more what you'd call "guidelines" than actual rules. Welcome aboard the Black Pearl, Miss Turner.<br><cite>–Barbossa, Pirates of the Caribbean</cite></blockquote>

_DRY_ stands for _Don't Repeat Yourself_. The idea is to write a chunk of code once-and-well, then use it over-and-over. This principle will cause you to spend more time crafting important chunks of code. But you'll understand this code better and it'll pay off.  However, taken to the extreme, it's possible to make our code inscrutable in our bid to make it universally valuable. It's caused some to react with their own acronyms, like [DAMP](https://stackoverflow.com/questions/6453235/what-does-damp-not-dry-mean-when-talking-about-unit-tests) (_Descriptive And Meaningful Phrases_) and [WET](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (_We Enjoy Typing_). Or we spend too much time refactoring existing code that works pretty well, trying to get it just right. Remember, this is just a guideline, not a rule. Use it wisely.

---

## Well Done!

_Nice!_ You've made it through 2 tutorials (or maybe you wandered directly into this one). You've now separated you data and your presentation layer, which is a principle of good clean programming. We've still just scratched the surface. If you're ready, join me for Tutorial 3.

![big finish](../images/israel-hands-stamp.jpg){:style="float: left; margin-right: 20px; width: 350px;"} _Israel Hands was an 18th century Caribbean pirate who was second-in-command under Blackbeard. ([credit](http://www.trussel.com/rls/rlscayman.htm))_<br>


---

