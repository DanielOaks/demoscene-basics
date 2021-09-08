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
        <a href="#external-resources">External Resources</a>
      </td>
      <td>
        <p>
          Links to other sites describing tweetcarts, pico-8, and effects!
        </p>
      </td>
    </tr>
    <tr class="h1">
      <td>
        <a href="#_draw-vs-goto">_draw() vs goto</a>
      </td>
      <td>
        <p>
          Describes why goto is used a lot more than the _draw() function.
        </p>
      </td>
    </tr>
    <tr class="h1">
      <td>
        <a href="#changing-the-display-palette">Changing The Display Palette</a>
      </td>
      <td>
        <p>
          Describes changing how pico-8 shows colours on the display.
        </p>
      </td>
    </tr>
    <tr class="h1">
      <td>
        <a href="#cls-vs-dithering">cls() vs dithering</a>
      </td>
      <td>
        <p>
          Describes ways to clear the background when rendering new frames.
        </p>
      </td>
    </tr>
    <tr class="h1">
      <td>
        <a href="#condensing-code">Condensing Code</a>
      </td>
      <td>
        <p>
          Describes ways to condense code, as well as specific patterns.
        </p>
      </td>
    </tr>
    <tr class="h2">
      <td>
        <a href="#flooring-numbers">Flooring Numbers</a>
      </td>
      <td>
        <p>
          Describes using the integer division operator to floor numbers.
        </p>
      </td>
    </tr>
    <tr class="h2">
      <td>
        <a href="#random-values-and-tables">Random Values and Tables</a>
      </td>
      <td>
        <p>
          Describes regenerating random values every frame (instead of using a table).
        </p>
      </td>
    </tr>
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
  </tbody>
</table>


-----


## External Resources
Others have made some amazing resources about tweetcarts, pico-8, and demoscene effects that are super useful! Please feel free to check them out!

<div class="tweetcart-grid">

<a class="tweetcart" href="https://www.lexaloffle.com/bbs/?tid=37886" title="Guide for Making Tweetcarts">
    <img src="/img/tweetcarts/resources/guide-for-making-tweetcarts.png">
    <span>@PrincessChooChoo</span>
    <span>Guide for Making Tweetcarts</span>
</a>

<a class="tweetcart" href="https://rexcellentgames.com/minifying-tweetcarts/" title="Guide on minifying tweetcarts">
    <img src="/img/tweetcarts/resources/guide-on-minifying-tweetcarts.png">
    <span>@egordorichev</span>
    <span>Guide on minifying tweetcarts</span>
</a>

<a class="tweetcart" href="https://demoman.net/?a=optimizing-for-tweetcarts" title="Optimizing Character-Count for Tweetcarts">
    <img src="/img/tweetcarts/resources/optimising-for-tweetcarts.png">
    <span>@2DArray</span>
    <span>Optimizing Character-Count for Tweetcarts</span>
</a>

<a class="tweetcart" href="https://demoman.net/" title="Demo-Man: Interactive Pico-8 and gamedev tutorials">
    <img src="/img/tweetcarts/resources/demo-man.png">
    <span>@2DArray</span>
    <span>Demo-Man: Interactive Pico-8 and gamedev tutorials</span>
</a>

</div>


-----



## _draw() vs goto
When you look at tweetcarts for the first time, you may notice that almost all of them use something like this:

```lua
::_::
  -- code here
goto _
```

instead of this:

```lua
function _draw()
  -- code here
end
```

But why? Let's dig into it.

<hr class="smol">

When used in a tweetcart, the `_draw()` function does one very specific thing. It runs the code you put inside it, runs `flip()` to push it to the screen, and then runs that code again. If your code uses more than 100% CPU, it slows the drawing down to 15fps to compensate (this is what `flip()` does).

For most pico-8 carts, this works fine. This is perfect, and exactly how you want your drawing code to be run.

Let's assume that your tweetcart also wants to do this. Here's a couple of example carts that do exactly the same thing.

using `_draw()`:

