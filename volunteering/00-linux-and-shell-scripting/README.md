

Overview
---------

Linux is a distributed OS based on Unix. This means that with linux installed multiple people can parallely access the system and work on it.

Your typical system looks like this:

![computer-layers](https://i.imgur.com/SaXIMOO.png?2)

Let us focus on layer 4. The **shell** is a program that takes some input from the keyboard and gives it to the OS to perform. In linux we have programs like bash or zsh that acts as the shell. In this layer we also have another program called the **terminal emulator**. This is a program that lets you interact with the shell.

In most servers we use the shell/terminal to do stuff because a lot of the system resources are consumed by the UI.

-----------------------------------------------------

Basics
------

If you want to access a remote linux machine you need: 1) Informantion about the machine - this includes the IP or hostname and the login credentials (user + pass) and 2) Depending on what system you are using you may need proxy tools like Putty to grant access.

- Note: To know the ip config just use the command use: ifconfig

If you need a tool called putty, download it and put in the ip address in the hostname field. Putty will connect to your server on a port number such as port 22.
  - A port is a socket on the server which is open to the external world to communicate to another machine.
  
Linux has a tree based file system. The admin is given access to the root directory (/) and can do whatever he/she wants. As a user you will be given your own account which is created in the home directory (home/user) -> here you have limited access and can only touch things under the user/directory.

As an admin you can add new users by doing the following as root:

- adduser <username>: this adds a new user <username>
- password <username>: this a new password for the user <username>
- su <username>: This can allow you to switch users (if you have appropriate permissions)

In linux you have commands that have the following format: command -option

----------------------------------------

### File navigation

**Commands are:**

- pwd: know your current location
- ls: list current dir. Flags listed below:
    - l: detailed view
    - a: show hidden
- cd: change directory

------------------------------------------

### File operations

**Basic commands are:**

- mkdir <name>: make dir
    - p: For the creation of nested directories
- cp: Used to copy
    - cp file.txt dir: Copies source file to destination directory
    - cp file.txt file-duplicate.txt: Makes duplicate of file
    - cp -r dir1 dir2 : Recursively copies dir1 and all its contents into dir2
    - cp *.txt dir: Copies all .txt files in current location to directory dir
- rm: Delete a file
    - rm -r dir: recursively delete directory and all its contents
    - rm -rf dir: recursively delete and dont ask for confirmation
- mv: move or rename file
    - mv hi.txt hello.txt: changes hi to hello
    - mv dir1 dir2: changes directory names (no need to give -r flag)

**Commands to read a file are:**

- cat: read contents of file and paste in terminal
- more: same as cat but view page by page (space to go to next page)
- less: same as more but with extra capabilities - navigate up/down, can search for string using /<keyword> command and supports vi commands
- head -50: shows first 50 lines. It can be changed to any number
- tail -20: shows last 20 lines. It can be changed to any number

**Command to write is:**

- vi: Lets you perform write operation using the vi editor
  
---------------------------------------------

### Other file commands

This section describes other file commands that can make life easier while working.

**Searching files**

- find: Name based search for a file among a list of files (similar to searching for a file in windows search box)
    - find . -name Hello.txt: Searches for file named Hello.txt among list of files in current (.) directory
    - find . -name H*: Searches for all files starting with H among list of files in current directory (* is wildcard)
    - find . -name \*.txt: Searches for all files ending with .txt in current directory (all text files)
    - find also has flags -size and -mtime for size/time based searches.

**Searching file content**

- grep: File content search. This searches for a given pattern (character string) in all the files present in the given directory (or file).
    - grep string file.txt: Searches for all instances of string in file.txt
    - grep string \*: Searches for string in all files in directory
    - grep -R string \*.txt: Recursively searches for string in all text files in current directory and all subdirectories.       - grep -R -n string \*.txt: The -n flag to also shows the line number.
    - grep -R -n -i string \*.txt: The -i flag ignores the case so we search for both String and string
    - grep -w string \*.txt: Searches for only one word (string) in all text files.

- wc: Counts number of lines, words and characters of file (-l, -w, -c flags) =
    - wc -l hello.txt: Prints number of lines in hello.txt

- cut: If we have a file in the form of a table and we want to return a specific column this is used.
    - cut f3 table.txt: Prints 3rd column in the table. Condition - columns must be separated by tabs. Here f is field separator
    - cut f1-3 table.txt: Prints columns 1-3. Condition - columns must be separated by tabs
    - cut -d" " f1-3 table.txt: Prints columns 1-3 where columns are separated by single space. -d is delimiter flag.
    - cut -d";" f1 table.txt: Prints first col of table where cols are separated by ;. 

- uniq: Helps eleminate duplicates - only shows unique files
    - uniq hello.txt: Eleminates consecutive dupliates to show only uniques. Running the command on:
    
          Hello
          hi
          hi
          Hello
          Hello
      
      Produces
          
          Hello
          hi
          Hello
          
- sort: sorts a file. Also works with strings and special characters (ASCII based sort)
    - sort file.txt: Sorts in ascending order
    - sort -r file.txt: Sorts in descending order

