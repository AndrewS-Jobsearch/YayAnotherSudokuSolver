Sudoku Solver

Rules of Sudoku:

    1. Each row, column, and (small) box must
       contain the numbers 1-9 exactly once.

    2. No deleting the original puzzle's boxes!

    3. A well-designed puzzle may not require "guess & check"
       Every cell may be logically deduced from existing cells.

    4. A well-designed puzzle has exactly one solution.

(this solver should also work, at least optionally,
 for puzzles that are not well-designed)

---

Example Input:

    ┌───┬───┬───┐
    │4 9│ 1 │8 3│ 
    │   │  9│24 │
    │   │   │  1│
    ├───┼───┼───┤
    │39 │274│6  │
    │   │ 5 │   │
    │  2│163│ 59│
    ├───┼───┼───┤
    │5  │   │   │
    │ 13│6  │   │
    │9 8│ 2 │1 7│
    └───┴───┴───┘

Using this format, for ease of machine import:

    409010803000009240000000001390274600000050000002163059500000000013600000908020107

---

Example Output:

    ┌───┬───┬───┐
    │429│516│873│
    │851│739│246│
    │637│482│591│
    ├───┼───┼───┤
    │395│274│618│
    │164│958│732│
    │782│163│459│
    ├───┼───┼───┤
    │576│891│324│
    │213│647│985│
    │948│325│167│
    └───┴───┴───┘

    0 guesses required!

---


1. Read input (from cli, file, whatever) into "state" data structure

    `YASS.exe 409010803000009240000000001390274600000050000002163059500000000013600000908020107`

2. Calculate constraints, store in same data structure as "state"

    cell B1 can't be 1_34___89    
    cell D1 can't be 1234_6_8_    
    etc...

   1. if any cell can't be any number, or if any "solved" number is invalid, throw a constraint violation error  

   2. This should be done by the data structure itself for actual rules, with a series of (eventually, pluggable/configurable) constraints (so it can solve more complicated sudoku-family puzzles, ex: https://youtu.be/yKf9aUIxdb4?t=225)

3. Pass structure to series of (eventually, pluggable / configurable) logical solvers

    cell ab can't have 8 numbers, fill it in  
    box 1 can only have a 1 in position xy, fill it in    
    row 4 can only have a 9 in position xy, fill it in  
    cell cd and ef are a pair (cd can only be 1 or 2, ef can only be 1 or 2, cd and ef share a row/column/box), constrain siblings  
    etc...

4. If all logical solvers return with no changes
   1. increment "guesses required by currently selected solvers"
   2. Select random "legal" number, recurse to step 2. 
      1. catch constraint violation errors, increment guesses required, try another "legal" number.
      2. If guess returns answer, add "guesses required" from the guess to my own "guesses required".

5. Return solution (eventually, configurable, for now pretty print).