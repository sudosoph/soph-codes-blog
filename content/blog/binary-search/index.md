---
title: All About Binary Search
date: "2020-04-27"
description: Walk-through of a simple Binary Search implementation in Python and discussion of binary search time and space complexity.
---

A binary search tree is an algorithm that finds the position of a target value in a _sorted_ list.

## Process
1. Compare target to middle number, since the last is sorted, this tells us whether the target will be in the left or right half of the list.
2. We now ignore the other half of the list, seeing as we have divided the problem in half.
3. Repeat the same process, with the middle of the new half of the list we are using, until finding the target value or not finding it in the list.

## Benefits
- Always reduces complexity of linear search from O(log(N)) to O(log(N)) in sorted list
	- This is because search interval decreases by a power of two ech time (halving the lists). Includes cost of insert(), delete(), and lookup()
	- Space complexity is O(1), meaning it requires constant time to perform operations, as we just need to store three values (upper, middle, and lower bounds)
- More efficient for searching for specific target from large input
	- We have ordering of keys stored in trees
- Simple to program for lists
- Can use BST to do range queries (finding values between x and y values) and implement order statistics seeing as the list is sorted

**Fun fact:** You could search all the names in the world (written in order) and find a specific name in a maximum of [35 iterations](https://www.hackerearth.com/practice/algorithms/searching/binary-search/tutorial/)

## Disadvantages
- It only works on sorted lists that are kept sorted
	- More complicated to implement than a linear search, requires three-way update of low and high index and potentially an additional check if target wasn't found
	- Overkill for smaller lists
- The recursive implementation requires more stack space
- Loss of efficiency if the list does not support random-access
- Using interpolating search to predict where to start searching when the elements are distributed evenly gets us to our destination more quickly than a binary search tree, i.e. O(log(log(N))) instead of O(log(N))
	- Improvement over Binary Search for sorted arrays that are uniformly distributed (sorted but unindexed on-disk datasets)
	- However, when list size increases exponentially, interpolation search time complexity is O(log(N)), the worst case, as it has to go to different locations according to the value of what is being searched

## Implementation

```shell
def binary_search(target, input):
    """See if target appears in input"""
    # We think of low and high as "walls" around
    # the possible positions of our target so by -1 below we mean
    # to start our wall "to the left" of the 0th index
    # (we *don't* mean "the last index")
    low = -1
    high = len(input)

    # If there isn't at least 1 index between low index and high index,
    # we've run out of guesses and the number must not be present
    while low + 1 < high:
        # Find the index ~halfway between the low index and high index
        # We use integer division, so we'll never get a "half index"
        distance = high - low
        middle = distance // 2
        guess_index = low + middle

        guess_value = input[guess_index]
        if guess_value == target:
            return True

        if guess_value > target:
            # Target is to the left, so move high index to the left
            high = guess_index
        else:
            # Target is to the right, so move low index to the right
            low = guess_index

    return False
```

## Python-Specific Modules


