---
layout: tweetcart
title: In the land of BSOD - PICO-8 Tweetcart Studies
wip: true
---

## In the land of BSOD

_Author: Michał Rostocki ([@von_rostock](https://twitter.com/von_rostock))_<br>
_Link: [https://twitter.com/von_rostock/status/1327057028825698304](https://twitter.com/von_rostock/status/1327057028825698304)_

<img class="screenie" src="/img/tweetcarts/landofbsod.gif" alt="In the land of BSOD">

#### Summary
This tweetcart does a lot of processing on each pixel. Each loop only sets the colour of a single random pixel, so it takes some time to get every pixel on the screen filled with a real value.

Most pico-8 games call `cls()` at the start of every frame, but this effect just leaves the existing screen alone and draws over the top of it. The pictures below shows which pixels are actually drawn every frame, and also how this looks when starting up. Doing this ensures that the screen will always be in a good-looking state, so there's no need to ever call `flip()`. See the [cls-vs-dithering page](./basics#cls-vs-dithering) for some more info on this technique.

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
<p>Rendering a single full frame (notice how we don't see the dithering that's present in the full effect, because we're not rendering any separate pixels afterwards).</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.landofbsod %}
