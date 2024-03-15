---
layout: ../../layouts/WebLogLayout.astro
title: 'Renderer Triangle Rasterization'
date: '3.15.2024'
description: "Implemented basic triangle rasterization in the software renderer."
---
I've gotten back to work on the Software Renderer over the past couple of days after finally getting some 3D perspective projection output last time in wireframe, so I figured the next thing I should do is go ahead and draw full triangles. This process is called rasterization in 3D graphics rendering, and essentially means coloring in the pixels contained in the triangles that have been projected onto the (abstract) screen.

For reference, here's the rendering output of what we started out with last time:
![Perspective Projection Wireframe Output](/personal-website/blogimages/blog6/blog6_projection.png)

You may notice it looks a little different from the last blog. That's because I went ahead and calculated the normal value of each triangle face and then skipped rendering the ones facing away from the camera (so only the visible front of the cube is rendered!)

Now, we need to fill these "empty" triangles. There are a few ways of doing it, one of which involves drawing lines across the breadth of the triangle, however this is an older method that isn't very useful for things I may implement in the future including UV texture mapping and more importantly a Z-buffer.

I've implemented an algorithm for this before based on barycentric coordinates (much help thanks to the writeup found on ssloy's tinyrenderer wiki page on github, here: https://github.com/ssloy/tinyrenderer/wiki/), which is essentially finding a new expression for the coordinates of each pixel on the screen based on weights assigned to the vertices of the projected triangle. You essentially make a bounding box of all pixels possibly contained in the triangle, and then iterate through them finding the barycentric coordinates of each pixel, and if the weights are all positive then the pixel lies on the inside of the triangle. I won't go through a full writeup of the linear algebra here (most of which is found on the aforementioned tinyrenderer wiki page about triangle rasterization), but here's what the bounding box of each triangle (all overlaid together) looks like:
![Bounding Boxes](/personal-website/blogimages/blog6/blog6_boundingbox.png)

Filling in each of these bounding boxes according to the barycentric coordinates of each pixel then looks like this:
![Solid Box!](/personal-website/blogimages/blog6/blog6_firstraster.png)

We have a solid box!

Without any lighting effects, it's impossible to make out the geometry of the cube, so I went ahead and make a variation of the output with the wireframe overlaid on top of the rasterized version, here:
![Wireframe + Raster](/personal-website/blogimages/blog6/blog6_wireframeraster.png)

Anyways, that's what I've been up to on the software renderer. Finally dipping my toes into more of the applied linear algebra related to software rendering. I've messed around a little with adding basic lighting effects from the triangle face normals, but haven't finished yet, so I'm hoping to get that done soon so I can show it here. I figure after I get some basic lighting effects in, I'll go ahead and implement a model file format importer so I can render some more advanced output with bigger models. Maybe after that I'll try to get a real-time rendering window with SDL implemented, and will have some videos to show off. But that's it for now.