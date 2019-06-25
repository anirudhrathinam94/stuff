
Environment
-----------

It is whatever you need to write and execute a program on your computer. You can split it into 3 types:

- Development environment: Everything you need to write the code. Code editors, ides etc
- Build environemnt: Given some code it is everything you need to compile it and make an executable
- Runtime environment: Everything you need to execute a program.

Let us consider a simple hello world program: The dev env is the editor/ide we used, to build it we had to use javac <Filename.java> and to run it we had to run java <Classname>.

Note: Show students my own dev env here + dot files and scripts. Also if they use windows/mac tell them about cygwin, homebrew etc in case they dont use it.

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

Create file structure as follows:

    .
    ├── build
    │   ├── classes
    │   │   └── tutorial
    │   │       ├── Hello.class
    │   │       └── Statement.class
    │   └── jar
    └── src
        ├── Hello.java
        └── Statement.java

Run the following command to create the jar

    jar cf build/jar/Hello.jar -C build/classes/tutorial .
    
Now our job is to run this. If we try to run the jar using the following command:

    java -jar build/jar/Hello.jar

We get an error saying no main manifest attribute, in build/jar/Hello.jar. This is because there are 2 types of jars - Executable and Non Executable jars. This is non executable. We can draw parallels in the windows world where we have .exe and .dll both are a collection of classes but .exe is executable, .dll is non executable. However in java both executable and non executable have the .jar extension.

In order to make a .jar executable, we need somthing called a manifest file.

    echo Main-Class: tutorial.Hello>myManifest

Then run the following to create the executable jar:

    jar cfm build/jar/Hello.jar myManifest -C build/classes .

By looking at the jar file name you cannot tell if it is an executable but if you try running it, it gets executed successfully.

Folder structure is shown below:

    .
    ├── build
    │   ├── classes
    │   │   └── tutorial
    │   │       ├── Hello.class
    │   │       └── Statement.class
    │   └── jar
    │       └── Hello.jar
    ├── myManifest
    └── src
        ├── Hello.java
        └── Statement.java

We had to do all this crap just to run a simple hello world program. Bigger projects can get far more complicated. When we deploy apps to the server, we need an easier way to build files so they can be executed. Doing it manually is time consuming (for larger projects) and error prone. 

This is where the need to automate the build process arises. There are various tools to automate the build process such as ANT, Maven, Gradle etc.

----------------------------------------
**Ant basics**

Ant is an XML based scripting language. To install and all you need to do is: 

- download and extract the ant archive 
- set the path variable (which lets us use the ant command in the extracted directory).
- Your package manager can take care of this for you (sudo apt-get install ant or brew install ant)

Now that ANT is setup, we need to automate the build process. We do so by writing an XML file and pass that file via the ant command. We simply do: ant build.xml and ANT will do everything for us including build, compile, execute, package etc.

Ant needs a build.xml file to do its thing. This section briefly describes the build.xml file and how it works.

----------------------------------------
**Maven basics**

Testing - Junit, Mockito and unit/integration tests
---------------------------------------------------

Dependency Injection and Spring
-------------------------------

