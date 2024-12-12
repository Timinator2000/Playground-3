# Futoshiki Solver (Revisited)

__Puzzle:__ [Futoshiki Solver](https://www.codingame.com/training/medium/futoshiki-solver)

__Author:__ [@Sundiver](https://www.codingame.com/profile/a4d5c1786311a05772d1b2f5dadac78e6102203)

__Published Difficulty:__ Medium

__Algorithm X Complexity:__ Great Confidence Builder

# Strategy

The first time Futoshiki was discussed in this playground, a few key distinctions were made.

* Just like Sudoku, Futoshiki is a special kind of Latin Square.

* Inequalities allow for important problem-space reduction.

I think we all like to reuse code wherever possible. Here are a couple of conceptual ideas we know to be true:

<BR><BR>
![Futoshiki Classes](FutoshikiClasses.png)
<BR>

In the final Sudoku Challenge, I suggested each cell having pointers to the groups to which it belongs. In Futoshki, each cell only belongs to a row and a column. There are no boxes. Rather than addressing this difference between a `SudokuCell` and `FutoshikiCell`, I suggest going back to the basic version of a `SudokuCell` that did not know about its groups. The basic version is plenty to completely solve all Futoshiki test cases.

The Futoshiki input is somewhat tougher to parse, but you can use your exact same Sudoku code to create  `n * n` instances of the `SudokuCell` class. You can even use your previous code to create `SudokuGroup`s for each row and each column of the Futoshiki grid. Keep in mind, Futoshiki does not have boxes (sub-grids) like Sudoku. These groups will be important if you hope to solve all the Futoshiki test cases completely with logic, but they are not necessary if you plan to do backtracking with Algorithm X.

What I am about to say should not be a surprise and I hope I have not made a mistake by not issuing a spoiler alert, but… the inequalities are <u>really</u> important. They are so important; I suggest you add another class to your object model.

<BR><BR>
![Futoshiki Classes Including Inequality](FutoshikiWithInequality.png)
<BR>

Every `Inequality` needs two pointers. The first points to the `SudokuCell` that must be less than the other side. The second pointer points to the `SudokuCell` that must be greater than the other side. The only critical `Inequality` method is `reduce_()`.

You will need to fill in the details of what it means to __reduce__ an inequality. As you know, reduction in this type of puzzle is all about eliminating candidates from cells. What candidates can be eliminated from the cells on either side of the inequality based on the rules that govern how inequalities work?

Finally, I will copy the exact reduction code structure from the earlier Sudoku discussion into the FutoshikiSolver class constructor.

```python
        finished_reducing = False
        while not finished_reducing:
            finished_reducing = True
            for inequality in self.inequalities:
                if inequality.reduce_():
                    finished_reducing = False
```

<BR>

__Test Case 2: Comparisons only horizontal__ and __Test Case 3: Comparisons only vertical__ are the only two interesting test cases you can completely solve simply by reducing the problem space based on inequalities. However, this one small reduction effort puts Algorithm X in position to find solutions to <u>all remaining</u> test cases very fast.

As I alluded to earlier, a bit more than just basic `SudokuGroup` reduction logic is necessary to solve all test cases strictly with logic, no guessing, but it is very doable. If you would like to take on that challenge, I will get you started with the reduction loop, which probably does not come as a surprise by now.

```python
        finished_reducing = False
        while not finished_reducing:
            finished_reducing = True
            for inequality in self.inequalities:
                if inequality.reduce_():
                    finished_reducing = False

            for group in rows + cols:
                if group.reduce_():
                    finished_reducing = False
```

<BR>

# Solving Logic Puzzles Logically

Many __Futoshiki__ puzzles can be solved without making any guesses. Click [here](solving-with-logic-only) to see my progress toward solving as many logic puzzles as possible, strictly with logic, no guessing.
