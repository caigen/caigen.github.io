---
layout: post
title:  "POSIX shared memory"
date:   2014-04-15 17:00:00
categories: jekyll update
---

### Show me how to use POSIX shared memory.

*shm.cpp*


    #include <sys/mman.h>   // memory management.
    #include <sys/stat.h>   // file stat. man 2 stat
    #include <fcntl.h>      // O_CREAT
    #include <unistd.h>

    #include <errno.h>
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        const int page_size = getpagesize();
        printf("page_size: %d\n", page_size);
        
        const char* const shm_name = "/shm.temp";
        int shm_fd = shm_open(shm_name, O_CREAT | O_RDWR | O_TRUNC, S_IRUSR | S_IWUSR); 
        perror("shm_open");

        ftruncate(shm_fd, page_size);

        char* begin = (char *)mmap(NULL, page_size, PROT_WRITE, MAP_SHARED, shm_fd, 0); 
        perror("mmap");

        begin[0] = 'a';
        begin[1] = 'b';

        return 0;
    }


*shm_read.cpp*


    #include <sys/mman.h>   // memory management.
    #include <sys/stat.h>   // file stat. man 2 stat
    #include <fcntl.h>      // O_CREAT
    #include <unistd.h>

    #include <errno.h>
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        const char* const shm_name = "/shm.temp";
        int shm_fd = shm_open(shm_name, O_RDONLY, S_IRUSR); 
        perror("shm_open");

        char* begin = (char *)mmap(NULL, 1, PROT_READ | PROT_WRITE, MAP_PRIVATE, shm_fd, 0); 
        perror("mmap");

        printf("%c%c\n", begin[0], begin[1]);

        shm_unlink(shm_name);
        perror("shm_unlink");

        return 0;
    }

*how to*


    g++ shm.cpp -lrt
    g++ shm_read.cpp -lrt -o b.out
    ./a.out
    ./b.out


### Some related info.

    cat /proc/sys/kernel/shm*
    man shm_overview
    ...
    cat /proc/pid/maps
    strace ./a.out
    g++ -static option

