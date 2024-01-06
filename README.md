This project is part of Freecodecamp's [Scientific Computing with Python](https://www.freecodecamp.org/learn/scientific-computing-with-python/) curriculum

- **[Tower of Hanoi - Recursive Approach](https://github.com/mzs21/tower-of-hanoi/blob/main/recurtowerofhanoi.py)**
- **[Tower of Hanoi - Iterative Approach](https://github.com/mzs21/tower-of-hanoi/blob/main/itertowerofhanoi.py)**

## Recursive Approach

```py
'''
To solve the puzzle with recursion, the first thing to do is break the original problem down into smaller sub-problems.

The final configuration with n disks piled up to the third rod in decreasing order can be obtained by moving:

n - 1 disks from the source to the auxiliary rod
the largest disk from the source to the target
and then the n - 1 disks from the auxiliary rod to the target.
So, the first thing the move function should do is calling itself with n - 1 as the first argument. But if you try to do so without defining a base case, you will get a RecursionError. This happens because the function keeps calling itself indefinitely.
'''

NUMBER_OF_DISKS = 5

A = list(range(NUMBER_OF_DISKS, 0, -1))
B = []
C = []

def move(n, source, auxiliary, target):
    if n <= 0: # Base case. If n is less than or equal to 0, it will return
        return

    # move n - 1 disks from source to auxiliary, so they are out of the way
    move(n - 1, source, target, auxiliary)

    # move the nth disk from source to target
    target.append(source.pop())

    # display our progress
    print(A, B, C, '\n')

    # move the n - 1 disks that we left on auxiliary onto target
    move(n - 1,  auxiliary, source, target)

# initiate call from source A to target C with auxiliary B
move(NUMBER_OF_DISKS, A, B, C)


# initiate call from source A to target C with auxiliary B
move(NUMBER_OF_DISKS, 'A', 'B', 'C')
```

## Iterative Approach

```py
NUMBER_OF_DISKS = 4
number_of_moves = 2**NUMBER_OF_DISKS - 1

rods = {
    'A': list(range(NUMBER_OF_DISKS, 0, -1)),
    'B': [],
    'C': []
}

def move(n, source, auxiliary, target):
    # display starting configuration
    print(rods, '\n')
    for i in range(number_of_moves): #The allowed disk movements between the rods exhibit a repetitive pattern occurring every three moves.
        remainder = (i + 1) % 3

        if remainder == 1:
            # For example, movements between rod A and rod C are allowed on the first, the fourth and the seventh move, where the remainder of the division between the move number and 3 is equal to 1

            if n%2 == 1: # If number of disks is odd

                print(f'Move {i + 1} allowed between {source} and {target}')

                make_allowed_move(source, target)

            else:
                print(f'Move {i + 1} allowed between {source} and {auxiliary}')

                make_allowed_move(source, auxiliary)

        elif remainder == 2:
            # When the remainder of the move number divided by 3 is equal to 2, the movement is allowed between A and B (the source and the auxiliary rods).

            if n%2 == 1:  # If number of disks is odd

                print(f'Move {i + 1} allowed between {source} and {auxiliary}')

                make_allowed_move(source, auxiliary)

            else:
                print(f'Move {i + 1} allowed between {source} and {target}')
                make_allowed_move(source, target)


        elif remainder == 0:
            # When the move number divided by 3 has no remainder, the movement is allowed between B and C.
            print(f'Move {i + 1} allowed between {auxiliary} and {target}')

            make_allowed_move(auxiliary, target)

def make_allowed_move(rod1, rod2):
    forward = False # Check in which direction it's needed to move the disk between the rods

    if not rods[rod2]:
        # When target (rod2) is empty, the disk should be moved necessarily from source (rod1) to target (rod2)
        forward = True

    elif rods[rod1] and rods[rod1][-1] < rods[rod2][-1]:
        # If source list (rod1) is not empty and the last disk in source (rod1) is lower than the last disk in target (rod2)
        forward = True

    if forward:
        print(f'Moving disk {rods[rod1][-1]} from {rod1} to {rod2}')

        rods[rod2].append(rods[rod1].pop())
        # Remove the last element from the source rod (rod1) and append it to target rod (rod2)

    else: # When forward is False, the disk has to be moved in the opposite direction.
        print(f'Moving disk {rods[rod2][-1]} from {rod2} to {rod1}')

        rods[rod1].append(rods[rod2].pop())
        # Remove the last element from the target rod (rod2) and append it to source (rod1) rod

    # Display our progress
    print(rods, '\n')


# initiate call from source A to target C with auxiliary B
move(NUMBER_OF_DISKS, 'A', 'B', 'C')
```
