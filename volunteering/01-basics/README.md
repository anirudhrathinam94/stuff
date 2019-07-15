
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
### Ant basics

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
### Maven basics

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

**Structure of pom.xml**

Pom (project object model) is the default script used by maven. It is similar to build.xml used in Ant. The basic structure is as follows:

It has a root project tag. The first group of tags enclosed in the project are the 4 coordinate tags. They are:

- The artifactId tag
    - Here we mention the application name (Hello World App)
    - According to the maven docs this should be the name of the jar without the version name
- The groupId tag
    - Here we mention a unique identifier in the convention com.domain_name
    - This is something that identifies your project uniquely across all projects. 
    - It should be a domain you control and can have as many subgroups as you want like com.apache.maven or com.apache.commons etc
- The version tag
- The package tag
    - Specifies the type of package such as jar

The next part contains library dependencies. Maven has a reference of libraries. It gets then and makes use of them.

- Here we have the dependencies tag
    - The dependencies tag may enclose one or more dependency tags
- Inside each dependency tag we mention the individual dependency details.
    - These are the coorinates of each dependency (artefaceId, groupId, version, package of each individual dependency)

These are the 2 main parts of the pom.xml. We also have 2 other parts which are:

- build tag: Encloses plugins. Plugins can be used to run the extra stuff - things like shell scripts, perl scripts etc
- repository tag: Specifies remote maven repo that the company uses.

An example of a maven script is below:

    <project> 

        <artefactId>Hello World</artefactId>
        <groupId>com.abc</groupId>
        <version>1.0</version>
        <package>jar</package>

        <dependencies>

            <dependency>
                <artefactId>Hello World Dep 1</artefactId>
                <groupId>com.dom1</groupId>
                <version>1.0</version>
            </dependency>

            <dependency>
                <artefactId>Hello World Dep 2</artefactId>
                <groupId>com.dom2</groupId>
                <version>1.0</version>
            </dependency>

        </dependencies>

    </project>

**Lifecycle of maven script**

This describes how maven takes care of what you want. There are 3 life cycles for maven. They are:
- Default: called using mvn phase_name. It is explained in detail below
- Clean: called using mvn clean - this lifecycle deletes target directory after completion
- Site: called using mvn site - this generates a documentation of the source code others can see.

Of these default is the most common one. Each life cycle has phases. A phase is similar to Ant's target - it is a set of activities that you perform in that state of time. Like target each phase can have multiple tasks. The phases of the default life cycle are as follows:

    validate -> compile (compile src) -> test-compile -> test (run testcases) -> package -> integration-test -> verify -> install -> deploy

Note that there are 14 phases but these are the most important ones.

In the pom.xml above, we did not describe the phases. So how to call each stage of the lifecycle? We simply do: mvn life_cycle_name. So for example to compile we do:

    mvn compile

Now suppose we call mvn package:
    - Maven will automatically call the previous steps in the life cycle - these are called the same way as ant dependencies are called.
    - So if package is called, we call previous steps like validate, compile, compile testcases, tests and only after this is done, package is called.
    

The phases are as follows:
- validate: this is a phase where all the dependency jars (dependency artifacts) as described in the dependency tag are downloaded and stored in the developer's local machine.
    - These dependencies are used in the next phases for compiling the code
- compile: Here we do the compilation of the source code (project/src/main/java_code)
- test-compile: Here we do the compilation of the unit tests (project/src/tst/unit_tests)
- test: run testcases
- package: Creates a jar from the compiled code
- integration-test: Process and deploy the package if necessary into an environment where integration tests can be run
- verify: Runs checks to verify if the package is valid
- install: Installs the package into the local repo where it can be used as a dependency in other projects locally. Basically just copies the jar file created in target file into the local .m2 repository.
- deploy: This pushes the created jar files into the remote repository (example: nexus maven repo hosted on tomcat) where the jar can be shared with other devs.


**The maven file structure** 

