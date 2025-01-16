# 42 Push_Swap

This project takes a collection of integers and sorts them by applying a specific set of instructions on two separate stacks. The instructions you may use are listed in the table below, and your program must output the necessary push_swap commands that arrange the integers in ascending order.

## Available Instructions

If an instruction cannot be performed (e.g., there are not enough elements to swap), only the feasible portion of that instruction will run.

| Code  | Instruction                         | Action                                                 |
| ----- | ----------------------------------- | ------------------------------------------------------ |
| `sa`  | swap a                              | swaps the 2 top elements of stack a                    |
| `sb`  | swap b                              | swaps the 2 top elements of stack b                    |
| `ss`  | swap a + swap b                     | Performs both `sa` and `sb`                                     |
| `pa`  | push a                              | moves the top element of stack b at the top of stack a |
| `pb`  | push b                              | moves the top element of stack a at the top of stack b |
| `ra`  | rotate a                            | shifts all elements of stack a from bottom to top      |
| `rb`  | rotate b                            | shifts all elements of stack b from bottom to top      |
| `rr`  | rotate a + rotate b                 | Performs both `ra` and `rb`                                     |
| `rra` | reverse rotate a                    | shifts all elements of stack a from top to bottom      |
| `rrb` | reverse rotate b                    | shifts all elements of stack b from top to bottom      |
| `rrr` | reverse rotate a + reverse rotate b | Performs both `rra` and `rrb`                                   |

## Algorithm

For stacks that contain fewer than 6 integers, there is a straightforward sorting approach implemented (see the src folder).

For bigger sets of integers, this project utilizes a version of the Radix sort. This algorithm efficiently sorts non-negative integers in linear time, denoted as O(n). Take the example below:

```
87 487 781 100 101 0 1
```

Consider 10 “boxes” labeled from 0 to 9.
Starting with the least significant digit (rightmost), place each integer into the box corresponding to that digit.
For instance, 87 has 7 as its last digit, so it goes into box 7. Likewise, 487 has the same last digit 7, so it follows behind 87 in box 7.
After distributing all numbers based on this digit, the arrangement might look like this:

```
box 0    100    0
box 1    781    101    1
box 2
box 3
box 4
box 5
box 6
box 7     87    487
box 8
box 9
```

After that, we connect every number according to the order of boxes.

```
100 0 781 101 1 87 487
```

As we can see, the numbers are sorted according to the digit in 1’s place. For those with the same digit in 1’s place, they’re sorted according to their order in the original list.

We repeat this procedure n times, whiere n is the number of digits of the largest number in the array
(In this case 783 => n = 3).

After doing it n times and connecting numbers after each cycle we will have array sorted.

### Simplify numbers

Instead of dealing with potentially large or negative integers, we assign each number an index based on its relative size. This process is often called "coordinate compression" or "discretization".

   For example, if we have [-5, 100, 2, -10], we'd simplify it to [1, 3, 2, 0].

2. Base-2 representation:
   After simplification, we represent each number in binary (base-2). This allows us to sort using only two stacks, which we'll call A and B.

3. Radix sort adaptation:
   We perform a radix sort, but instead of using 10 buckets (for base-10), we use 2 stacks (for base-2).

4. Sorting process:
   - We start from the least significant bit (rightmost) and move towards the most significant bit (leftmost).
   - For each bit position:
     - If the bit is 0, we move the number to stack B (pb - push to B).
     - If the bit is 1, we rotate stack A (ra - rotate A), keeping the number in A.
   - After processing all numbers for a bit, we move all numbers from B back to A (pa - push to A).
   - We repeat this process for each bit.

### Performance of the Algorithm

My push_swap sorts

    3 numbers with maximum 3 instructions,
    4 numbers with maximum 7 instructions,
    5 numbers with maximum 11 instructions,
    100 numbers with maximum 1084 instructions => 3 points,
    500 numbers with maximum 6785 instructions => 4 points.

The algorith is good enought to pass the project. If the Bonus part is also done the project could get more than 105%.

### Bonus Section

You can also create a checker program that accepts the initial configuration for stack A and then reads instructions from standard input. After receiving all commands (press Ctrl + D to end input), the program will execute them on stacks A and B.

If, after executing every instruction, A is sorted and B is empty, the program outputs "OK".
If it is not sorted or stack B is not empty, it outputs "KO".
If there is any invalid input, it prints Error.
For a reference, the checker logic can be viewed in the checker.c file in this repository.
