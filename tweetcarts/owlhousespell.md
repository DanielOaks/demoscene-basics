---
layout: tweetcart
title: Owl House Spell - PICO-8 Tweetcart Studies
---

## Owl House Spell

_Author: bean borg ([@beanborg](https://twitter.com/beanborg))_<br>
_Link: [https://twitter.com/beanborg/status/1372709786601598983](https://twitter.com/beanborg/status/1372709786601598983)_

<img class="screenie" src="/img/tweetcarts/owlhousespell.gif" alt="Owl House Spell">

#### Summary
This is a cool little cart with some animation and a flame effect! `cls()` is never called – the cart uses this fact to draw a bunch of separate-looking candles around the outside (see the [cls-vs-dithering page](./basics#cls-vs-dithering) for some more info on this).

Because this is how it's animated, adjusting the time (the `a=t()/3`) changes how close or far apart the candles are. If the time goes quicker (`a=t()/2`) the candles are further apart, and the time being slower means the candles are closer together, shown in the examples below.

For the flame effect, the cart uses `memcpy()` to very efficiently shift the whole screen up one pixel – take a look at the [memory layout wiki page](https://pico-8.fandom.com/wiki/Memory#Screen_data) for some more info on how this works.

This cart also uses condensed `line()` calls to pack a lot of geometry into a short amount of space. Take a look at which `line()` functions only contain a single `x,y` set of parameters, and lookup 'endpoints' on [this wiki page](https://pico-8.fandom.com/wiki/Line).

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/owlhousespell-cls.gif">
<p>This is how the effect looks with a cls() after each frame.</p>
</div>

<div>
<img src="/img/tweetcarts/owlhousespell-slower.gif">
<p>This is how the effect looks when the time is slowed down, <code>a=t()/3</code> replaced with <code>a=t()/5</code>.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.owlhousespell %}
