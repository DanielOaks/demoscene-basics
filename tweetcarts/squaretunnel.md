---
layout: tweetcart
title: Square Tunnel - PICO-8 Tweetcart Studies
---

## Square Tunnel

_Author: Luca Harris ([@lucatron_](https://twitter.com/lucatron_))_<br>
_Link: [https://twitter.com/lucatron_/status/1096168653735657472](https://twitter.com/lucatron_/status/1096168653735657472)_

<img class="screenie" src="/img/tweetcarts/squaretunnel.gif" alt="Square Tunnel">

#### Tunnel Colour Pattern
{% include pico8-palette.html colours="0,1,2,8,14,15,7" %}

#### Summary
This is a tunnel that uses a really simple, but effective approach. Making tunnel effects with `circ()` that work this way is difficult because it's really easy to run into situations where empty pixels exist in the rounded corners. But since `rect()` is being used here, that problem just doesn't happen.

Actually generating the tunnel here is fairly simple. We start off at the far end (with the small radius), and then slowly step further out until the entire screen is covered. Far in the tunnel, the colour shifting happens fast and the spiraling gets more intense, and as it gets closer to the camera both of those slow down.

The most complicated part of this effect is probably the colours. The fill pattern's a checkerboard, meaning that the 'upper' colour is used for one half the screen and the 'lower' colour is used for the other half. When the tunnel has only shows full blocks of colour, that means the 'upper' and 'lower' colours are the same (and when the tunnel shows the gradient, the upper and lower colours differ). The `f()` function takes the input number and produces an output that ping-pongs from one side of the colour pattern to the other, and the inputs to that function are offset by `.5` to produce the solid and checkerboard parts. If you're after more information on pico-8 fill patterns, take a look at [this wiki page](https://pico-8.fandom.com/wiki/Fillp) and [this cart](https://mboffin.itch.io/pico8-fill-patterns).

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/squaretunnel-strippedback.gif">
<p>This is how the further parts of the tunnel look, see how the spiral gets more intense. We replaced <code>,68</code> with <code>68+min(0,sin(t()/4)*70)</code></p>
</div>

<div>
<img src="/img/tweetcarts/squaretunnel-point2.gif">
<p>This is how the effect looks if we replace <code>f(i+.5)</code> with <code>f(i+.2)</code> in the rect call. See how the <code>+num</code> controls the width of the borders.</p>
</div>

<div>
<img src="/img/tweetcarts/squaretunnel-slices.gif">
<p>This is how the effect looks if we only render a few slices (replacing the <code>,.1</code> with a <code>,5</code>). Keep an eye on the colours of each slice.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.squaretunnel %}
