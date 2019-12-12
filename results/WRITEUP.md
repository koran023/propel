# Next-Largest Prime
_Avery Koranda_

## Overview

This is an attempt to use Propel to solve the following problem: given a prime number P, return the next largest prime.

I chose this problem because, while the problem space is fairly simple and straightforward, the problem itself is not trivial and seems like a good opportunity to explore what EC is capable of doing. Creating a fitness function should be relatively simple, and generating test cases is trivial. This will allow me to focus on making small changes to the setup and recording the different impact those changes have.

## Initial Setup

### Operators & Constants

Since this problem is dealing strictly with integers, I am starting with utilizing only the exec, integer, and boolean stacks.

Here is the initial set of operators and constants I am providing:

```
(def default-instructions
  (list
   'in1
   'integer_+
   'integer_-
   'integer_*
   'integer_%
   'integer_=
   'exec_dup
   'exec_if
   'boolean_and
   'boolean_or
   'boolean_not
   'boolean_=
   'close
   true false
   0 1 2 3 4 5 6 7 8 9))
```

### Fitness Function

The most concise fitness function I can see for this problem is the following:

```
fitness(attempt) = | attempt - correct |
```

`attempt` is the answer given by the program, while `correct` is the correct answer from the test case.

This provides a gradient that rewards programs for being close to the answer in a linear manner.

Possible things to tweak here are penalizing numbers that are divisible by 2, 3, and 5, since divisibility by any of those is an easy check, as well as rewarding programs if the number returned is prime at all.

### Test Case Generation

Initially, a random test case will be provided for every individual. This is one dimension that may be useful to explore to see how the provided test cases impact how quickly a solution is found, as well as how versatile/complete the given solution is.

### Evolution Environment

The initial values defining how propel will evolve are as follows (mostly default from NicMcPhee's fork of Propel):

```
:instructions default-instructions
:error-function nlp-error-function
:max-generations 500
:population-size 200
:max-initial-plushy-size 50
:step-limit 100
:parent-selection :tournament
:tournament-size 5
```

## Results

### Initial Run
_See runs/run1.txt for log output_

Propel ran for a full 500 generations without finding a solution. With 20 test cases per individual, some generations produced an error as little as ~24.

I haven't seen any error vectors that have a mix of both odd and even errors; they seem to be all one or the other.

**Changes for Next Run:**
* Include `prime?` as an operator