
## Overview

Shell scripts are lightweight scripts that help us automate system tasks. Scripts are interpreted - this means we scan and execute line by line. They usually have the .sh extension. Simply put, shell scripts are a combination of commands with some logic.

A sample hello world script is as follows:

    echo "hello world"

Save this as HelloWorld.sh. To run the script simply type the following command in the terminal:

    sh HelloWorld.sh

**Variables and Datatypes**

When it comes to shell scripts, **there is only 1 datatype - this is String**. Similar to languages like python or ruby, shell scripts are dynamically typed so variables can be assigned on the fly [Note: there should be no spaces between variables and assignment]. To reference variables we use thr ```$``` symbol.

Below is a script where we declare a variable and print it on screen.
