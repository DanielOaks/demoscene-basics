---
layout: tweetcart
title: Basics - PICO-8 Tweetcart Studies
---

## Basics
Here are some basic effects and techniques that are used in tweetcarts.

If you're looking for some general pico-8 information, or basic effect creation, [the demobasics page](https://demobasics.pixienop.net/) goes through creating a few traditional carts from scratch.

<table class="sections">
  <colgroup>
    <col class="name">
    <col class="description">
  </colgroup>
  <tbody>
    <tr class="h1">
      <td>
        <a href="#plasmas">Plasmas</a>
      </td>
      <td>
        <p>
          Describes plasma effects, how they work and how to create them!
        </p>
      </td>
    </tr>
    <tr class="h1">
      <td>
        <a href="#cls-vs-dithering">cls() vs dithering</a>
      </td>
      <td>
        <p>
          Describes ways to clear the background when rendering new frames
        </p>
      </td>
    </tr>
    <tr class="h1">
      <td>
        <a href="#changing-the-display-palette">Changing The Display Palette</a>
      </td>
      <td>
        <p>
          Describes changing how pico-8 shows colours on the display
        </p>
      </td>
    </tr>
  </tbody>
</table>


-----


## Plasmas
A plasma is a sort of noise function. Specifically, it's a **contiguous** noise function, meaning that there's a smooth flow between values. Compare these two examples:

<div class="halfgrid">

<div>
<img src="/img/tweetcarts/basics-plasma-1.png">
<p>non-contiguous noise</p>
<a href="javascript:;" onclick="navigator.clipboard.writeText('pal({9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135},1)::w::for x=0,127do for y=0,127 do\nk=rnd(16)pset(x,y,k)end\nend\ngoto w')">copy code to clipboard</a>
</div>

<div>
<img src="/img/tweetcarts/basics-plasma-2.png">
<p>contiguous noise</p>
<a href="javascript:;" onclick="navigator.clipboard.writeText('pal({9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135},1)::w::for x=0,127 do for y=0,127 do k=(sin(x/160)+cos(y/220)+sin(x/74)/4+sin(y/22)/10)*16pset(x,y,k)end\nend\ngoto w')">copy code to clipboard</a>
</div>

</div>

The first example is created just by calling `rnd()` on each pixel - no pixels relate to each other, it's like TV static.

The second example is created with a series of `sin` and `cos` functions, so there is a smooth relationship between these pixels.

<hr class="smol">

Plasmas get more complex as you add more `sin()` and `cos()` (and other) functions to them, and get very interesting when you start including time! Take a look at how this plasma changes as we add more functions on top:

<div class="halfgrid">

<div>
<img src="/img/tweetcarts/basics-plasma-ex1.gif">
<p><code>sin(x/160 + t()/2) + cos(y/220)</code></p>
<a href="javascript:;" onclick="navigator.clipboard.writeText('pal({9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135},1)cls(8)b=21::w::for x=b,127-b do\nfor y=b,127-b do\nk=(sin(x/160+t()/2)+cos(y/220))*16pset(x,y,k)end\nend\ngoto w')">copy code to clipboard</a>
</div>

<div>
<img src="/img/tweetcarts/basics-plasma-ex2.gif">
<p><code>+ sin(x/120)/2</code></p>
<a href="javascript:;" onclick="navigator.clipboard.writeText('pal({9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135},1)cls(8)b=25::w::for x=b,127-b do\nfor y=b,127-b do\nk=(sin(x/160+t()/2)+cos(y/220)+sin(x/120)/2)*16pset(x,y,k)end\nend\ngoto w')">copy code to clipboard</a>
</div>

<div>
<img src="/img/tweetcarts/basics-plasma-ex3.gif">
<p><code>+ cos(y/72 - t())/5</code></p>
<a href="javascript:;" onclick="navigator.clipboard.writeText('pal({9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135},1)cls(8)b=30::w::for x=b,127-b do\nfor y=b,127-b do\nk=(sin(x/160+t()/2)+cos(y/220)+sin(x/120)/2+cos(y/72-t())/5)*16pset(x,y,k)end\nend\ngoto w')">copy code to clipboard</a>
</div>

<div>
<img src="/img/tweetcarts/basics-plasma-ex4.gif">
<p><code>+ cos(x/70 + y/40 + t())/5</code></p>
<a href="javascript:;" onclick="navigator.clipboard.writeText('pal({9,137,136,2,141,12,140,1,129,131,3,139,11,138,10,135},1)cls(8)b=34::w::for x=b,127-b do\nfor y=b,127-b do\nk=(sin(x/160+t()/2)+cos(y/220)+sin(x/120)/2+cos(y/72-t())/5+cos(x/70+y/40+t())/5)*16pset(x,y,k)end\nend\ngoto w')">copy code to clipboard</a>
</div>

