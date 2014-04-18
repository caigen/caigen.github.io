---
layout: post
title:  "credentials"
date:   2014-04-18 19:10:00
categories: jekyll update
---

#### credentials


    man id
    id root
    id <user name>
    man credentials


#### functions demo


    #include <unistd.h>
    #include <stdio.h>

    int main(int argc, char* argv[]) {
        pid_t pid = getpid();

        printf("Process ID:\n");
        printf("    getpid() is: %d\n", pid);
        printf("    getppid() is: %d\n", getppid());


        printf("Process Session ID & Process Group ID:\n");
        printf("    getsid() is: %d\n", getsid(pid));
        printf("    getpgid() is: %d\n", getpgid(pid));

        printf("User ID & Group ID:\n");
        // real user id
        printf("    getuid() is: %d\n", getuid());
        printf("    getgid() is: %d\n", getgid());
        // effective user id
        printf("    geteuid() is: %d\n", geteuid());
        printf("    getegid() is: %d\n", getegid());

        // saved set-user-ID
        uid_t ruid, euid, suid;
        getresuid(&ruid, &euid, &suid);
        printf("    ruid is: %d\n", ruid);
        printf("    euid is: %d\n", euid);
        printf("    suid is: %d\n", suid);

        return 0;
    }


    g++ credentials.cpp
    ./a.out
    ps -aux | grep Shell