Maven has its own file structure that we have to follow. We can create one ourselves using the archetype command. To generate the file structure use the command: 
    
    mvn archetype:generate

If you want to create a default simple archetype and not use some other template just do the following (for the purposes of the demo I will be using this)

    mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4
    
    ...
    
    Define value for property 'groupId': com.mycompany
    Define value for property 'artifactId': App 
    Define value for property 'version' 1.0-SNAPSHOT: : 1.0
    Define value for property 'package' com.mycompany: : jar
    Confirm properties configuration:
    groupId: com.mycompany
    artifactId: App
    version: 1.0
    package: jar
     Y: : y

This will create a file structure as follows:

    .
    └── App
        ├── pom.xml
        └── src
            ├── main
            │   └── java
            │       └── jar
            │           └── App.java
            └── test
                └── java
                    └── jar
                        └── AppTest.java

**Dependencies and the maven repository**

If you look at the pom.xml, junit is listed as a dependency. We will now see how maven downloads it. Cd into the App directory. Then use the mvn compile command

Maven now generates class files into a target folder. The structure is now:

    .
    └── App
        ├── pom.xml
        ├── src
        │   ├── main
        │   │   └── java
        │   │       └── jar
        │   │           └── App.java
        │   └── test
        │       └── java
        │           └── jar
        │               └── AppTest.java
        └── target
            ├── classes
            │   └── jar
            │       └── App.class
            ├── generated-sources
            │   └── annotations
            └── maven-status
                └── maven-compiler-plugin
                    └── compile
                        └── default-compile
                            ├── createdFiles.lst
                            └── inputFiles.lst

So where is the junit library downloaded? It is clearly not in the project so where can it be? This is downloaded into the local machine. Maven is required to have a local repository on the machine where it stores all dependencies. You can find this in **~/.m2/repository/junit** folder.

Here .m2 is the local repository where maven keeps all the dependencies. This is great because if we already have a dependency that maven has already downloaded, it will not redownload it again.

If you run mvn package, the jar is created in target with the name: App-1.0.jar. We can now run integration tests, verify, install and deploy our app. Similarly when you package a webapp a war file is created that includes things like web.xml and the web inf files. Note: web.xml does url mappings. Now we can use annotation based mapping instead. These packages can be used as dependencies by other devs.

We have the following types of repositories:

1. Local m2 repo: This is the local repository
2. Central repo: This is maintained by apache and contains common jars like spring, junit etc
3. Remote repo: This is maintained by the company where individual devs can share their jars to be used as dependencies.

So now the task of the dev ops guys within the company is to set up the remote repository.

**Remote maven repository**

There exist various tools to set up remote maven repositories. Some of these tools include Nexus, Artifactory, Archiva etc. Here we will take a look at setting up Nexus.

A step by step guide for setting up a remote maven repo is in the setup folder - use this link:

https://github.com/anirudhrathinam94/stuff/blob/master/volunteering/setup/01-nexus-remote-maven-repo.md

In the pom.xml, the **repositories** tag points to the remote repo that we created. In the repositories tag we mention 2 repositories they are:

1. repository: this is the release repository
2. snapshotRepository: this is the snapshot repository that is used during development.

Each individual repository tag has 3 important subtags/attributes:

- id: this is the repository id
- name: this is the name of the repository
- url: this is the repository url

An example is shown below:

    <repositories>
        <repository>
            <id>repoR</id>
            <name>repoR</name>
            <url>http://localhost:8080/nexus/content/repositories/repoR</url>
        </repository>

        <repository>
            <id>repoS</id>
            <name>repoS</name>
            <url>http://localhost:8080/nexus/content/repositories/repoS</url>
        </repository>
    </repositories>
    

Now if we run: mvn clean deploy ... it fails!

Note: If you look at the error message it is connected to the release repository not the snapshot repository. In the version tag, if the version is 1.0 it is sent to release. If it is 1.0-SNAPSHOT only then it will be sent to snapshot repository.

