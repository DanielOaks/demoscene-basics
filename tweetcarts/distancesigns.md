---
layout: tweetcart
title: distance signs - PICO-8 Tweetcart Studies
---

## distance signs

_Author: Munro Hoberman ([@MunroHoberman](https://twitter.com/MunroHoberman))_<br>
_Link: [https://twitter.com/MunroHoberman/status/1365000731284041732](https://twitter.com/MunroHoberman/status/1365000731284041732)_

<img class="screenie" src="/img/tweetcarts/distancesigns.gif" alt="distance signs">

#### Display Palette
{% include pico8-palette.html colours="0,-16,-14,-11,-3,13,6,7" %}

#### Summary
This is a really interesting cart. It uses something like a signed distance function to create a square, and rotates the pixel it's rendering so that the square ends up rotating around the screen! For the point rotation, [this SO question](https://stackoverflow.com/questions/2259476/rotating-a-point-about-another-point-2d) and [this answer in particular](https://stackoverflow.com/a/15109215) will both be very useful for working that part of the code out.

For how the square bounds actually get calculated, take a look at [this 2d distance function page](https://iquilezles.untergrund.net/www/articles/distfunctions2d/distfunctions2d.htm) from [@iquilezles](https://twitter.com/iquilezles), and also at [some](https://youtu.be/s8nFqwOho-s) [of](https://www.youtube.com/watch?v=Ff0jJyyiVyw) [these](https://www.youtube.com/watch?v=T-9R0zAwL7s) [videos](https://www.youtube.com/watch?v=-pdSjBPH3zM) (though the videos mostly focus on 3d distance functions, the same principle applies).

This tweetcart also has a very unique look to it. A few things contribute to this. First off, all the drawing is done with `circ()` with a radius of 1 - so the actual pixel that we're rendering isn't written, we just write all four pixels that border this one. As well, in `c` we calculate the **current** value of the pixel, send it towards 0, and then add that to the new value our renderer ends up with. Both of these together means that fading-out both slowly goes darker (see the display palette above for the gradient), and it also spreads out as it fades out.

Take a close look at how `c` is calculated: `c=abs(pget(x,y)-1)`. What this means that a pixel that's coloured `0` will write a colour of `1`, and vice-versa. But that's done to all of the neighbouring pixels, not the one we're actually rendering and reading from. This **also** means that by default, the rendering function will try to create checkerboard patterns of `0` and `1` coloured pixels. And that areas that are either all `1` or all `0` will create some furious value-flipping. Looking at how the edges of different checkerboard spaces interact in the cart really reminds me of when you interrupt stable patterns in the [game of life](https://conwaylife.com/wiki/Still_life).

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/distancesigns-points.gif">
<p>This is how the cart looks with a <code>pset()</code> instead of a <code>circ()</code> with a radius of 1.</p>
<p>Less pixels are drawn, but <strong>also</strong> less pixels are dithered-out, so the fading away takes longer.</p>
</div>

<div>
<img src="/img/tweetcarts/distancesigns-cls15.gif">
<p>This is how the cart looks when starting from a <code>cls(15)</code>. The <code>min(7,*)</code> immediately flicks the colour down to pure white, and then it keeps fading towards black/purple (0/1) from there.</p>
</div>

<div>
<img src="/img/tweetcarts/distancesigns-point2.gif">
<p>This is how the cart looks when we do <code>-.2</code> in the distance function instead of <code>.3</code>. Less of the border is taken away, so the square is larger.</p>
</div>

<div>
<img src="/img/tweetcarts/distancesigns-norotate.gif">
<p>This is how the cart looks if we set <code>j=x/128</code> and <code>k=y/128</code>. This is the actual shape we're rendering (and <code>.3</code> of each border is cut off!).</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.distancesigns %}
