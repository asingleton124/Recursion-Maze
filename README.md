'''proj3.1. Maze Solver - Linked List Stack
Linked List Based Stack and Maze Solving
Your goal for this project is to write a Maze class that stores a maze and solves the maze. To make sure we can track the solution of our Maze, we'll create a linked-list based stack to keep track of the solution path. We'll consider a solution path one that starts at the entry point, ends at the exit point, and does not visit the same spot twice (e.g. a final path won't involve going down something, backtracking, and then going another way to get to the solution, nor will it involve needlessly looping in a cycle before taking the path that gets to the solution).
Step 0 - Understand and the Node Class
You are provided a small Node class defined for you in node.py. These nodes will make up the linked-list-based stack you create in the next step. Download and read through this class to make sure you understand it.


Step 1 - Linked list Based Stack (LLStack) Class
Create a class called LLStack in a file named llstack.py that will store a singly-linked-list (without tail) based stack, using instances of the
This should have the following instance variables:
__head: Node: the head node in the underlying linked list used to store the stack
__size: int: the number of elements in the stack
This class should have the following properties:
size: Underlying variable should be __size. Readable only.
It should also have the following methods:
__init__(self): initializes empty linked-list-based stack, with __headinitialized to None and __size initialized to 0.
pop(self) -> tuple: removes the top node on the stack and returns the data (tuple) stored at that node. Raise an IndexError if the stack is empty.
push(self, data: tuple): add a new node to the top of the stack. Raise a TypeError if data is not a tuple containing integers.
__str__(self): str: gives a string representation of the stack. The exact form of this is up to you, but it should make use of the __str__ method for each node, and the output for each node should be in the order they would need to be traversed, with the string representation for the node at the entry point coming first and the node at the exit point coming last. As one example, the following might be a valid string representation of path for a  maze with entrance at 0,0 and exit at 2,2 (assuming all of these spots are not walls):
(0,0) -> (0,1) -> (1,1) -> (2,1) -> (2,2)





Step 2 - Maze Class
Create a file named maze.py. Before diving into the maze class, create the following custom exception class in maze.py:
class InvalidCoordinateError(Exception):
    pass



Then, create a class called Maze in maze.py that will store the maze, necessary details, and contain methods to solve the maze. The smallest puzzle your program should accept is a 3x3 maze.
This should have the following instance variables:
__nrows: the number of cells wide the maze is
__ncols: the number of cells tall the maze is
__entry: tuple, the indices of the entry point of the maze in the form (row, col)
__exit: tuple the indices of the exit point of the maze in the form (row, col)
__grid: List[List[str]]: the underlying grid storing the maze. Spots with a 'o' are open and spots with an 'x' are walls.
__path: LLStack: the path through the maze
__shortest_path: LLStack: the shortest path through the maze
It should also have the following properties (created via the use of @property(readable) and/or @property.setter (writeable) decorators. Make sure the setters enforce any restrictions.
nrows: Underlying variable should be __nrows. Readable and writeable. Should raise a ValueError if less than 3.
ncols: Underlying variable should be __ncols. Readable and writeable. Should raise a ValueError if less than 3.
entry_coords: Underlying variable should be __entry. Readable and writeable. Should raise a TypeError if not provided a tuple or if the tuple entries are not integers. Should raise an InvalidCoordinateError if not a valid spot in the maze or if the corresponding spot in the maze is a wall.
exit_coords: Underlying variable should be __exit. Readable and writeable. Should raise a TypeError if not provided a tuple or if the tuple entries are not integers. Should raise an InvalidCoordinateError if not a valid spot in the maze or if the corresponding spot in the maze is a wall. You will need to create a custom exception with this name, which should be done in the maze.py file.
path: Underlying variable should be __path. Should be readable, but not writeable.
shortest_path: Underlying variable should be __shortest_path. Should be readable, but not writeable.
It should have the following methods:
__init__(self, grid: List[List[str]], entry_loc: tuple, exit_loc: tuple: constructor for Maze. All instance variables should be initialized to the proper values based on the parameters, ensuring that the errors specified under properties above are raised if a maze with an invalid size is given or if invalid coordinates for entry/exit are provided. Ensure that grid is of the appropriate type, raising a TypeError if it is not a list of lists of strings. Hint: if your constructor uses your properties, you won't duplicate code to check everything is valid. Both __path and __shortest_path should both be initialized to None.
solve(self): the top level method to solve the maze.
This is the method the user would call directly.
If the maze has a solution, after calling this, __path should be updated to be an LLStack representing the valid path through the maze (with the exit cell being the node at the top of the stack and the entry cell being the node at the bottom of the stack).
If the maze is not solveable, __path should continue to be None after calling this method.
This method should not use any loops
__solve_helper(...): recursive helper method to solve a maze.
You can decide what arguments this needs to take
You can decide what if anything it needs to return
This method must be recursive and should not use any loops.
solve_shortest(self): a top level method to solve the maze, not just finding a valid solution, but instead finding the shortest possible path.
This is a method the user would call directly.
After calling this, __shortest_path should be updated to be an LLStackrepresenting the shortest possible valid path through the maze (with the exit cell being the node at the top of the stack and the entry cell being the node at the bottom of the stack).
You can assume there will be a unique shortest path through the maze
Hints:
To do this, you'll likely want to use recursion with memoization, where you effectively store answers to subproblems that you've already seen (often for this it makes sense to use a dictionary, where they key is what represents the subproblem and the value is the answer for that subproblem). Then, as one of your base cases you check if the problem has already been solved, returning the already known answer.
It may help to split this into two steps:
A helper method that uses recursion with memoization to figure out the shortest distance from a cell to the exit cell (filling in something like a dictionary as you go).
Another recursive helper method that gets called after the first to construct the shortest path and assign to shortest_path.
Example Maze
Consider the maze with the underlying grid given by:
grid1 = [['o','x','o','o','o'],
         ['o','x','o','x','o'],
         ['o','o','o','x','o'],
         ['o','x','x','o','o'],
         ['o','o','o','o','o']]



The maze with this underlying grid has multiple possible solution paths:
(0,0) -> (1,0) -> (2,0) -> (2,1) -> (2,2) -> (1,2) -> (0,2) -> (0,3) -> (0,4) -> (1,4) -> (2,4) -> (3,4) -> (3,3) -> (4,3) -> (4,4)
(0,0) -> (1,0) -> (2,0) -> (2,1) -> (2,2) -> (1,2) -> (0,2) -> (0,3) -> (0,4) -> (1,4) -> (2,4) -> (3,4) -> (4,4)
(0,0) -> (1,0) -> (2,0) -> (3,0) -> (4,0) -> (4,1) -> (4,2) -> (4,3) -> (3,3) -> (3,4) -> (4,4)
(0,0) -> (1,0) -> (2,0) -> (3,0) -> (4,0) -> (4,1) -> (4,2) -> (4,3) -> (4,4)
However, it has a unique shortest path: (0,0) -> (1,0) -> (2,0) -> (3,0) -> (4,0) -> (4,1) -> (4,2) -> (4,3) -> (4,4)
Notes
You are NOT allowed to use python's built-in lists in your implementation of the LLStack at all
You must store your path using an LLStack, not python's built-in lists.
Your maze solver approaches must be recursive.
Your LLStack class must be a stack using a singly-linked-list without tail for the underlying implementation.
Make sure your stack implementation has push and pop being both operations.
Make sure any internal helper methods or instance variables you have start with __.
Do not use a loop in solve(), solve_shortest(), or any helper methods either of these call (finding a path or the shortest path should be done using recursion).
