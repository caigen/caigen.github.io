---
layout: post
title:  "vim basic setting & operation"
date:   2014-03-10 14:09:00
---
~/.vimrc:

    set number

    syntax on
    set hlsearch

    " related vim command: :> & :<
    set shiftwidth=4

    set tabstop=4
    set expandtab

vim file browser addin (netrw.vim) basic command:

    :e .
    <Enter>
    -

vim indent mutilines:

    visual model:v
    select mutilines:j/k
    indent/unindent:>/<

vim window operations:

    /* i don't wanna use ctrl + w. it is not comfortable. */
    :vertical split
    :vertical resize -4
    :wincmd h/l/j/k

vim code completion:
 
    ctrl + n (next)
    ctrl + p (previous)

