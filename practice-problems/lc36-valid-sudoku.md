# Valid Sudoku

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

**Note**:

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

**Constraints**:

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit `1-9` or `'.'`.

## Solutions

Generally involves using a set to keep track of duplicates while looping through grid. Best case is an empty grid ($O(n^2)$ time still spent looping, but no space needed to store seen items), and worst case is a full grid (where you must track all items in grid).

### Brute Force

Just loop through everything separately, using a set to keep track of duplicates.

**Time Complexity**: $O(n^2)$

- Looping through the 9 rows and 9 cols.
- Using a _HashSet_ allows for $O(1)$ duplicate checks, but it can still be done $9^2$ times

**Space Complexity**: $O(n)$

- Using space for sets.
- Each set only contains at max 9 items, and is then discarded before the next one is created.

```java
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        // Loop through rows
        for (int row = 0; row < 9; row++) {
            Set<Character> seen = new HashSet<>();

            for (int i = 0; i < 9; i++) {
                if (board[row][i] == '.') continue;
                if (seen.contains(board[row][i])) return false;
                seen.add(board[row][i]);
            }
        }

        // Loop through columns
        for (int col = 0; col < 9; col++) {
            Set<Character> seen = new HashSet<>();

            for (int i = 0; i < 9; i++) {
                if (board[i][col] == '.') continue;
                if (seen.contains(board[i][col])) return false;
                seen.add(board[i][col]);
            }
        }

        // Loop through squares
        // Use int division and modulo to stay in a row and column
        for (int square = 0; square < 9; square++) {
            Set<Character> seen = new HashSet<>();
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    int row = (square / 3) * 3 + i;
                    int col = (square % 3) * 3 + j;

                    if (board[row][col] == '.') continue;
                    if (seen.contains(board[row][col])) return false;
                    seen.add(board[row][col]);
                }
            }
        }

        // Return true if no duplicates were found
        return true;
    }
}
```

### Hash Set (One Pass)

Instead of having 3 separate loops, keep a map of sets for rows, cols, and sqaures. Each map will contain 9 items and a set up of up to 9 numbers.

**Time Complexity**: $O(n^2)$

- Using hash maps and sets allow for constant check times, but you still have to loop the whole 9x9 grid.

**Space Complexity**: $O(n^2)$

- You need to store and manage the entire contents of the board in 3 separate forms.

```java
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        Map<Integer, Set<Character>> rows = new HashMap<>(); // (row #, set of seen numbers)
        Map<Integer, Set<Character>> cols = new HashMap<>();
        Map<String, Set<Character>> squares = new HashMap<>(); // ("r,c", set of seen numbers)

        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                char item = board[r][c];

                if (item == '.') continue;

                String squareKey = (r / 3) + "," + (c / 3);

                // Check for duplicate in row, col, square all at once
                // Initialize set if currently absent, then check for item in set
                if (rows.computeIfAbsent(r, k -> new HashSet<>()).contains(item) ||
                    cols.computeIfAbsent(c, k -> new HashSet<>()).contains(item) ||
                    squares.computeIfAbsent(squareKey, k -> new HashSet<>()).contains(item)) {
                        return false;
                }

                // Add item into sets
                // We know that the Map.get() exists because of the Map.computeIfAbsent()
                rows.get(r).add(item);
                cols.get(c).add(item);
                squares.get(squareKey).add(item);
            }
        }

        // Return true if no duplicates found
        return true;
    }
}
```
