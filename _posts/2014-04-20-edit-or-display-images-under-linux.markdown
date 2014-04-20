---
layout: post
title:  "edit/display iamges under linux"
date:   2014-04-20 12:30:00
categories: jekyll update
---

#### Set up

Before going on, maybe you need run the below commands.


    # take Ubuntu as example
    sudo apt-get install netpbm
    sudo apt-get install imagemagick
    sudo apt-get install feh


#### Reading manuals


    man netpbm
    man imagemagick
    man feh


#### Try to display an image


    display untitled.ppm    # with imagemagick
    ^C
    feh untitled.ppm        # with feh
    ^C


#### Go on studying what you are curious about.

* <http://en.wikipedia.org/wiki/Netpbm>
* <http://en.wikipedia.org/wiki/Image_file_formats>

