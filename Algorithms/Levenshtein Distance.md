An algorithm for measuring the distance between two strings. It returns the minimum number of single-character edits to change one word to another.
## Core Concept
The algorithm calculates the distance based on *three allowable operations*. Each operation has a "cost" of 1.

1. **Insertion**: Adding a character (e.g., `cat` -> `cast`)
2. **Deletion**: Removing a character (e.g., `cast` -> `cat`)
3. **Substitution**: Replacing a character (e.g., `cat` -> `bat`)

The Levenshtein distance between two strings is the sum of the costs of the cheapest set of operations that transforms string A into string B.
## Algorithm
> Calculating this efficiently requires [[Dynamic Programming]].

==TODO==
