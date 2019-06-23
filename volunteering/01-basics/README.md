
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

**How do we build programs?**

Let us go back to our hello world program and consider the steps for building it. Create a separate folder for all the class executables and build the file.
To build just run the following command:

    javac -s src -d build/classes src/Hello.java

This should leave you with a project heirarchy as follows:

    .
    ├── build
    │   └── classes
    │          └── Hello.class
    └── src
        └── Hello.java

To run the app, simply do:

    java -cp build/classes Hello
    
We have the desired output. Let us now define a Statement class in a new file that returns some string. We want to print this string out in the Hello Class.

The code for this is:

    package tutorial;
    class Statement {
        public String getString() {
            String s = "Something";
            return s;
        }
    }
    
    package tutorial;
    class Hello {
        public static void main(String args[]) {
            Statement s = new Statement();
            System.out.println(s.getString());
        }
    }

To compile do: 
    
    javac -s src -d build/classes src/Hello.java src/Statement.java

The project structure becomes (tutorial created as it is name of pkg):

    .
    ├── build
    │   └── classes
    │       └── tutorial
    │           ├── Hello.class
    │           └── Statement.class
    └── src
        ├── Hello.java
        └── Statement.java


However what if we have ten java files? or a hundred? or a thousand? We cannot run the command for all of them. Which is why we compile pakcages into jars. Think of a jar as a collection of class files.



**Need for automation (Ant basics)**

**What about dependencies? (Maven basics)**

Testing - Junit, Mockito and unit/integration tests
---------------------------------------------------

Dependency Injection and Spring
-------------------------------

