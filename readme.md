# SUDOKU COURSEWORK #

# CHOICE OF ALGORITHM #

The AI algorithm that I decided to use was a backtracking search which is a combination of a depth-first search and constraint propagation. 
I chose this mainly due to the fact that it replicates the way in which a human would go about solving a sudoku,
which means that it is already a proven effective strategy and also that it would be easier for me to implement as it follows logical steps.
Backtracking also only considers the children of nodes that are consistent with the constraints,
meaning that the time and space complexity is greatly reduced from that of a standard depth-first search. 
By using constraint propagation it also means that I can include well documented human sudoku solving techniques in my code,
to further reduce the domains and therefore the number of nodes expanded.

# IMPLEMENTATION OF ALGORITHM #

X = {R0C0, R1C0, … , R8C8} : set of variables of the sudoku grid
D = {1,2,3,4,5,6,7,8,9} : set of domains specifying the values each variable can take
C = a number may only be repeated once in each row, column, and three by three grid; naked subset rule ; unique candidate rule
: set of constraints that specifies the values that the variables are allowed to have collectively

# PARTIAL STATE #

Once the sudoku is passed in from the testing script it is passed into the PartialSudokuState class in order to create a partial state. 
A partial state is a state (an assignment of values to all or some of the variables) which does not have all of the variables assigned. 
To create this partial state, once a new partial state object is instantiated, it creates a new attribute, “possible_values”,
which is an array of all the domain values currently possible within the constraints.
The “manually_solve_sudoku” method is then run on this partial state which checks if the state is complete and checks if it is consistent.
If it is complete, which is assessed by “check_if_sudoku_solved” and consistent, which is  assessed by “check_is_invalid”,
then a solution has been reached without needing backtracking and the sudoku is returned. If the state is not consistent and violates the constraints,
it means that the initial sudoku was invalid and therefore returns the sudoku as an array of -1s to indicate this.
In the manually solve method it also runs the methods “strip_multiple_values”, which removes a value from the domain if it
is the only value in the domain of another square in its row, column or 3x3, and “constrain_values”,
which applies the more complicated constraints of naked subsets and unique candidates to the domains.
Both of these methods greatly reduce the size of the domains meaning that there are much fewer nodes expanded and therefore
the sudokus are solved much faster.

# BACKTRACKING #

Once the partial state has been generated and its completeness and consistency have been assessed then the partial state is passed into
the depth-first search function. This function firstly picks the index of the square it is going to guess and then it orders the values
from the domain of that index that it will guess with. It then creates a deep state with a deep copy of the partial state
(so as not to alter the partial state if the guess is incorrect) and in the “set_value” method assigns the guess to the state and alters
the domains of all the other variables according to the constraints. This new state is then assessed: if it is not consistent then it backtracks,
otherwise it keeps going until the solution is reached or no solution is able to be found.

# OPTIMISATION #

Initially my code was not solving the hard puzzles in a reasonable period of time and so I had to optimise the code
to reduce the number of nodes being expanded. I optimised particularly by using heuristics and further constraints.
For the selection of the value to guess in the depth-first search function I initially had both the selection of the
square and the ordering of the domain randomised. In order to optimise and reduce the number of nodes needing to be
expanded by increasing our changes of selecting the right value first time. I did this by using the minimum remaining
value heuristic in the “pick_next_value” function and the least constraining value heuristic in the “order_values” function.
For the constraint propagation I originally only had the constraint of only one of each number in each row, column and 3x3 square.
To add more constraints to the domains I researched sudoku solving techniques (see references) and implemented some of them
such as naked (double and triple) subsets and unique candidates.
I implemented the sole candidate rule, which is commented out in the “constrain_values” method,
however I decided to remove it as there were not enough instances of the sole candidate rule being needed and therefore it actually slowed down the code.

# REFERENCES #

Kristanix.com. 2021. Sudoku Solving Techniques. [online] Available at: <https://www.kristanix.com/sudokuepic/sudoku-solving-techniques.php> [Accessed 10 December 2021].
way, H., 2021. How to sort two lists (which reference each other) in the exact same way. [online] Stack Overflow. Available at: <https://stackoverflow.com/questions/9764298/how-to-sort-two-lists-which-reference-each-other-in-the-exact-same-way> [Accessed 14 December 2021].
