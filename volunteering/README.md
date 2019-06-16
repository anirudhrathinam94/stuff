
## Course Content (tbd)

Theory/Revision
---------------
- Java basics (spring, design patterns)
- Version control
- TDD basics
- Intro to CLI and unix etc

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