</div>

Notice that with each added function, the pattern gets more interesting. The amount of pixels we can render while staying at 30fps also gets smaller (see the border getting larger each time).

When your function starts getting complex, you need to make less calculations and stretch your plasma. The [landofbsod cart](./landofbsod) calculates enough new pixels that you get the gist of the movement (and the non-calculated pixels end up looking like bits of stray cloud). The [bluevertigo cart](./bluevertigo) calculates the colour of a single point and then applies that colour to an entire line segment. Lots of ways to stretch the effect so it covers the whole screen!

<hr class="smol">

You'll see this effect used all the time in the demoscene and in tweetcarts because it's easy to use and adjust (just change the values that you put into `sin` and `cos`, and adjust what you multiply/divide it all by). With enough tweaking, you can end up with something that looks like clouds, a twisty pattern for a tunnel – lots of different applications!

[This page](https://lodev.org/cgtutor/plasma.html) also has some interesting information and examples of plasmas.

<hr class="smol">

Here are some carts that use this effect:

{% include tweetcart-grid.html carts="landofbsod,bluevertigo" %}



## cls() vs dithering
Whenever you start a new frame, you can either call `cls()` to clear the screen, or you can just clear part of the screen. If you only clear part of the screen, you end up with an interesting dithered effect that can enhance how your scene looks.

<hr class="smol">

Here's an example cart that uses `cls()` every frame:

```lua
::♥::
cls()

x=64+sin(t())*30
y=64+cos(t())*30
circfill(x,y,4,7)

flip()
goto ♥
```
<img class="screenie" src="/img/tweetcarts/basics-clsdither-1.gif">

<hr class="smol">

Here's the same cart, but instead of using `cls()` it sets a bunch of random pixels to black instead:

```lua
::♥::
for x=1,6000 do
	pset(rnd(128),rnd(128),0)
end

x=64+sin(t())*30
y=64+cos(t())*30
circfill(x,y,4,7)

flip()
goto ♥
```
<img class="screenie" src="/img/tweetcarts/basics-clsdither-2.gif">

<hr class="smol">

It might look like the dithered example is too long to be useful, but it's common for intensive effects to leave out the `cls()` and render some amount of new pixels over the top of the old frame (to still update the display while using less cpu). If you render enough new pixels, it's really hard to tell that you're doing this. And if you don't render enough, you get a cool dithery look that can seem pretty intentional.

It's easiest to see this on startup, when carts that do this haven't populated the entire screen yet so there's lots of empty spaces.

Let's modify this example just a little bit and redraw the circle after we do the `flip()`, but in a different colour:

```lua
::♥::
for x=1,6000 do
	pset(rnd(128),rnd(128),0)
end

x=64+sin(t())*30
y=64+cos(t())*30
circfill(x,y,4,7)

flip()
circfill(x,y,4,t()*4)
goto ♥
```
<img class="screenie" src="/img/tweetcarts/basics-clsdither-3.gif">

<hr class="smol">

Now that looks really intentional! Here are some carts that use this technique:

{% include tweetcart-grid.html carts="bluevertigo,landofbsod" %}



## Changing The Display Palette
Here's pico-8's default display palette:
{% include pico8-palette.html colours="0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15" %}

<hr class="smol">

Say that you're rendering a yellow-red gradient and want the colours in a nicer order. You could setup the first five colours on the palette to be this instead. Then when you display 0-5 it would show a smooth gradient:
{% include pico8-palette.html colours="7,10,9,8,2,0" %}

Or, say that you want to access the secret colours to produce an even smoother blue gradient. Then you **need** to change the screen palette to something like this:
{% include pico8-palette.html colours="-15,1,-13,-4,12,6,7" %}

<hr class="smol">

There's been a lot of ways to do this in pico-8. These days, using the `pal()` table syntax is the quickest and easiest.

For example, this sets colours 0-6 to be the above blue gradient:

```lua
pal({-15,1,-13,-4,12,6,7},1)
```

However, before the table syntax was introduced you'd need to do something along these lines:

```lua
p={-15,1,-13,-4,12,6,7}
for i,c in pairs(p) do
  pal(i-1,c,1)
end
```

But there are lots of ways to do this. Anytime you see `pal(*,1)` that's probably the display palette being changed. Checkout some carts on this site, most of them will change the display palette!

<hr class="smol">

For reference, here's all of the standard and the extended colours (using the minus-names to refer to the extended colours, since they're shorter than the full colour ids):
{% include pico8-palette.html colours="0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15" %}
{% include pico8-palette.html colours="-1,-2,-3,-4,-5,-6,-7,-8,-9,-10,-11,-12,-13,-14,-15,-16" %}

<hr class="smol">

 Here are some carts that do this:

{% include tweetcart-grid.html carts="landofbsod,rainbowtentacles,bluevertigo" %}
