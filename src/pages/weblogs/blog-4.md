---
layout: ../../layouts/WebLogLayout.astro
title: 'Software Renderer Rewrite, Makefiles, Line Drawing'
date: '3.6.2024'
description: "Rewrote some software renderer code, added a Makefile, and implemented a line-drawing algorithm."
---
I've been busy recently and haven't had a ton of time to work on my software renderer, but I've been working on it little by little.

First, I decided to rewrite the matrix library in order to be a little more memory efficient. The abstraction of being able to declare arbitrary sized matrices isn't really important for this application, so I think I'm just going to hardcode types for vec2, vec3, vec4, mat4x4, and the operations for each one. It's a little extra effort and a little bit of bad practice to end up with a much more efficient renderer that will be better once I get real-time rendering working.

Anyways, the number of source files reached a point where it became too much work to compile and link each source file in the command window every time, so I went ahead and installed a Windows version of make and made a makefile for the project. Here's a snippet of the makefile:
![Makefile](/personal-website/blogimages/blog4/blog4_makefile.png)

The biggest thing I've done is a basic implementation of a line-drawing algorithm (adapted from Bresenham's Line Drawing algorithm). It's very important to draw lines for the next stage of the renderer to get some basic wireframe output, so this is a prerequisite for finally starting on the rendering pipeline. Originally I was trying to iterate over every pixel of the buffer and check for distance from a given line, but this is fairly inefficient and has issues when you want to draw line segments (which is almost always in a renderer), plus I had difficulty deriving the point-to-line distance formula in a way that would work quickly in the code. So I decided to go for a basic line-drawing algorithm for approximating.

Basically, you get the start and end pixels from the cartesian coordinates of the start and end of the line segment, and then use the slope from the line defined by this points to iterate horizontally and adjust the vertical position of the painted pixel (usually multiple times if the slope is high). I had a really annoying bug where I was outputting the buffer contents to the .tga output with reversed x and y coords, which is unnoticeable when you're working with the y=x line but became confusing once I started getting flipped output that I thought was due to the line drawing algorithm. Once I found the issue, the line drawing worked fine for positive sloped lines.

I just added a negative multiplier and used it to reverse the direction of the y-coord adjustment for the cases in which slope was negative. Then I added a basic case for vertical lines.

Anyway, here's a screencap of the function (note the new vec2 type, as well as the new functions for interacting with the buffer):
![Line Drawing Function](/personal-website/blogimages/blog4/blog4_linedrawing.png)

Here's an example of some output from testing:
![Line drawn on screen](/personal-website/blogimages/blog4/blog4_lineoutput.png)

So that's what I've been up to, software-renderer-wise. Hopefully soon I'll have a basic rendering pipeline implemented, at which point I'll go ahead and push what I have so far to GitHub.