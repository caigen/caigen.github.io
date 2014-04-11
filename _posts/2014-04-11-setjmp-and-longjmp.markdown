---
layout: post
title:  "setjmp and longjmp"
date:   2014-04-11 18:40:00
categories: jekyll update
---

### Why?

C's `goto` only could be used in current function.

    #include <assert.h>
    #include <setjmp.h>

    int local_goto() {
        goto END;
    BEYOND:
    END:
        return 0;
    }


    int beyond_goto() {
        goto BEYOND;
        return 0;
    }

    int main(int argc, char* argv[]) {
        local_goto();
        return 0;
    }

    goto.cpp:13: error: label BEYOND used but not defined


### How to use it?

    #include <assert.h>
    #include <setjmp.h>
    #include <stdio.h>

    /* jmp_buf variable must be declared in the outermost scope of where it is used. */
    jmp_buf beyond;

    void test() {
        longjmp(beyond, 8);

        assert(0);
        printf("line %d\n", __LINE__);
    }

    int main(int argc, char* argv[]) {
        switch (setjmp(beyond)) {
            case 0:
                printf("line %d\n", __LINE__);
                test();
                break;
            case 8:
                printf("line %d\n", __LINE__);
                goto END;
                break;
            default:
                assert(0);
        }

    END:
       return 0;
    }

### The implementation of setjmp & longjmp.

See [setjmp.s](http://www.opensource.apple.com/source/xnu/xnu-792.13.8/libsa/i386/setjmp.s).


### Tips from *The Linux Programming Interface*

The setjmp() and longjmp() functions provide a way to perform a nonlocal goto from one function to another (unwinding  the stack). Optimizing compilers may rearrange the order of instructions in a program and store certain variables in CPU registers, rather than RAM. In order to avoid problems with compiler optimization, we may  need to declare variables with the `volatile` modifier when making use of these functions. 

### More ...

1. <http://blog.codingnow.com/2010/05/setjmp.html>
2. <http://en.wikipedia.org/wiki/Setcontext>

### Most ...

Now you may want to know how to use setjmp and longjmp to implement the try-catch mechanism like C++. Try it first and read <http://www.di.unipi.it/~nids/docs/longjump_try_trow_catch.html>.

A simple and clear demo:

    #include <assert.h>
    #include <setjmp.h>
    #include <stdio.h>

    #define THROW \
        longjmp(env, 1)

    #define TRY \
        jmp_buf env; \
        if (setjmp(env) == 0) {

    #define CATCH \
        } else {

    #define ENDTRY \
        }

    int main(int argc, char* argv[]) {
        TRY {
            printf("line %d\n", __LINE__);
            THROW;
        }
        CATCH {
            printf("line %d\n", __LINE__);
        }
        ENDTRY;
    }

