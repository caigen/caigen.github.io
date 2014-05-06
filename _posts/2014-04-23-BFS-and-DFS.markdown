---
layout: post
title:  "DFS and BFS"
date:   2014-04-23 16:00:00
categories: Algorithm
---

Tips:

    Breadth First Search -> Queue.
    Depth First Search -> Stack.

Demo:

    #include <assert.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>
    
    #include <queue>
    #include <stack>

    bool* alloc_matrix(const int num) {
       bool* adj = (bool *)malloc(num * num * sizeof(bool));
       memset(adj, false, num * num * sizeof(bool));
       return adj;
    }

    // white vertices: not ready.
    // gray vertices: ready to access.
    // black verices: accessed.

    // when a vertex is turned from gray to black, its unvisited adjacent vertices 
    // should be turned from white to gray and pushed into the ready queue.
    void BFS(const bool* const adj, const int num) {
        bool* const visited = (bool *)malloc(num * sizeof(bool));
        memset(visited, false, num * sizeof(bool));

        std::queue<int> gray;
        gray.push(0);
        
        while (!gray.empty()) {
            int id = gray.front();
            if (visited[id] == false) {
                visited[id] = true;
                printf("%d\n", id);
            }
            gray.pop();

            for (int i = 0; i < num; ++i) {
                if (adj[id * num + i] == true && visited[i] == false) {
                    gray.push(i);
                }
            }
        }
        // Attention!
        free(visited);
    }

    void DFS(const bool* const adj, const int num) {
        bool* const visited = (bool *)malloc(num * sizeof(bool));
        memset(visited, false, num * sizeof(bool));

        std::stack<int> black;
        visited[0] = true;
        printf("0\n");
        black.push(0);

        while (!black.empty()) {
            // find first unvisited adjacent vertex
            int id = black.top();
            int next = -1;
            for (int i = 0; i < num; ++i) {
                if (adj[id * num + i] == true && visited[i] == false) {
                    next = i;
                    break;
                }
            }

            if (next != -1) {
                visited[next] = true;
                printf("%d\n", next);
                black.push(next);
            } else {
               black.pop(); 
            }
        }
        
        free(visited);
    }

    // no recursive breadth first search! (as far as i know)
    // recursive depth first search
    void DFS2(bool* adj, const int num, int id) {
        static bool visited[16];
        assert(num <= 16);
        assert(visited[id] == false);

        visited[id] = true;
        printf("%d\n", id);

        for (int i = 0; i < num; ++i) {
            if (visited[i] == false && adj[id * num + i] == true) {
                DFS2(adj, num, i);
            }
        }
    }

    int main(int argc, char* argv[]) {
        const int vertices = 4;
        bool* const adj = alloc_matrix(vertices);
        memset(adj, false, vertices * vertices * sizeof(bool));
       
        // 0 - 1
        // |   |
        // 2   3
        printf("\n\
        // 0 - 1\n\
        // |   |\n\
        // 2   3\n");
        adj[0*vertices + 1] = true;
        adj[0*vertices + 2] = true;
        adj[1*vertices + 0] = true;
        adj[1*vertices + 3] = true;
        adj[2*vertices + 0] = true;
        adj[3*vertices + 1] = true;

        printf("Breadth First Search:\n");
        BFS(adj, vertices);
       
        printf("Depth First Search:\n");
        DFS(adj, vertices);

        printf("Recursive Depth First Search from 1:\n");
        DFS2(adj, vertices, 1);

        free(adj);

        return 0;
    }
