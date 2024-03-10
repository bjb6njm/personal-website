---
layout: ../../layouts/WebLogLayout.astro
title: 'First Rendering Output!'
date: '3.10.2024'
description: "Basic rendering pipeline transforms implemented, first visible 3d output for the software renderer!"
---
For the past few days, I've been working on and off on the software renderer to get some basic 3d output. I've been successful.

First thing I did was use the model data type I made in the model.h header to hardcode in a Unit Cube so that I'd have a model that I could actually render. This was a somewhat time consuming process since I had to define all 12 of the triangles making up the surface of the cube after defining all 8 vertices. I could have done this by making a model in a 3d program and then using some kind of model file reader to bring it into my program, however I'm not ready at this point to make/implement a model file reader, but I will have to do this eventually for more complicated output. Anywhere, here's what hardcoding a unit cube looks like:

![Unit Cube Model](/personal-website/blogimages/blog5/blog5_unitcube.png)

Pretty silly, huh?

Anyways, once that was done, I made a header file for the ACTUAL rendering component (renderer.h) and made a basic struct defining a single renderer. To actually render the scene, the program generates a bunch of "render objects" which are essentially a pointer to a model paired with a position and a rotation and sticks them in a renderer, which you can then pass a buffer for the renderer to render every render object onto, at which point the render object list resets and the whole thing is done again (every frame, once real-time rendering is implemented).

The actual rendering basically loops through every render object, loops through every triangle of the render object, and transforms the three points making up the triangle, and then draws lines between them to make a wireframe output. In the future I'll be coloring in full triangles, and hopefully eventually mapping textures. Anyways, without doing any perspective projection, I just wanted to make sure things were working okay so far, so I did a kind of "orthographic" projection (basically just taking the x and y coordinates from the model's position and scaling them and sticking them on the screen), which looked like this:

![Pseudo Orthographic Output](/personal-website/blogimages/blog5/blog5_orthooutput.png)

So far so good. Then I went ahead and used my rewritten matrix library to implement a function to get a transformation for each render object to adjust the the vertices according to the position and rotation. Basically, you apply a bunch of matrix transformations to each vertex before drawing the lines that make up the edges of each triangle. For the perspective projection, I just divided the x and y coords by the vertex's z position, which is a perspective projection with focal length of 1. I won't go deep into explaining how perspective projection works here, but my implementation is pretty standard. Finally, I have to renormalize the coordinates of each vertex on the projected plane to match the coordinates of the screen buffer, which is just flipping the y coordinate essentially.

Anyways, with all of these transformations applied and our lines drawn in each triangle, we get some (REAL!) 3d output! Here are some pictures:
![Output 1](/personal-website/blogimages/blog5/blog5_output1.png)
![Output 2](/personal-website/blogimages/blog5/blog5_output2.png)
![Output 3](/personal-website/blogimages/blog5/blog5_output3.png)

So, yeah. My software renderer is officially a 3d renderer now--although very barebones!
My plan right now for the renderer is to clean up the code a bit and maybe add full triangle drawing before finally putting the first version of the renderer on GitHub. But we'll see.