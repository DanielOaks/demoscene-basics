---
layout: tweetcart
title: Rainbow Tentacles - PICO-8 Tweetcart Studies
---

## Rainbow Tentacles

_Author: Daniel Oaks ([@pixienop](https://twitter.com/pixienop))_<br>
_Link: [https://twitter.com/pixienop/status/1180997046138032129](https://twitter.com/pixienop/status/1180997046138032129)_

<img class="screenie" src="/img/tweetcarts/rainbowtentacles.gif" alt="Rainbow Tentacles">

#### Display Palette
{% include pico8-palette.html colours="9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135" %}

#### Summary
This effect creates tentacles out of circles! The circles near the tip are really tiny, and they get bigger as you get to the base!

The screen palette is setup so that 0->15 is a rainbow (using colours from the [secret palette](https://youtu.be/AsVzk6kCAJY)). Every pixel along the tentacle, from the tip to the base, is a separate circle so we can change what colour each one is (which lets us easily create that expanding/retracting look shown above).

Because the circles 'closer to the camera' are drawn after the ones near the tip, we also make some nice overlaps that look really interesting and give a 3d look to the bottom tentacle.

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/rainbowtentacles-slow.gif">
<p>Each tentacle being drawn, slowly.</p>
</div>

<div>
<img src="/img/tweetcarts/rainbowtentacles-nopal.gif">
<p>How the effect looks without the initial <code>pal()</code> calls to setup the rainbow.</p>
</div>

<div>
<img src="/img/tweetcarts/rainbowtentacles-nopulse.gif">
<p>How the effect looks without the 'pulsing'.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.rainbowtentacles %}
