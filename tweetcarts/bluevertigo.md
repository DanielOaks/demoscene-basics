---
layout: tweetcart
title: Blue Vertigo - PICO-8 Tweetcart Studies
---

## Blue Vertigo

_Author: Michał Rostocki ([@von_rostock](https://twitter.com/von_rostock))_<br>
_Link: [https://twitter.com/von_rostock/status/1220430044054663168](https://twitter.com/von_rostock/status/1220430044054663168)_

<img class="screenie" src="/img/tweetcarts/bluevertigo.gif" alt="Blue Vertigo">

#### Summary
This is a tunnel effect that uses colours from the [secret palette](https://youtu.be/AsVzk6kCAJY).

Most pico-8 games call `cls()` at the start of every frame, but this effect just leaves the existing screen alone and draws over the top of it. The pictures below shows which pixels are actually drawn every frame, and also how this looks when starting up. Doing this _(instead of using `cls()` and redrawing everything)_ ensures that the screen will always be in a good-looking state, so there's no need to call `flip()` at the end of each rendering loop. See the [cls-vs-dithering page](./basics#cls-vs-dithering) for some more info on this technique.

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/bluevertigo-startup.gif">
<p>The existing frame is updated each loop (rather than using <code>cls()</code> to start fresh).</p>
<p>When starting up it takes a short while to hit every pixel on the screen.</p>
</div>

<div>
<img src="/img/tweetcarts/bluevertigo-cls-loop.gif">
<p>This shows what's actually drawn each loop</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.bluevertigo %}
