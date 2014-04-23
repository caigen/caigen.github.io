---
layout: post
title:  "vector dot production for graphics"
date:   2014-04-23 21:00:00
categories: Graphics
---

#### Dot Production

Mathematical definition example:

    [0, 1] dot [2, 3] 
        = 0 * 2 + 1 * 3 
        = 3 (scalar!)

#### Practice

    Q: how to determine that if a ray and a circle intersect?
    A: see below.

*vector4graphics.h*

    #include <assert.h>
    #include <math.h>
    #include <vector>

    using std::vector;

    template <typename T>
    vector<T> operator-(const vector<T> &a, const vector<T> &b) {
        assert(a.size() == b.size());
        
        vector<T> x(a.size(), 0);
        for (int i = 0; i < a.size(); ++i) {
            x[i] = a[i] - b[i];
        }

        return x;
    }

    // Note: here is a dot product. And .(dot) operator can not be overloaded.
    template <typename T>
    T operator*(const vector<T> &a, const vector<T> &b) {
        assert(a.size() == b.size());

        T x = 0;
        for (int i = 0; i < a.size(); ++i) {
            x += a[i] * b[i];
        }
        return x;
    }

    template <typename T>
    vector<T> operator*(T a, const vector<T> &b) {
       vector<T> x(b.size(), 0);
       for (int i = 0; i < x.size(); i++) {
           x[i] = a*b[i];
       }
       return x;
    }

    template <typename T>
    vector<T> normalize(const vector<T> &a) {
        assert(a.size() != 0);

        vector<T> x(a.size(), 0);
        T lenght = sqrt(a * a);
        for (int i = 0; i < a.size(); ++i) {
            x[i] = a[i] * 1.0 / lenght;
        }
        return x;
    }

*intersection.cpp*

    #include "vector4graphics.h"
    #include <assert.h>
    #include <stdio.h>

    // 2D space: if a ray intersects with a circle.
    template <typename T>
    bool intersection(const vector<T> &start, const vector<T> &direction,
            const vector<T> &center, const double radius) {

        assert(start.size() == 2 && direction.size() == 2 && center.size() == 2);
       
        // In Circle.
        if ((center - start) * (center - start) <= radius * radius) {
            return true;
        }

        // Out of Circle.
        T dot_product = (center - start) * normalize(direction);
        if (dot_product <= 0) {
            return false;
        } else {
            int distance_square = (center - start) * (center - start)
                - dot_product * dot_product;

            if (distance_square > radius * radius) {
                return false;
            } else {
                return true;
            }
        }
    }

    int main(int argc, char* argv[]) {
        // ^y
        // |  
        // | O
        //  ----->x
        
        vector<int> start(2, 0);
        
        vector<int> direction;
        direction.push_back(1);
        direction.push_back(0);
        
        vector<int> center;
        center.push_back(1);
        center.push_back(1);

        printf("%d\n%d\n",
                intersection(start, direction, center, 1),
                intersection(start, direction, center, 0.999)
              );

        return 0;
    }

