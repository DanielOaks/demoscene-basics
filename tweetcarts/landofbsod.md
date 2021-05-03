---
layout: tweetcart
title: In the land of BSOD - PICO-8 Tweetcart Studies
---

## In the land of BSOD

_Author: Michał Rostocki ([@von_rostock](https://twitter.com/von_rostock))_<br>
_Link: [https://twitter.com/von_rostock/status/1327057028825698304](https://twitter.com/von_rostock/status/1327057028825698304)_

<img class="screenie" src="/img/tweetcarts/landofbsod.gif" alt="In the land of BSOD">

#### Display Palette
{% include pico8-palette.html colours="0,-4,12,6,7,7,7,-13,3,-5,-6" %}

#### Summary
This lovely tweetcart is actually a fairly simple effect - just two plasmas! A fairly tame plasma for the hill, and a more intense one for the sky (each one set to use different colours).

If you're unfamiliar with plasmas, it's an effect where you keep throwing different `sin`, `cos`, and similar functions on top of each other until you get a random-looking, but **contiguous**, output.  See the [plasmas page](./basics#plasmas) for some more info on this effect.

This tweetcart does a lot of processing on each pixel. Each loop only sets the colour of a single random pixel, so it takes some time to get every pixel on the screen filled with a real value.

Most pico-8 games call `cls()` at the start of every frame, but this effect doesn't really define 'frames' and just keeps drawing new pixels on top of the screen. Because which pixel is drawn is chosen at random, it takes a while to hit every pixel on the screen when starting up. And because the screen will always be in a good-looking state, there's no need to ever call `flip()`. See the [cls-vs-dithering page](./basics#cls-vs-dithering) for some more info and examples of this.

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/landofbsod-startup.gif">
<p>When starting up it takes a while to hit every pixel on the screen.</p>
</div>

<div>
<img src="/img/tweetcarts/landofbsod-dashes.gif">
<p>Using ▤ instead of ▒ for the fill pattern.</p>
</div>

<div>
<img src="/img/tweetcarts/landofbsod-cleanframe.gif">
<p>Rendering a single complete frame. We don't see the dithering that's present in the full effect because we're not rendering any pixels on top.</p>
</div>

<div>
<img src="/img/tweetcarts/landofbsod-2not6.gif">
<p>The effect with <code>i=0,2</code> instead of the regular <code>i=0,6</code>. Less iterations and layers of <code>sin()</code>s means a more basic gradient for the plasma.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.landofbsod %}
