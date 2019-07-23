
## Overview

Shell scripts are lightweight scripts that help us automate system tasks. Scripts are interpreted - this means we scan and execute line by line. They usually have the .sh extension. Simply put, shell scripts are a combination of commands with some logic.

A simple hello world script is as follows:

    echo hello world

Save this as HelloWorld.sh. To run the script simply type the following command in the terminal:

    sh HelloWorld.sh

**Variables and Datatypes**

When it comes to shell scripts, **there is only 1 datatype - this is String**. Similar to languages like python or ruby, shell scripts are dynamically typed so variables can be assigned on the fly 

    - [Note: there should be no spaces between variables and assignment]. 

To reference variables we use thr ```$``` symbol. Below is a script where we declare a variable and print it on screen.

    #echo hello world
    x=10
    echo value of x is: $x

**I/O operations**

Write is done using the ```echo``` command as seen above.
Read can be done using the ```read``` keyword. This reads a variable from the command line.

A sample script is as follows:

    echo enter the value of x:
    read x
    echo the value of x is: $x

**Arithmetic operations**

The following **DOES NOT** work:

    echo Example
    x=20
    y=30

    c=$x+$y

    echo c is: $c

It prints out: ```c is: 20+30``` instead of 5. The reason is that shell scripts treat everything as strings - here in the case of x, it is giving x the value of String "20" and not Integer 20.

So to perform arithmetic operations we can use the ```expr``` command. 

    echo Example
    x=2
    y=3

    expr $x + $y

- [Note in ```expr $x + $y``` you need to leave whitespace]

This performs the add operation correctly.

To store the value of a command such as ```expr $x + $y``` in a variable we use the backtick command ( ` ).




