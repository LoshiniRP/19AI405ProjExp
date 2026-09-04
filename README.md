# Implement a Sudoku Solver From Scratch
## Steps to solve the Sudoku Puzzle in Python
<ol>
  <li>In this method for solving the sudoku puzzle, first, we assign the size of the 2D matrix to a variable M (M*M).</li>
 <li>Then we assign the utility function (puzzle) to print the grid.</li>
<li>Later it will assign num to the row and col.</li>
<li>If we find the same num in the same row or same column or in the specific 3*3 matrix, ‘false’ will be returned.</li>
<li>Then we will check if we have reached the 8th row and 9th column and return true for stopping further backtracking.</li>
<li>Next, we will check if the column value becomes 9 then we move to the next row and column.</li>
<li>Further now we see if the current position of the grid has a value greater than 0, then we iterate for the next column.</li>
<li>After checking if it is a safe place, we move to the next column and then assign the num in the current (row, col) position of the grid. Later we check for the next possibility with the next column.</li>
<li>As our assumption was wrong, we discard the assigned num and then we go for the next assumption with a different num value</li>
</ol>

## Program:

```
import random

def print_board(board):
    """Print the Sudoku board nicely."""
    for i in range(9):
        if i % 3 == 0 and i != 0:
            print("-" * 21)
        for j in range(9):
            if j % 3 == 0 and j != 0:
                print(" | ", end="")
            print(board[i][j] if board[i][j] != 0 else ".", end=" ")
        print()

def is_valid(board, row, col, num):
    """Check if a number is valid in a given position."""
    for x in range(9):
        if board[row][x] == num or board[x][col] == num:
            return False
            
    start_row, start_col = 3 * (row // 3), 3 * (col // 3)
    for i in range(3):
        for j in range(3):
            if board[start_row + i][start_col + j] == num:
                return False
    return True

def solve_sudoku(board):
    """Solve the Sudoku puzzle using backtracking."""
    for row in range(9):
        for col in range(9):
            if board[row][col] == 0:
                for num in range(1, 10):
                    if is_valid(board, row, col, num):
                        board[row][col] = num
                        if solve_sudoku(board):
                            return True
                        board[row][col] = 0
                return False
    return True

def fill_board(board):
    """Fill the board completely to create a valid full grid."""
    for row in range(9):
        for col in range(9):
            if board[row][col] == 0:
                numbers = list(range(1, 10))
                random.shuffle(numbers)
                for num in numbers:
                    if is_valid(board, row, col, num):
                        board[row][col] = num
                        if fill_board(board):
                            return True
                        board[row][col] = 0
                return False
    return True

def generate_sudoku(num_removed=45):
    """Generate a Sudoku puzzle by removing numbers from a full grid."""
    board = [[0] * 9 for _ in range(9)]
    fill_board(board)
    
    puzzle = [row[:] for row in board]
    removed = 0
    while removed < num_removed:
        row = random.randint(0, 8)
        col = random.randint(0, 8)
        if puzzle[row][col] != 0:
            puzzle[row][col] = 0
            removed += 1
            
    return puzzle, board

# Generate puzzle and solution
puzzle, solution = generate_sudoku(num_removed=40)

print("Generated Sudoku Puzzle:")
print_board(puzzle)

print("\nSolved Sudoku:")
solve_sudoku(puzzle)
print_board(puzzle)
```

## Output:
<img width="245" height="550" alt="image" src="https://github.com/user-attachments/assets/567d36db-88e6-4cb0-afe7-94e43aa8b318" />


## Result:
Thus, the program can be executed successfully.