- sed: Used for find and replace
    - sed 's/,/%/g' hello.txt: finds string (s) ',' replaces it with string '%' in global (g) scope in file hello.txt
    - Note: This will not modify hello.txt. It just shows it on console. To save those changes just use -i command (or just do the same in vim). So why use sed when we have vi? It is because sed can be used for multiple files.
    - sed 's/,/%/g' \*.txt: Do sed operations on all text files.

- awk: Basically used to format the data and shows the output. 
    
    - For example suppose running cut -d" " f1-2 table.txt shows:

          ro1 ro2 
          ABC CSE
          XYZ IT
          
    - If we want to replace spaces with " | " for better formatting we can use the awk command.
    - awk -F" " '{print $1$2}' table.txt: The output is:
    
          ro1 ro2 
          ABC CSE
          XYZ IT
          
    - awl -F" " '{print $1" | "$2}' table.txt: The output is:
          
          ro1 | ro2 
          ABC | CSE
          XYZ | IT
        
------------------------------------------------

### Operators:

There are special operators you use. They are:

    >: this is the redirectional operator
    >>: this is the append operator
    |: this is the pipe operator

They are described as follows:

  - \>: Used to redirect output. For example running ```ls > hello.txt``` will print the contents of ls into hello.txt. Note that each time we do this, the file ```hello.txt``` is overwritten
  
  - \>>: This is the append operator. It is the same as above but instead of overwriting it appends.
  
  - |: Used for chaining/combining commands. Pipes the output of one command to another. Here doing ```c1 | c2``` means that output after running c1 is input of c2. Some examples are below:
       
    **Example 1: Removing duplicates**
    
    If we have:
    
        Hello
        Hello
        SCP
        BGM
        SCP
        BG<
        
    Running ```uniq file.txt``` produces the following. This still has duplicates as only consecutive duplicates are eleminated.
    
        Hello
        SCP
        BGM
        SCP
        BGM
    
    But if we run ```sort file.txt | uniq``` it produces:
    
        BGM
        SCP
        Hello

    **Example 2: print a specific row in a table** 
    
    If we have the following: 
    
        ro1 | ro2 
        ABC | CSE
        XYZ | IT
        
    And we only want to print the 2nd row we can do ```head -2 file.txt | tail -1```. This prints ```ABC | CSE```.

-----------------------------------------------------

### System commands


**System Processes**

- ps -aef: Lists all the processes. Shows User (UID), Process ID (PID), Parent Process ID (PPID - every process is forked from another process. 
    - The bash terminal is also a process - the command ps -aef that we ean is also a process whose parent is the terminal
- kill: kill the process. For example if you run ```sleep 300``` and now you want to kill it, **you can use Ctrl+z to push the process to the background**.
    - Then run ```kill <PID>``` to kill it.
    - kill -9 <PID>: This is force kill
    - Note: as regular user you cannot kill root processes because you dont have permissions


**Disk commands**

- df -h: Shows things like free disk space, used space, usage % etc
- du -sh hello.txt: Shows the space taken by hello.txt
    - You can also do ```du -sh *``` for all files.
- top: this shows the load on the CPU, which process takes how much memory etc

**Network commands**

- **Use case 1:** If we are running a server like apache on the linux machine and you are accessing s/w from some other application, it will open a port. A port is a socket connection to connect to the external world. So how do you see an open port in a given machine?
    - Run the command ```netstat -nl```. To check if port number 8000 is open you can simply pipe it into grep and see if an output is returned. Run ```netstat -nl | grep -1 8000```

- **Use case 2:** Let us assume we have 2 linux machines ans port 8000 of machine 1 is open. Is m1-8000 accessible by m2?
    - telnet <IP-address> <Port-number>: If running this makes the screen hang or does not show terminal, it is valid. If port not open we get message saying could not open connections


-----------------------------------------------------

### Machine to machine communication

- scp: This is secure copy. It helps us copy files between one machine to another machine.
    - To copy within the same machine the command is cp <filename> <destination>
    - To copy to different machine (machine B) your destination should have the username of machine B, the ip and the filepath
    - For example ```scp hello.txt user_machine_B @ 192.168.146.129: <path>``` Here 192.168.146.129 is IP of B
    - You need to provide the password of machine B (unless auto ssh is enabled) and we can transfer files.
    - ```scp -R <dir-name> <user>@<ip>:<path>```: Like copy the -R flag lets you copy entire directories.

- ssh: This is the secure shell command. This is used to run commands remotely - that is we can use machine A to run commands on machine B.
    - use ```ssh user@<ip-address>``` to ssh.
 
------------------------------------------

### File Permissions

When you do detailed ls (ls -l) you see a bunch of characters like this in the beginning ```drwxr-xr-x```. These represent the file permissions.

The format is divided into 4 groups as follows:

    _ | _ _ _ | _ _ _ | _ _ _ 

    0     1       2       3


**Group 0**: This is used to identify if the item is a file or directory.
  - -: means it is a file
  - d: means it is a directory

**Group 1**: This represents the author.

**Group 2**: This represents the group.

**Group 3**: This represents others.

Each of the groups have characters r, w, x
  - r is read permission. It is assigned the value 4.
  - w is write permission. It is assigned the value 2.
  - x is execute permission. It is assigned the value 1.
  
To change permissions we use the command ```chmod <number_for_author> <number_for_group> <number_for_others>```.

So running ```chmod 754``` means:

- Author has read, write and execute permissions
- Group has read and execute permissions
- Others only have read permissions








