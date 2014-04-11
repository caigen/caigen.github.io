---
layout: post
title:  "c++ preprocessing tokens"
date:   2014-04-11 14:40:00
categories: jekyll update
---

In `cpp_standard`, we could get the below sentence:

    2.4 Preprocessing tokens
    
    5 [Example: The program fragment x+++++y is parsed as x ++ ++ + y , which, if x and y are of built-in
      types, violates a constraint on increment operators, even though the parse x ++ + ++ y might yield a
      correct expression.  ]


It means that `x+++++y` is not legal under this clause. The parsing flow of `x+++++y` is (greedy alogrithm):

    x+++++y     // same with x++ +++y
    x ++ ++ + y
    -> error!


But the fragment `x+++ ++y` is legal. The parsing flow of `x+++ ++y` is:

    x+++ ++y    // same with x++ + ++y, etc.
    x ++ + ++ y
    -> ok!



The result of parsing it with gcc is shown as below.

    int main(int argc, char* argv[]) {
        int i = 1;
        int j = 1;
        int k1 = i+++++j;
        int k2 = i++ +++j;
        int k3 = i+++ ++j;
        int k4 = i++ + ++j;

        return 0;
    }

    test.cpp: In function ¡®int main(int, char**)¡¯:
    test.cpp:4: error: lvalue required as increment operand
    test.cpp:5: error: lvalue required as increment operand


