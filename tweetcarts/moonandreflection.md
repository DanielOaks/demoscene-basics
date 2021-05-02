---
layout: tweetcart
title: Moon and Reflection - PICO-8 Tweetcart Studies
---

## Moon and Reflection

_Author: Luca Harris ([@lucatron_](https://twitter.com/lucatron_))_<br>
_Link: [https://twitter.com/lucatron_/status/1111025092236959744](https://twitter.com/lucatron_/status/1111025092236959744)_

<img class="screenie" src="/img/tweetcarts/moonandreflection.gif" alt="Moon and Reflection">

#### Summary
This effect is a lot of lines of different lengths moving at different speeds (and wrapping back around to the left). The colour for each line is either 1 (the water colour) or is selected from the top of the screen.

`srnd()` seeds the random number generator on every frame, and this is used to get a set of random, but **consistent**, values each frame for each of the lines, their speeds, etc. It's much easier and faster to do this than to e.g. generate all those values once and then store them in a table or an array.

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/moonandreflection-slow.gif">
<p>This is a slowed-down version showing how each frame is rendered.</p>
</div>

<div>
<img src="/img/tweetcarts/moonandreflection-pixels.gif">
<p>This shows the midpoint and colour for each line (the effect with the width of each line being 0).</p>
<p>You can see how precise the reflection of the moon looks here. It just gets all jagged in the full effect because each pixel's colour is used for a whole line.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.moonandreflection %}
