---
title: "Dynamic Programming"
date: "2024-03-04"
description: ""
categories: ["Algorithm"]
---

## Definition
> Dynamic Programming (DP) is defined as a technique that solves some particular type of problems in Polynomial Time. 
> Dynamic Programming solutions are faster than the exponential brute method and can be easily proved their correctness.


## Dynamic programming works on following principles:
- Characterize structure of optimal solution, i.e. build a mathematical model of the solution.
- Recursively define the value of the optimal solution.
- Using bottom-up approach, compute the value of the optimal solution for each possible sub-problems.
- Construct optimal solution for the original problem using information computed in the previous step.


## What is the difference between a Dynamic programming algorithm and recursion?
- In dynamic programming, problems are solved by breaking them down into smaller ones to solve the larger ones, 
while recursion is when a function is called and executed by itself. 
While dynamic programming can function without making use of recursion techniques, 
since the purpose of dynamic programming is to optimize and accelerate the process,
programmers usually make use of recursion techniques to accelerate and turn the process efficiently.

- When a function can execute a specific task by calling itself, receive the name of the recursive function.
In order to perform and accomplish the work, this function calls itself when it has to be executed.

- In dynamic programming, you can break a problem into smaller parts, called sub-problems, to solve it.
Dynamic programming involves solving the problem for the first time, then using memoization to store the solutions.

- Recursion is used to automate a function, whereas dynamic programming is an optimization technique used to solve problems.

- Recursive functions recognize when they are needed, execute themselves, then stop working.
When the function identifies the moment it is needed, it calls itself and is executed; 
this is called a recursive case. As a result, the function must stop once the task is completed, known as the base case.

- Dynamic programming recognizes the problem and divides it into sub-problems in order to solve the whole scene. 
After solving these sub-problems, or variables, the programmer must establish a mathematical relationship between them.
These solutions and results are stored as algorithms, so they can be accessed in the future without having to solve the 
whole problem again.


## Techniques to solve Dynamic Programming Problems:
### Top-Down(Memoization):
Break down the given problem in order to begin solving it. If you see that the problem has already been solved,
return the saved answer. If it hasn’t been solved, solve it and save it. This is usually easy to think of and very
intuitive, This is referred to as Memoization.

### Bottom-Up(Dynamic Programming):
Analyze the problem and see in what order the sub-problems are solved, and work your way up from the trivial 
sub-problem to the given problem. This process ensures that the sub-problems are solved before the main problem. 
This is referred to as Dynamic Programming.


## Tabulation(Dynamic Programming) vs Memoization:
|                     | Tabulation                                                                                                                                                         | Memoization                                                                                                                                                                     |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| State               | State transition relation is difficult to think                                                                                                                    | State Transition relation is easy to think                                                                                                                                      |
| Code                | Code gets complicated when a lot of conditions are required                                                                                                        | Code is easy and less complicated                                                                                                                                               |
| Speed               | Fast, as we directly access previous states from the table                                                                                                         | Slow, due to a lot of recursive calls and return statements                                                                                                                     |
| Sub-problem solving | If all sub-problems must be solved at least once, a bottom-up dynamic programming algorithm usually outperforms a top-down memoized algorithm by a constant factor | If some sub-problems in the subproblem space need not be solved at all, the memoized solution has the advantage of solving only those sub-problems that are definitely required |
| Table entries       | Starting from the first entry, all entries are filled one by one                                                                                                   | All entries of the lookup table are not necessarily filled in Memoized version. The table is filled on demand                                                                   |
| Approach            | iterative approach                                                                                                                                                 | recursive approach                                                                                                                                                              |

## Greedy approach vs Dynamic programming
| Feature                | Greedy method                                                                                              | Dynamic programming                                                                                                                     |
|------------------------|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Feasibility            | we make whatever choice seems best at the moment in the hope that it will lead to global optimal solution. | we make decision at each step considering current problem and solution to previously solved sub problem to calculate optimal solution . |
| Optimality             | sometimes there is no such guarantee of getting Optimal Solution.                                          | will generate an optimal solution as it generally considers all possible cases and then choose the best.                                |
| Recursion              | follows the problem solving heuristic of making the locally optimal choice at each stage.                  | based on a recurrent formula that uses some previously calculated states.                                                               |
| Memoization            | more efficient in terms of memory as it never look back or revise previous choices                         | requires table for Memoization and it increases it’s memory complexity.                                                                 |
| Time        complexity | generally faster.                                                                                          | generally slower                                                                                                                        |


## How to solve a Dynamic Programming Problem?
### We need to check two necessary conditions: 
#### Overlapping Subproblems
When the solutions to the same subproblems are needed repetitively for solving the actual problem. 
The problem is said to have overlapping subproblems property.
#### Optimal Substructure Property:
If the optimal solution of the given problem can be obtained by using optimal solutions of its subproblems 
then the problem is said to have Optimal Substructure Property.
### Steps to solve a Dynamic programming problem:
#### Identify if it is a Dynamic programming problem.
- Typically, all the problems that require maximizing or minimizing certain quantities or counting problems 
that say to count the arrangements under certain conditions or certain probability problems can be solved by
using Dynamic Programming.
- All dynamic programming problems satisfy the overlapping subproblems property and most of the classic 
Dynamic programming problems also satisfy the optimal substructure property.
Once we observe these properties in a given problem be sure that it can be solved using Dynamic Programming.
#### Decide a state expression with the Least parameters.
Problems with dynamic programming are mostly concerned with the state and its transition. 
The most fundamental phase must be carried out with extreme care because the state transition depends on the 
state definition you select.
>State: A state is a collection of characteristics that can be used to specifically describe a given position or standing in a given challenge. To minimise state space, this set of parameters has to be as compact as feasible.
#### Formulate state and transition relationships.
The hardest part of a Dynamic Programming challenge is this step, which calls for a lot of intuition, observation, and training.
#### Do tabulation (or memorization).
































