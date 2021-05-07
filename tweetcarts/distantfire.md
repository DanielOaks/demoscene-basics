---
layout: tweetcart
title: distant fire - PICO-8 Tweetcart Studies
---

{% include tweetcart-header.html cart=site.data.pico8.tweetcarts.distantfire %}

#### Colour Pattern
{% include pico8-palette.html colours="0,1,2,14,9,8,10,7" %}

#### Summary
This is a really cool flame effect! It uses the spritesheet to store the actual energy of each pixel (`0-15`), then writes the corresponding colour to the screen.

Every time a pixel is rendered, its current energy and the energy of a random nearby pixel below it get added together (with some multiplying/dividing), and in the end that lets the cart create some amazingly natural-looking clouds above the smoke.

Even when pixels are rendered as totally black, there's a good chance that there's still energy in that spot – more energy for areas which are 'hotter' or have had flames burning through them recently. Which is really interesting, there's more than meets the eye when you first look at this effect.

#### Pictures
<div class="halfgrid">

<div>
<img src="/img/tweetcarts/distantfire-energy.gif">
<p>Here we're showing what the spritesheet looks like (the real energy values that get stored).</p>
<p>At the end we switch to showing the actual screen values, see just how much energy data is in those areas that seem fairly empty.</p>
</div>

<div>
<img src="/img/tweetcarts/distantfire-lively.gif">
<p>This is what happens if we swap <code>*2)/2.87</code> for <code>*5)/5.87</code> when calculating <code>c</code>. The fire's more lively (white/red/etc travel higher) but the large clouds also dissipate quicker.</p>
</div>

</div>

{% include tweetcart-code.html cart=site.data.pico8.tweetcarts.distantfire %}