```lua
function _draw()
cls()
circ(64,64,12,7)
end
```

using `goto`:

```lua
::_::
cls()
circ(64,64,12,7)
flip()goto _
```

Even though they're both doing exactly the same thing, the `goto` example is 2 characters shorter. That's two more characters of math and effect you can squeeze in!

<hr class="smol">

But when writing effects, sometimes you don't need to use `flip()`.

If your effect is written in a way that means the screen never gets into a bad state (e.g. you never call `cls()` to clear the screen), then pico-8 can decide when to push the newest frame on its own, and it'll still look good.

Now, here's where using `goto` can be really interesting.

If your screen is never in a bad state, then you can just leave the `flip()` out.

<hr class="smol">

There are two ways you'll tend to see `goto` being used, and those are:

1. Each 'drawing loop' writes the entire screen.
2. Each 'drawing loop' only writes one pixel, or a small area of the screen.

With **(1)**, even if you need to call `flip()` it still ends up shorter and doing it with `_draw()` would be, and:

It's really hard to do **(2)** in a short way while using the `_draw()` function.

As an example, let's write up a cart that sets random pixels on the screen to random colours.

Here's how the effect looks:

<img class="screenie" src="/img/tweetcarts/basics-goto-1.gif">

<hr class="smol">

Here's that cart implemented using `goto`:

```lua
::a::
pset(rnd(128),rnd(128),rnd(16))
goto a
```

On every loop, it sets one pixel to a new random colour. Simple and effective!

<hr class="smol">

To do that nicely in a `_draw()` function would be difficult. You'd need to figure out how many pixels could be drawn each loop, and then only draw that many pixels per loop, or else get capped at 15fps. Here's the equivalent code but using `_draw()`:

```lua
function _draw()
  for _=1,6000 do
    pset(rnd(128),rnd(128),rnd(16))
  end
end
```

I've indented it here for clarity, but even without the indenting it's clear that there's a lot more to do. The `_draw()` version _is_ more configurable, but in tweetcarts we're usually optimising for short character counts rather than tweakability.

And, every time you change how complex that `pset()` function is, you'll need to check how much CPU it's using (Ctrl+P while running the cart) and adjust how many loops happen per frame.

<hr class="smol">

In short, you'll see `goto` used much more than `_draw()` in tweetcarts because it's shorter and it gives you more control.

Take a look at some of these tweetcarts to see examples of both in action:

{% include tweetcart-grid.html carts="squaretunnel,landofbsod,distancesigns" %}



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

For example, this sets colours 1-7 to be the above blue gradient. Note that we're leaving 6 and 7 out because 6/7 are already set to what we want in the default palette.

```lua
pal({-15,1,-13,-4,12},1)
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


## Condensing Code
Here are some bits of code and functions, along with shorter alternatives. Also take a look at the minifying and optimisation guides linked in the external resources above.

<hr class="smol">

### Flooring Numbers

```lua
-- original
flr(x)

-- shorter
x\1
```
Backslash is used for 'integer division' in pico-8. This lets us do the same thing as the `flr()` function but in less code.

<hr class="smol">

### Random Values and Tables

```lua
-- original
s={}
for i=1,150 do
  add(s,{rnd(128),rnd(128),rnd(30)})
end
::_::
cls()
for p in all(s) do
  pset((p[1]-t()*10*p[3])%128,p[2],7)
end
flip() goto _

-- shorter
::_::
cls()
srand() -- re-initialise rng to known value
for i=1,150 do
  pset((rnd(128)-t()*10*rnd(30))%128,rnd(128),7)
end
flip() goto _
```

Take a look at how `srand()` and `rnd()` interact on [this wiki page](https://pico-8.fandom.com/wiki/Srand).

If you are storing randomly-generated things in a table, and they don't get modified afterwards (except with consistent functions like `t()`, `sin` and `cos`), you might be able to skip the table and just re-generate the same values every tick. Just use `srand()` before the places where you calculate those consistent values and the output of `rnd()` will be the same every time!

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
