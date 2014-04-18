---
layout: post
title:  "Use socketpair() to simulate CPU"
date:   2014-04-18 21:30:00
categories: jekyll update
---

#### Design

    2 units: core/coprocessor
    each unit has 1 register (r0)
    2 instructions: MRC/MCR

         ----------
        | CPU Core |
         ----------
              $
         ----------
        | CPU cp15 | 
         ----------

#### Implementation


    #include <sys/socket.h>
    #include <unistd.h>
    #include <string.h>
    #include <stdlib.h>
    #include <stdio.h>

    class Unit {
    protected:
        int r0;

    public:
        explicit Unit(int socket) {
            r0 = socket;
        }
        void w(const char* const msg) const {
            //write(r0, msg, strlen(msg));
            send(r0, msg, strlen(msg), 0);
        }
        char* r() const {
            char *msg = (char *)malloc(sizeof(char) * 16);
            //read(r0, msg, 16);
            recv(r0, msg, 16, 0);

            return msg;
        }
    };

    class CPUCoprocessor: public Unit {
    public:
        explicit CPUCoprocessor(int socket): Unit(socket) {
            // Nothing.
        }
    };

    class CPUCore: public Unit {
    public:
        explicit CPUCore(int socket_fd): Unit(socket_fd) {
            // Nothing
        }

        // MRC & MCR instructions of ARM
        void MRC(const CPUCoprocessor &cp) const {
            cp.w("r <- cp");

            printf("MRC:\n");
            printf("    %s\n", r());
        }
        void MCR(const CPUCoprocessor &cp) const {
            w("cp <- r");

            printf("MCR:\n");
            printf("    %s\n", cp.r());
        }
    };

    void simulate(void) {
        int socket_fd[2];
        socketpair(AF_UNIX, SOCK_DGRAM, 0, socket_fd);

        CPUCoprocessor cp(socket_fd[0]);
        CPUCore core(socket_fd[1]);

        core.MRC(cp);
        core.MCR(cp);
    }

    int main(int argc, char* argv[]) {
        simulate();
    }
