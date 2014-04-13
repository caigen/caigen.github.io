---
layout: post
title:  "POSIX mqueue"
date:   2014-04-13 17:00:00
categories: jekyll update
---

### How to use POSIX mqueue?

You had better read the manual of mq_overview and releated mq call.

    man mq_overview
    man mq_open

    man ipcs // not fully compatible to the POSIX ipcs utility.


### Ok. Can you show me a clear demo?

*mqueue.cpp*


    #include <mqueue.h>
    #include <unistd.h>
    #include <linux/stat.h>

    #include <errno.h>
    #include <string.h> // for strerror() 
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        const char* const mq_name = "/temp.mqueue";
        mode_t mode = S_IRWXU;

        mq_attr attr;
        attr.mq_maxmsg = 10; 
        attr.mq_msgsize = 8192;

        mqd_t mqd = mq_open(mq_name, O_CREAT | O_RDWR | O_EXCL, mode, &attr);
        printf("mq_open: %d: %s\n", errno, strerror(errno));
            
        mq_send(mqd, "hi", 3, 1); 
        perror("mq_send");

        mq_close(mqd);
            
        return 0;
    }

*mqueue_receive.cpp*


    #include <mqueue.h>
    #include <unistd.h>
    #include <linux/stat.h>

    #include <stdio.h>
    #include <stdlib.h>

    int main(int argc, char* argv[]) {
        const char* const mq_name = "/temp.mqueue";
        mqd_t mqd = mq_open(mq_name, O_RDWR | O_EXCL);
        perror("mq_open");

        mq_attr attr;
        mq_getattr(mqd, &attr);

        char*  const msg_ptr = (char *)malloc(8 * sizeof(char));
        unsigned int pri;

        mq_receive(mqd, msg_ptr, attr.mq_msgsize, &pri);
        perror("mq_receive");
        printf("%s %d\n", msg_ptr, pri);

        mq_close(mqd);
        mq_unlink(mq_name);

        return 0;
    }

*how to*


    g++ -lrt mqueue.cpp
    g++ -lrt mqueue_receive.cpp -o b.out
    ./a.out
    ./b.out


### How to see the mqueue file.

    mkdir posix_mqueue
    sudo mount -t mqueue none ./posix_mqueue
    ls ./posix_mqueue
    ./a.out
    cat ./posix_mqueue/temp.mqueue
    ./b.out

    ls ./posix_mqueue
    sudo umount -t mqueue none
    ls ./posix_mqueue
    rm ./posix_mqueue -r


### Related resources you should read.

    ls /proc/sys/fs/mqueue/
    cat /proc/sys/fs/mqueue/msg_max -> 10
    cat /proc/sys/fs/mqueue/msgsize_max -> 8192
    cat /proc/sys/fs/mqueue/queues_max -> 256

    /usr/include/mqueue.h
    /usr/include/fcntl.h
    /usr/include/bits/fcntl.h
    /usr/include/linux/stat.h

    /usr/include/errno.h
    ...
    /usr/include/asm-generic/errno-base.h
    
    man errno
    man strerror
    man perror

