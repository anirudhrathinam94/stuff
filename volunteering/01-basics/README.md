
Environment
-----------

It is whatever you need to write and execute a program on your computer. You can split it into 3 types:

- Development environment: Everything you need to write the code. Code editors, ides etc
- Build environemnt: Given some code it is everything you need to compile it and make an executable
- Runtime environment: Everything you need to execute a program.

Let us consider a simple hello world program: The dev env is the editor/ide we used, to build it we had to use javac <Filename.java> and to run it we had to run java <Classname>.

Note: Show students my own dev env here + dot files and scripts 

Building the code
------------------

**What is the use of build automation? (Ant basics)**

Let us go back to our hello world program and consider the steps for building it. Create project structure as follows:

    .
    ├── build
    │   └── classes
    └── src
        └── Hello.java

To build just run: javac -s src -d build/classes src/Hello.java

**What about dependencies? (Maven basics)**

Testing - Junit, Mockito and unit/integration tests
---------------------------------------------------

Dependency Injection and Spring
-------------------------------

