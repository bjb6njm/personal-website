---
layout: ../../layouts/WebLogLayout.astro
title: 'C Software Renderer Begun!'
date: '2.26.2024'
description: "Project setup for the C Software Renderer, Matrix Math Library, and TGA Image manipulation."
---
I've started work on the software renderer over the past week, written in C.

I decided to go with C for this project because it's been awhile since I've used the language, and I need a refresher. In addition, I've worked with graphics programming plenty in the past, so I figure a software renderer in C would be a good refresher project.

The first thing I did was reread the C language overview in K&R's "The C Programming Language" as a refresher on the syntax and basic idiosyncracies of the language, and then I installed MinGW to compile my code with. After that, I figured the first thing to do would be to get a barebones 'library' up and running for working with matrices and matrix operations, which are important for graphics programming as it's essentially an applied form of linear algebra.

Here's a screenshot of the header file:

![cmatrix.h](/personal-website/blogimages/blog3/blog3_cmatlibheader.png)

The full source code in the .c file will be available on my GitHub within the software-renderer project. But I'd like to make a few comments on what I've done here:

First, the most principal drawback in my implementation of matrices and their operations is that I've elected to give ALL matrices a fixed size in memory -- that being the size of the largest allowed matrix (currently 4x4). I've done this instead of worrying about dynamically allocating memory for different matrix sizes is because I imagine a lot of matrices are going to be created and destroyed in the program, and dynamic memory allocation is slow. My current implementation has some dead space with each matrix (which could add up in the future and warrant a rewrite of the implementation!), but it's a lot easier to handle things via pass-by-value and setting matrices equal to each other.

I'm currently planning on representing vectors as a 1-column matrix, however given the large amount of wasted space with my current implementation of matrices for each vector, I might write a separate struct and then make new functions to operate on combinations of vectors and matrices, such as for linear transformations in the rendering pipeline.

My goal with this project is just to get a neat little software renderer working, so I'm not currently worried about optimization or efficiency. That may change in the future, and if so, I'll have to revisit the way I handle matrices.

After I finished the cmatrix library, I needed a way to internally represent images in my program, so I created screen.h. Here's the header:

![screen.h](/personal-website/blogimages/blog3/blog3_screenheader.png)

Colors are represented by a 4-index array of fixed 0-255 valued integers, the standard RGBA implementation. The screen in practice is a 1d array of these color values, and is indexed based on x and y position according to a simple formula determined by the screen's width and height value. I'm actually unsure if the alpha channel will be necessary for this software renderer, but I can always change it later if need be to save on memory.

This is a pretty simple representation of an image, and I'm sure it will have to be expanded in the future to handle operations related to buffers.

After that, I needed a way to actually get output. I went ahead and found a simple open-source reading and writing tool for the .tga image filetype on GitHub (credit to Caden Ji's tgafunc repository: https://github.com/cadenji/tgafunc) and have included it in my project for the time being as a way to get output and see whether or not my rendering code works. I modified Caden Ji's example of writing and saving a .tga image to work with my data structure in screen.h, so now I can save any screen to a .tga image. I plan to get output this way before trying to have any sort of real-time rendering going on in a window, which I want to get around to implementing AFTER I get the basic rendering pipeline implemented.

Anyways, here's a (screenshot of) a 4x4 .tga image where I specifically made the values at (0, 1) and (1, 0) the color black, to prove it works!

![Checkerboard output image](/personal-website/blogimages/blog3/blog3_checkerboard.png)

It's getting tiring manually compiling and linking object files in the terminal, so I think the next thing I need to do is to refresh myself on how to make/use makefiles. After that, I can begin work on the main rendering pipeline. More to come soon, I hope!