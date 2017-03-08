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

Steps for State Machine Logic:

1. Command and PreCondition
    1. Used to both generate and shrink test cases
2. PostCondition
    1. Used during test execution to check the result of each command and that it satisfies the properties
3. Initial state and next state
    1. Used during generation and test execution to keep track of the state of the test case.

## Ericsson’s Media Proxy

> Even if the corruption was survivable in the normal case, it is obviously undersirable for a system to corrupt its data. 
If nothing else, this is a trap lying in wait for any future developer modifying the code. 
It is interesting that QuickCheck could reveal this fault, despite knowing nothing at all about the Proxy’s internal data.

#### Formulate bug preconditions:

* Helps formulate a hypothesis about when the bug appears.
* Test hypothesis by verifying that the precondition fixes the bug
* Document the bug by using a precondition 

#### Test Case Wisdom
* During regular test case scenarios in unit-testing your follow the happy path or normal path
    * This in turn forms basis for future test cases
* By generating test cases you can find bugs faster and more accuracy is what I am gleaning from the paper

**Benefits of QuickCheck Testing**

* Bug Preconditions help clarify business rules

## Concurrency

Author makes the point in the paper that although testing Concurrent programs is difficult it is still useful.
QuickCheck helped track down errors in erlang code and promoted a redesign of the "distributed process registry"

> Thus another way to use QuickCheck is to gain confidence that a formally verified algorithm has been correctly transferred to a real situation!

## Testing Imperative Code

Author makes the point that you can test Imperative code with QuickCheck.

* Use an interpreter for API calls
    * link the interpreter with the code under test and run in a separate process
    * QuickCheck can then be used to generate sequences of call that are sent to the interpreter and to check the results that are sent back.

> Indeed, it is already quite common to use a separate test scripting language, different from the implementation language of the code under test—so why shouldn’t that language be declarative.

## Erlang vs. Haskell

> We initially expected an Erlang version to be a little clumsier to use than the Haskell original, because of the lack of lazy evaluation, Haskell’s type system,and monadic syntax.

Author is basically making the point that Haskell's Powerful type system and Monad Syntax was thought to be best suited for QuickCheck.

> Quviq QuickCheck permits any data-structure containing embedded generators to beused as a generator for data-structures of that shape—something which is very convenient for users, but quite impossible in Haskell, where embedding a generator in a data-structure would normally result in a type error. 

**Quviq QuickCheck is the Erlang implementation of QuickCheck**

> In Erlang, we could represent a call just by two atoms and a (heterogenous)argument list, and provide a single generic interpreter run_commands, thanks to Erlang’s ability to call a function given only its name and arguments.

## Discussion

Author makes the point here that it is better to run smaller tests than large tests. Most errors can be found by a smaller test case.
QuickCheck also is better utilized with small test cases.

* Developer will jump onto the first failing case
* Rerun the test case and start debugging the issue
* Test cases generated by hand are time consuming as well
* When new test cases can be generated by hand in seconds it helps reduce developer time on trivial edge cases.