The failure error code is 401: Unauthorized - we do not have permissions to deploy to the server. To do this, we need to give permissions to the developer to access the repository. This is done by adding the **settings** tag to the pom.xml

- Here we provide the username and password for the nexus server

An example of this is as follows:

    <settings>
        <servers>
        
            <server>
                <id>repor</id>
                <username>admin</username>
                <password>admin123</password>
            </server>

            <server>
                <id>repos</id>
                <username>admin</username>
                <password>admin123</password>
            </server>

        </servers>
    </settings>
    
Now if you run mvn clean deploy, it gets deployed to the apache server.

**Plugins and profiles**

**Plugins** can be used to do all the extra stuff. As an example let us run a hello world on the console using a maven project. Maven by default does not support execution so we need to do it through a plugin. Just use google to find the plugin you want.

**Profiles** are used to separate environments. For example developers may use dev resource files, qa may use qa resource/config files and when the app is released in prod, we make use of prod config files.

So if you want to separate dev, test and prod we would normally have to create 3 separate pom.xml files. However if we want a single pom.xml for this, we use profiles. I can create separate profiles for dev, test and prod. Then I can call the appropriate build by doing:

    mvn -P DEV
    mvn -P TST
    mvn -P PROD

So to create a profile do:

    <profiles>
        
        <profile>
            <id>DEV</id>
            <build> <plugins> ... </plugins> </build>
        </profile>
        
        <profile>
            <id>TST</id>
            <build> <plugins> ... </plugins> </build>
        </profile>
        
    </profiles>

This way when we run: mvn -P DEV, the dev plugins are run. When we run: mvn -P TST, the test plugins are run. In order to make one profile the default (say dev) you can do:

    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <env>dev</env>
            <url>http://localhost:8080/manager</url>
            <server>nnadbmon-dev-tomcat</server>
        </properties>
    </profile>


Continuous Integration
-----------------------

Let us consider why CI came to be

- Let's assume team X works on project A, team Y works on project B and so on.
- X would make assumptions on how project B was developed and develop based on those assumptions
- When it was time to integrate packages A and B to get the fully functional product, there would be numerous bugs that would push back the date of release.
    - Dealing a large number of bugs at once is not great
- Hence we have continuous integration. Here every time a small change is made, it is immediately integrated and deployed.
- This way if a change is made, devs have to deal with fewer bugs at a time and this greatly reduces the overall development time.

So if we follow this methodology, our job is to get the code from the VCS, build it etc at regular intervals (per day, per commit etc). So the question is how to schedule it?

One way to do it is to write a script that takes in packages as input and schedule it using cron but doing this all the time takes a lot of time and effort. So to schedule it we have certain tools like CruiseControl, Hudson, Jenkins, TeamCity, BuildForge, Bamboo etc. Here we will use Jenkins.

Jenkins is an open source platform independent webtool (war file) that you can host on an application container like tomcat. To install all you need to do is:
        
    Step 1: Setup Tomcat application container
    Step 2: Deploy Jenkins

**Jenkins Architecture and setup**

Jenkins is hosted on the server. Once you choose your server, you choose an app container like tomcat. Here the jenkins app is hosted. This is called master.

Now a problem arises: What if we have several jobs (each job contains steps like get code, build, run tests, report generation etc)? If we have too many jobs the master alone cannot handle everything. This is solved by attaching nodes to the master also called slaves. This way jobs can be run in parallel. Jenkins also gives you a way of distributing the build load from the master.

The best practice is to use master to only host jenkins do not use master to run tasks. To run tasks just use the slaves. The setup is as follows:
    
    1. Have a server ready
    2. Go to jenkins website and wget the link: wget <link_name>
    3. Go to tomcat and wget the link: wget <link_name>
    4. Install java: apt get install java
    5. Start tomcat and jenkins. 
        You should be able to see them by going to https://<server_ip>:<port_no>/jenkins|tomcat
    6. 
    

















