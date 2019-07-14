
Environment
-----------

It is whatever you need to write and execute a program on your computer. You can split it into 3 types:

- Development environment: Everything you need to write the code. Code editors, ides etc
- Build environemnt: Given some code it is everything you need to compile it and make an executable
- Runtime environment: Everything you need to execute a program.

Let us consider a simple hello world program: The dev env is the editor/ide we used, to build it we had to use javac <Filename.java> and to run it we had to run java <Classname>.

Note: Show students my own dev env here + dot files and scripts. Also if they use windows/mac tell them about cygwin, homebrew etc in case they dont use it.

Building code
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

**build.xml**

A basic build.xml has the following structure:

- We have the project tag that encloses everything. This is the root tag.
- Inside the project tag we have the target tag
    - In a project.xml we can have multiple targets
    - A target is just a group of tasks or action items you can perform
    - Every target may have multiple tasks
- A task represents an individual action item. For example compile the code, create dir etc are actions
    - Note: You cannot keep tasks outside the target tag

The tags and attributes are described below:

- Project:
    - name: name of project
    - default: default target to execute. Note that you can give only one target
    - basedir: where exactly the project should be run (example c drive)
- Target:
    - name: name of target (mandatory)
    - depends: dependency targets. If 'A' is called by project.default, target A first looks in the depends tag and executes that first
    - description: description of target
- Task:
    - An actual action performed. **Note: There is no standardized task tag. Tasks have different tags depending on the task you want to perform.** We have n number of task tags.
    - To compile java code we use <javac>, to make new directory we use <mkdir>, to delete dir we use <delete> and so on.
    - The rest of ant is about learning these tasks.

An example build.xml ant script skeleton is:

    <project name="tutorial" default="A" basedir=".">
      <target name="A" depends ="B">
          <echo>Target a task 1</echo>
          <echo>Target a task 2</echo>
      </target>
      <target name="B">
          <echo>Target b task 1</echo>
      </target>
    </project>

Here the task is echo. The terminal output on running this is:

    B:
         [echo] Target b task 1

    A:
         [echo] Target a task 1
         [echo] Target a task 2

----------------------------------------
**Maven basics**

This was developed to deal with the issues/concerns associated with ant. Some of the limitations of ANT are as follows:

- In the build.xml we had to describe what to build (what to compile) and how to run (using javac). We had to explicitely describe the ordering of dependencies (prepare folders -> then compile -> then package -> then run).
    - For shorter projects this is fine but for larger projects it can become far too long and complex as we have a single script where all the info is stored.
- Ant scripts are not standardized. 
- Dependent libraries that are part of source code so there is a coupling between dependencies and source code.
    - Suppose we have 3 modules we want to use. We have mod1/lib/12 jars, mod2/lib/10 jars, mod3/lib/8 jars
    - Of them suppose 5 jars are common, it does not make sense to have duplicates
    - Maven helps manage dependencies. It also helps separate libs from src code.

Some advantage of Maven are:

- In ant we say what we want to do and how we want to do it. In maven we only say what we want to do. Maven must figure out how to do things automatically. For example we say compile the program but we dont want to write the script telling how to compile like in ant.
- Maven will follow a default sequence of execution. We do not have to define it like we did in ANT. Maven lifecycles are designed to perform tasks without human intervention.
- Maven lets us segregate libraries. We do not need dependencies in the src code to execute the source code.
    - Maven manages the dependencies for you
    - This means src code becomes lightweight and there is no duplication of libraries.
- In the devops side using maven means writing less scripts when compared to using ant because most things are auto generated.

As a build tool, maven can generate code, compile, package and deploy on the server. Note that because maven does a lot of work for us, we need to follow a default project structure. This is different from ant where we could have any random structure because we had to write and specify everything ourselves in the build.xml script.
    - The advantage of having a std structure is because having non standard ones can be error prone. For example if I decide to move a dir in the workspace or rename a folder things will not compile and I need to either change the script or debug the problem by looking into the script.





Testing - Junit, Mockito and unit/integration tests
---------------------------------------------------

Dependency Injection and Spring
-------------------------------

