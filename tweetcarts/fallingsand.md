---
layout: tweetcart
title: falling sand - PICO-8 Tweetcart Studies
---

## falling sand

_Author: 2DArray ([@2DArray](https://twitter.com/2DArray))_<br>
_Link: [https://twitter.com/2DArray/status/1047932855526076418](https://twitter.com/2DArray/status/1047932855526076418)_

<img class="screenie" src="/img/tweetcarts/fallingsand.gif" alt="falling sand">

#### Summary
This is a fairly small, simple falling-sand effect. And it looks really good! There's a lot of ways to do effects like this, and this cart takes the approach of: for each pixel of sand, see whether the space right below it, below to the left, or below to the right is free, and if it is then move there. It's a short, simple approach, but it ends up working well for this.

Interestingly, cls() is never called during the render loop. But it doesn't need to be, since every time a moving pixel of sand is drawn with `pset()`, the code writes over that position with the dark-blue background colour on the next tick.

Exercise for the reader, try changing the effect so that the pile of sand can be less or more steep! Or take a look at [this tweak](https://twitter.com/Enargy/status/1048284281490079744) which allows mouse control of the drop point!

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/fallingsand-notlikely.gif">
<p>Replacing <code>rnd()<.1</code> near the end with <code>rnd()<.01</code> makes the sand 'set' slower.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.fallingsand %}
