

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
- less: same as more but with extra capabilities - navigate up/down, can search for string using /keywordto command and supports vi commands
- head -50: shows first 50 lines. It can be changed to any number
- tail -20: shows last 20 lines. It can be changed to any number

**Command to write is:**

- vi: Lets you perform write operation using the vi editor
  




