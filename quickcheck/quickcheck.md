## Created a fork of a repository called QuickCheck-Manual

1. Clone the repository at [quickcheck-manual](https://github.com/jbelmont/quickcheck-manual.git)
2. Move into the directory with `cd quickcheck-manual`
3. Read the `README.md` for steps on installing stack and setting up the project
4. Run `stack setup`
5. Run `stack ghci`

Inside of the GHCI repl we will run the commands from the paper

```
ghci> import Test.QuickCheck
```

```
ghci> quickCheck prop_revApp
OK passed 100 tests.
```

Quickcheck just ran 100 tests on the function `prop_revApp`

The paper makes the assertion that QuickCheck changes the way you test code.

**With QuickCheck you focus on the properties for the code under test needs to satisfy.**

## Software Testing

`Dijkstra's`
> Program testing can at best show the presence of errors, but never their absence

Author states that
> Thus we can expect testing to be the main form of program verification fora long time to come—it is the only practical technique in most cases

The point is made that with a CI process in place you can automate testing in your code base but there is still a dilemma on how many test cases to write.

Do you write one test case or many test cases?

The case is made to introduce QuickCheck into your CI process as it capture the general case and can generate many test cases that would be time consuming to write by hand.

## Shrinking

QuickCheck has the ability to shrink failing test cases. 

```
ghci> quickCheck prop_balanced
Falsifiable, after 22 successful tests (shrunk failing case 10 times):
[Insert (-9),Insert 12,Insert 8,Delete]
```

> In practice, much time is devoted either to simplifying a failing case by hand, or to debugging and tracing a complex case to understand why it fails. Shrinking failing cases automates the first stage of diagnosis, and makes the step from automated testing to locating a fault very short indeed

## Quviq QuickCheck

Steps to Install Erlang QuickCheck
1. `brew install elixir`
2. Download QuviQ on [QuviQ Downloads](http://www.quviq.com/downloads/) Page
3. `cd eqcmini-2.01.0/eqcmini-2.01.0`
4. Run the command `sudo iex`
5. Run the command `:eqc_install.install()`
    1. QuickCheck should now be installed

* The author explains that although porting QuickCheck from Haskell to Erlang seemed like a difficult task
* Using Call By Name mechanism such as passing 0-ary (0 argument) parameters
    * Erlang supports first-class functions
    * Macros were used instead of first-class functions being passed around
        * Simple Interface with macros used to build QuickCheck can be inferred.

**Erlang quickcheck use is similar to haskell**

As a side note some JavaScript libraries have been created and inspired from QuickCheck Haskell, Erlang

[JsVerify](http://jsverify.github.io/)
[TestCheckJS](http://leebyron.com/testcheck-js/)

## State Machine Specifications

* Author makes the point that they had instances where side effects occurred frequently along with pure functions.
* Testing code with side effects is not an easy task.
* Author says they made the point to represent test cases symbolically

> The reason we chose a symbolic representation is that this makes it easy to printout test cases, store them in files for later use, analyze them to collect statistics or test properties, or—and this is important—write functions to shrink them.

* How powerful should the test cases be?

Author makes the distinction that they made their test cases simple by only adding a subset of commands.
It is more important to see the path of failure than try to glean all possible paths.
Which is why they iterated on the design.

> During test  generation, the values returned by commands are unknown, so they cannot be used directly in further commands—yet we do  need to  generate commands that refer  to  them. The solution, of course, is to let symbolic test cases bind and reuse variables.

* References for Staged Programming and State Machines:
    * [Staged Programming](http://web.cecs.pdx.edu/~sheard/staged.html)
    * [State Machines](https://en.wikipedia.org/wiki/Finite-state_machine)

Author mentions that they utilitize callbacks in Erlang for their their state machine.