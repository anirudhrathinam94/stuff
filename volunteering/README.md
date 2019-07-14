
# In this repo

0. Setup: Contains step by step guides on setting stuff up this includes things like: 
    - setting up an internal git remote repo for company use 
    - setting up a remote maven repo through nexus where devs can share their packages/artifacts
1. Basics:
    - This has some dev ops stuff and explains how source code is built and deployed
    - Covers things like ant, maven, nexus, jenkins etc (to be updated)








---------------------------------------

# Notes and general content (tbd)

Theory/Revision
---------------
- Java basics (spring, design patterns)
- Version control
- TDD basics
- Intro to git, unix etc
- **Note to self:** Remember to ask Adam if leetcode/interview prep needs to be covered as well


Git 
----

sudo apt-get install git/brew install git

git config --global user.name "name"
git config --global user.email "email"

**To initialize repo do the following:**

cd to workplace
git init

**Connecting to remote**

in this case remote will be github
git remote add origin <link>

**Using git**

good practice to checkout to new branch and develop from there in case revisions need to be made
be sure to pull and rebase before push
can use tig to see commits


Writing code
------------

Pick minor project. Workflow as follows:

    write integration test
    while integration test fails do:
        write unit test to fulfill partial behavior
        while unit test fails do:
            write code to make unit tests pass
        commit
        refactor // if needed
        while unit test fails // due to refactoring
            write code to make unit test pass
        commit
    push

Devops/deployment
-----------------

- Docker
- CI/CD workflow
- AWS
- Kubernetes
