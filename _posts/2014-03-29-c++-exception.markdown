---
layout: post
title:  "c++ exception"
date:   2014-03-29 12:50:00
categories: jekyll update
---

###/usr/include/c++/4.4/exception

    extern "C++" {

    namespace std
    {
        class exception
        {
        public:
            exception() throw() { }
            virtual ~exception() throw();

            virtual const char* what() const throw();
        }

        class bad_exception : public exception()
        {
        public:
            bad exception() throw() { }
            virtual ~bad_exception() throw();

            virtual const char* what() const throw();
        }

        typedef void (*terminate_handler) ();
        typedef void (*unexpectd_handler) ();

        terninate_handler set_terminate(terminate_handler) throw();
        void terminate() __attribute__ ((__noreturn__));
        unexpected_handler set_unexpected(unexpected_handler) throw();
        void unexcepted() __attribute__ ((__noreturn__));

        bool uncaught_exception() throw();
    }

    } // extern "C++"


###/usr/include/c++/4.4/stdexcept

    ...
    class logic_error : public exception
    {
        string _M_msg;

    public:
        explicit
        logic_error(const string& __arg);

        virtual
        ~logic_error() throw();

        virtual const char* 
        what() const throw();
    }
    class domain_error : public logic_error;
    class invalid_argument : public logic_error;
    class length_error : public logic_error;
    class out_of_range : public logic_error;

    class runtime_error : public exception;
    class range_error :  public runtime_error;
    class overflow_error : public runtime_error;
    class underflow_error : public runtime_error;
    ...

###Useful info.

15.4 Exception specifications in `cpp_standard.pdf` file.

    A function with no exception-specification allows all exceptions. A function with an empty exception-specification, throw(), does not allow any exceptions.


###Tips

    1. explicit constructor must be called like `logic_error("logic")`.
    2. only member function could have const qualifier which means that it would not change the member data. non-member function cannot have cv-qualifier. 

    // You could try to compile the below code with g++. 
    // error: non-member function 'void hi()' cannot have cv-qualifier.  (cv is short for 'const/volatile')
    void hi() const 
    {
    }

