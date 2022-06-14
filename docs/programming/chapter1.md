# Unit Tests :D

## Effective Unit Testing by Eliotte Rusty Harold

### Why do we write our tests ?

* We want our code to work
* We want to keep it work
* We make mistakes
* To code faster, with more confidence and fewer regressions

### What is a Test

> Verify that a known, fixed input produces a known, fixed output

Test like `sqrt(9) should be 3`

## Guides

Eliminate everything that makes input or output unclear.

Don't use constant from the main source code to prevent changes of tests from main source code

Get away from ousitde dependency: network access, file system, the time and other external things \(the gravitiy :p\), multithreading

One assert per test

Unit also means **Independent** from each others

More than one test class to a source class if neccessary

Success tests should not proudce any output \(logs ... etc\)

Failing tests should produce clear ouptut to help you debug

Don't use same data in every tests

Be aware of system skew like os system dependenet assumptions, floating point roundoff, integer width, default encoding

Avoid conditional logic in tests, make them two separate tests

In case of bug write the test first to be sure you understand the bug.

Write test before refactoring

