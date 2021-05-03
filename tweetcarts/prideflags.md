---
layout: tweetcart
title: Pride Flags - PICO-8 Tweetcart Studies
---

## Pride Flags

_Author: Daniel Oaks ([@pixienop](https://twitter.com/pixienop))_<br>
_Link: [https://twitter.com/pixienop/status/1271731359032111105](https://twitter.com/pixienop/status/1271731359032111105)_

<img class="screenie" src="/img/tweetcarts/prideflags.gif" alt="Pride Flags">

#### Display Palette
{% include pico8-palette.html colours="0,8,10,12,136,141,1,5,14,7,137,2" %}

#### Summary
This is a pretty cute cart that draws a bunch of flags on the screen! It stores the flag colours in a string, then extracts each colour using `ord()` (props to [@beanborg](https://twitter.com/beanborg/status/1271798680610373632) for pointing me to that function).

Instead of `cls()`, it writes a random number of black pixels all over the screen, which creates this interesting dithered trail behind each flag. See the [cls-vs-dithering page](./basics#cls-vs-dithering) for some more info on this.

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/prideflags-nodither.gif">
<p>The same effect with a <code>cls()</code> instead of the random-black-pixels bg clearing.</p>
</div>

<div>
<img src="/img/tweetcarts/prideflags-blocks.gif">
<p>The same effect with a flag string of <code>111222333444555666</code></p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.prideflags %}
