# Sudoku_Solver
## Sudoku
Sudoku is a number-placement puzzle where the objective is to fill a square grid of size ‘n’ with numbers between 1 to ‘n’. The numbers must be placed so that each column, each row, and each of the sub-grids (if any) contains all of the numbers from 1 to ‘n’.

The most common Sudoku puzzles use a 9x9 grid. The grids are partially filled (with hints) to ensure a solution can be reached.

![Alt text](https://hackernoon.com/hn-images/1*V6o3RVkDbHbwhR3lH_Aq7A.png)


An unsolved 9x9 Sudoku puzzle. The bold numbers are the hints.
And here’s the solution. Notice how each row, each column and each sub-grid have all numbers from 1 to 9. Some puzzles may even have multiple solutions.

![Alt text](https://hackernoon.com/hn-images/1*uT1D1ZgbzNuJU_Q_X1Tl4A.png)


The solution to above Sudoku puzzle. One row, column and sub-grid have been highlighted.
Aside: solving a Sudoku puzzle
Sudoku is a logic-based puzzle. Needless to say, solving one requires a series of logical moves and might require a bit of guesswork. Since this isn’t an article to explore how to solve a Sudoku puzzle, I’ll just a leave a link to one that helped me getting started: kristanix.com/sudokuepic/sudoku-solving-techniques.


## Backtracking
Backtracking is an algorithm for finding all (or some) of the solutions to a problem that incrementally builds candidates to the solution(s). As soon as it determines that a candidate cannot possibly lead to a valid solution, it abandons the candidate. Backtracking is all about choices and consequences.

Abandoning a candidate typically results in visiting a previous stage of the problem-solving-process. This is what it means to “backtrack” — visit a previous stage and explore new possibilities from thereon.

Usually, apart from the original problem and the end goal, we also have a set of constraints that the solution must satisfy.

The simplest (read ‘dumbest’) implementations often use little to no “logic” or “insight” to the problem. Instead, they frantically try to find a solution by guesswork.

A backtracking algorithm can be thought of as a tree of possibilities. In this tree, the root node is the original problem, each node is a candidate and the leaf nodes are possible solution candidates. We traverse the tree depth-first from root to a leaf node to solve the problem.


![Alt text](https://hackernoon.com/hn-images/1*TBN_HH658zzTtOQCW3g6zQ.png)
![Alt text](https://hackernoon.com/hn-images/1*MJ1Rhf0_xmeT2FJG8p_92Q.png)

Tree of Possibilities for a typical backtracking algorithm
The tree diagram also shows 2 groups — Unexplored Possible Candidates and Impossible Candidates.

UPC marks nodes that were never explored. Some of them could have been viable candidates, leading to another solution. Since we never explored them, we can never know. Problems where multiple solutions are acceptable won’t have this group.

IC groups are the obvious ones. It contains nodes which have a failed candidate node as one of their ancestor nodes. None of the nodes in this group are candidate nodes and none of the leaf nodes are solution nodes.

## Sudoku & Backtracking
We will now create a Sudoku solver using backtracking by encoding our problem, goal and constraints in a step-by-step algorithm.

## Problem
Given a, possibly, partially filled grid of size ‘n’, completely fill the grid with number between 1 and ‘n’.

## Goal
Goal is defined for verifying the solution. Once the goal is reached, searching terminates. A fully filled grid is a solution if:

Each row has all numbers form 1 to ‘n’.
Each column has all numbers form 1 to ‘n’.
Each sub-grid (if any) has all numbers form 1 to ‘n’.
Constraints
Constraints are defined for verifying each candidate. A candidate is valid if:

Each row has unique numbers form 1 to ’n’ or empty spaces.
Each column has unique numbers form 1 to ‘n’ or empty spaces.
Each sub-grid (if any) has unique numbers form 1 to ‘n’ or empty spaces.
Termination conditions
Typically, backtracking algorithms have termination conditions other than reaching goal. These help with failures in solving the problem and special cases of the problem itself.

There are no empty spots left to fill and the candidate still doesn’t qualify as a the solution.
There are no empty spots to begin with, i.e., the grid is already fully filled.
Step-by-step algorithm
Here’s how our code will “guess” at each step, all the way to the final solution:

Make a list of all the empty spots.
Select a spot and place a number, between 1 and ‘n’, in it and validate the candidate grid.
If any of the constraints fails, abandon candidate and repeat step 2 with the next number. Otherwise, check if the goal is reached.
If a solution is found, stop searching. Otherwise, repeat steps 2 to 4.
Pen-paper demo
Groovy. Now let’s try all this in practice with a simple 3x3 grid.

![Alt text](https://hackernoon.com/hn-images/1*dFNtAnfevAa9wP-VQ10oXQ.png)
A 3x3 Sudoku puzzle

We start off by listing all the empty spots. If we label each cell in the grid with a pair of numbers (x,y) and mark the first cell (1,1), then our empty spots will be at locations:

```(1,2) (2,2) (2,3) (3,1) (3,2)```
We now select the first spot (1,2) to work with. Since this is a 3x3 grid, we have numbers 1 to 3 at our disposal and no sub-grids to worry about (sub-grids are only a bother for grids with squared sides, like 4, 9, 16 etc.).

Let’s place number 1 in this spot and see if it fits.

![Alt text](https://hackernoon.com/hn-images/1*eh31JFMgAEq9hxjISklwIA.png)
Filling ‘1’ in spot (1, 2)

It does. Great. We can now select the next spot on the list (2,2) and do the same thing again. This time however, it fails. We already have a 1 in this row. This means that we must abandon candidate and repeat step 2 with the next number — which is 2.

![Alt text](https://hackernoon.com/hn-images/1*-ZfNe5ATHjC5ew08rlKweg.png)
Filling 2 in spot (2, 2)

Huzzah! One more spot is filled. Also, it might not look like it, but we did just perform backtracking on a single spot. We abandoned a candidate solution (1 at spot (2,2)), visited a previous stage (empty spot (2,2)) and explored a new candidate solution (number 2 at spot (2,2)).

When we move on to spot (2,3), we have another problem. As you can see, we are all out of options. None of the possible numbers fit in. This means that we must now abandon candidate and repeat step 2 with the next number. Only this time, we must visit spot (2,2) first to fix spot (2,3).

![Alt text](https://hackernoon.com/hn-images/1*_ImNrh84gLZVm3yTA1Wh0g.png)
Failure to fill spot (2, 3)

We need to fill number 3 in spot (2,2) and that will resolve the issue.

![Alt text](https://hackernoon.com/hn-images/1*6QuUVfx8BLw9XeaiTCCR8Q.png)
Backtracking to spot (2, 2) and then re-visiting spot (2, 3)

We now repeat this process until with either reach the goal or we hit one of the termination conditions.

Since this was a demo problem, it should be obvious that we’d arrive at the solution without any further complications.

![Alt text](https://hackernoon.com/hn-images/1*qcNunCrrxO4zERyRfwNaGg.png)
Solved 3x3 Sudoku puzzle

However, consider the same grid with one small change. Replacing the 1 in cell (3,3) with a 2 renders the grid unsolvable. Similarly, removing hints from cells (2,1) and (3,3) allows for multiple solutions. But since this algorithm has a single goal, it stops after the first solution is reached.
![Alt text](https://hackernoon.com/hn-images/1*GOsXsydaPEZXExIKw-Wrxw.png)
Unsolvable 3x3 puzzle (left) and Multi-solution 3x3 puzzle (right)


Here’s what the tree of possibilities looks like:
![Alt text](https://hackernoon.com/hn-images/1*jiyNCSATqMmL6MwcmEISIA.png)