---
layout: post
title:  "vim basic setting"
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
