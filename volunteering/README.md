
## Course Content (tbd)

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

to initialize repo do the following:




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
