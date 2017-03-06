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