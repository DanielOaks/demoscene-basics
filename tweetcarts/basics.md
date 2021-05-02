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


### cls() vs dithering
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


-----


### Changing The Display Palette
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
