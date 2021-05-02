---
layout: tweetcart
title: Basics - PICO-8 Tweetcart Studies
---

## Basics
Here are some basic effects and techniques that are used in tweetcarts.


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

It might look like the dithered example is too long to be useful, but it's common for intensive effects to leave out the `cls()` and render some amount of new pixels over the top of the old frame (to reduce cpu usage, etc). If you render enough new pixels, it's really hard to tell that you're doing this. And if you don't render enough, you get a cool dithery look that can seem pretty intentional.

It's easiest to see this on startup, when carts that use this effect haven't populated the entire screen yet so there's lots of empty spaces.

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
