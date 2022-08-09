---
layout: single
title:  Get colors from images as swatches
tags: coding image-recognition image-processing
---

A few weeks ago I was playing with scientific figures and wondering how I might extract insights from them. One idea I had was to find all the colors in scientific images and rank them.

Given that different cells, cell parts, and tissues often have different colors—especially when stained to do so—that could be a productive path. For example, if cancer cells are stained a different color in an image than healthy cells, the higher the percentage of the cancer color, the worse it is.

Turns out this isn't an easy problem to solve. Why? Because while we see a few colors in an image, there are actually many variations of those colors which are imperceptible to us. 

The solution is to cluster images by color. For example, group all the reddish colors together, then all the bluish ones, and so on. And then you can determine the relative amounts of each color.

I wrote some code to do this, which I've called "[Colorgram](https://gist.github.com/simonmesmith/a1c3fdef3d8e9a03cd170cc3e7a5a596)" and uploaded as a Gist. Here's an example of it working on [a Dall-e image I generated](https://labs.openai.com/e/lsqFtxWjDEoJ7knsg5H24L0L/1hvQqqxamd1HlBcDECc2pSQz):

## Input image

![Colorful modern living room](/assets/images/colorful-modern-living-room.png)

## Colorgrammed image

![Colorful modern living room colorgrammed](/assets/images/colorful-modern-living-room-colorgrammed.png)

PS: I got a lot of knowledge and borrowed a fair amount of code from various things I found online. I didn't keep track of that, so apologies for not giving proper credit where it's due.