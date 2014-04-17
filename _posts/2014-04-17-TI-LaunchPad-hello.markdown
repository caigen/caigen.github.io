---
layout: post
title:  "TI LanuchPad hello project"
date:   2014-04-17 22:00:00
categories: jekyll update
---

### Assume that you have read the basic docs about how to run the hello project.

For *Tiva C Series TM4C123G LaunchPad Evaluation Kit EK-TM4C123GXL*.


### How/where can i see the "hello world"  from `UARTprintf("Hello, world!\n")`?

In readme.txt, it says that:

    UART0, connected to the Virtual Serial Port and running at
    115,200, 8-N-1, is used to display messages from this application.

So we should:
    
    1, config the Virtual Serial Port.
    2, use the terminal tool connect it. (e.g. SecureCRT)
    3, then you could see the text when you staart to debug session and run the program.


### Explain the startup_rvmdk.S

Search the directives you aren't familiar with in <http://infocenter.arm.com/help/index.jsp>.

### All are available. Nothing More.
