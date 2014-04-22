---
layout: post
title:  "the 1st lesson of edge detection"
date:   2014-04-22 13:00:00
categories: Image Processing
---

#### Wiki

<http://en.wikipedia.org/wiki/Edge_detection>

#### Get pbm(p1) image

    convert chick.jpg chick.pbm
    pnmtoplainpnm chick.pbm p1.pbm

#### Algorithm

    if 0->1, then get a rising edge.
    if 1->0, then get a trailing edge.

#### A left to right edge detection

    // edge.c
    #include <ctype.h>
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        // 1. magic number
        for (int i = 0; i <= 2; ++i) {
            printf("%c", getchar());
        }

        // 2. resolution
        int width = 0;
        int height = 0;

        char c = getchar();
        while (isdigit(c)) {
            width = width * 10 + (c - '0');
            c = getchar();
        }
        c = getchar();
        while (isdigit(c)) {
            height = height * 10 + (c - '0');
            c = getchar();
        }
        printf("%d %d\n", width, height);


        // 3. raster
        for (int j = 0; j < height; ++j) {
            char last;
            char current = getchar();
            printf("%c", current);

            for (int i = 1; i < width; ++i) {
                last = current;
                current = getchar();

                // throw spaces.
                if (isspace(current)) {
                    printf("%c", current);
                    current = getchar(); 
                }
               
                // edge detection from left to right.
                // it does not consider the difference between the rising egdge and trailing edge.
                bool edge = (last == current ? 0 : 1);
                if (!edge) {
                    printf("0");
                }
                else {
                    printf("1");
                }
            }
            printf("%c", getchar());
        }
      
        return 0;
    }

    g++ edge.c   
    a.out <p1.pbm >edge.pbm

#### A xy(left to right & top to down) edge detection

    // edge_xy.c
    #include <string.h>
    #include <ctype.h>
    #include <stdlib.h>
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        // 1. magic number
        getchar();
        char magic = getchar();
        getchar();
        
        // 2. resolution 
        int width = 0;
        int height = 0;

        char c = getchar();
        while (isdigit(c)) {
            width = width * 10 + (c - '0');
            c = getchar();
        }
        c = getchar();
        while (isdigit(c)) {
            height = height * 10 + (c - '0');
            c = getchar();
        }
      
        // 3. raster
        char* raster = (char*)malloc(width * height * sizeof(char));
        char *edge_image = (char*)malloc(width * height * sizeof(char));
        // memset(edge_image, '0', width * height);

        for (int i = 0; i < width * height; ++i) {
            do {
                raster[i] = getchar();
            } while(isspace(raster[i]));    // throw spaces
        }

        for (int i = 0; i < width * height; ++i) {
            if ((i+1) % width != 0) {
                // edge detectiion.
                // left->right: rising edge
                if (raster[i] < raster[i+1]) {
                    edge_image[i] = raster[i];
                    edge_image[i+1] = raster[i+1];
                }
                // left->right: trailing edge
                else if (raster[i] > raster[i+1]) {
                    edge_image[i] = raster[i];
                    edge_image[i+1] = raster[i+1];
                }

                
                if ( i + width < width * height) {
                    // top->down: rising edge
                    if (raster[i] < raster[i+width]) {
                        edge_image[i] = raster[i];
                        edge_image[i+width] = raster[i+width];
                    }
                    // top->down: trailing edge
                    if (raster[i] > raster[i+width]) {
                        edge_image[i] = raster[i];
                        edge_image[i+width] = raster[i+width];
                    }
                }

                // necessary to fill in the holes.
                if (edge_image[i] != '0' && edge_image[i] != '1') {
                    edge_image[i] = '0';
                }
            } else {
                // necessary to fill in the last column.
                edge_image[i] = raster[i];
            }
        }

        printf("P%c\n%d %d\n", magic, width, height);
        for (int i = 0; i < width * height; ++i) {
            printf("%c", edge_image[i]);
            
            if ((i+1)%width == 0 || ((i+1)%width)%70 == 0) {
                printf("\n");
            }
        }
      
        return 0;
    }

    g++ edge_xy.c -o b.out
    b.out <p1.pbm >edge2.pbm

#### Yup, you got it!

![chick.jpg](http://caigen.github.io/data/2014-04-22-p1.jpg)
![p1.jpg](http://caigen.github.io/data/2014-04-22-p1.jpg)
![edge.jpg](http://caigen.github.io/data/2014-04-22-edge.jpg)
![edge2.jpg](http://caigen.github.io/data/2014-04-22-edge2.jpg)
