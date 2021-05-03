---
layout: tweetcart
title: Tunnel - PICO-8 Tweetcart Studies
---

## Tunnel

_Author: Daniel Oaks ([@pixienop](https://twitter.com/pixienop))_<br>
_Link: [https://twitter.com/pixienop/status/1180850535026421765](https://twitter.com/pixienop/status/1180850535026421765)_

<img class="screenie" src="/img/tweetcarts/pixtunnel.gif" alt="Tunnel">

#### Display Palette
{% include pico8-palette.html colours="0,12,1,2,-8,8,14,15,-1,-7,9,10" %}

#### Summary
This is a tunnel that only uses simple circles. To fill in the empty space, it doesn't call `cls()` at the start of each frame and uses the colours that already exist on the screen (see the [cls-vs-dithering page](./basics#cls-vs-dithering) for some more info on this technique).

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/pixtunnel-cls.png">
<p>This is how many pixels get drawn each frame. The black space is empty and where the previous frame's colours would show through.</p>
</div>

<div>
<img src="/img/tweetcarts/pixtunnel-no-time-shifting.gif">
<p>The effect without the 'time shifting' on <code>st</code>. Doesn't look that interesting – the strange, kinda-wonky time shifting gives it character.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.pixtunnel %}
