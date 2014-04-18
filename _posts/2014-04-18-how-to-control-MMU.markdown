---
layout: post
title:  "how to control MMU of CPU?"
date:   2014-04-18 14:10:00
categories: jekyll update
---

*take ARM 920T as example*


#### CP15

CP15 is short for `CoProcessor 15`. It provides additional registers that are used to configure and control the caches, etc.
E.g.: Register 13, FCSE PID register, which is shorted c13, is used to convert VA to MVA (Modified Virtual Address).


#### What does the 15 mean?

The ARM instruction set supports the connection of 16 coprocessors, numbered 0 to 15, to an ARM processor.


#### How to access the registers defined in CP15?


    MCR: coprocessor <- register (move to Coprocessor register from arm Regestier)
    MRC: register <- coprocessor


#### Reference

[1] ARM920T Technial Reference Manual
