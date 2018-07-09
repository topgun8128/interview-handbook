---
description: 'source:hackerearth'
---

# Greedy Algorithms

**What is a 'Greedy algorithm'?**

A greedy algorithm, as the name suggests, **always makes the choice that seems to be the best at that moment**. This means that it makes a locally-optimal choice in the hope that this choice will lead to a globally-optimal solution.

How do you decide which choice is optimal?

Assume that you have an **objective function** that needs to be optimized \(either maximized or minimized\) at a given point. A Greedy algorithm makes greedy choices at each step to ensure that the objective function is optimized. The Greedy algorithm has only one shot to compute the optimal solution so that **it never goes back and reverses the decision**.

**Where to use Greedy algorithms?**

A problem must comprise these two components for a greedy algorithm to work:

1. It has **optimal substructures**. The optimal solution for the problem contains optimal solutions to the sub-problems.
2. It has a **greedy property** \(hard to prove its correctness!\). If you make a choice that seems the best at the moment and solve the remaining sub-problems later, you still reach an optimal solution. You will never have to reconsider your earlier choices.

For example:

1. [Activity Selection problem](https://en.wikipedia.org/wiki/Activity_selection_problem)
2. [Fractional Knapsack problem](https://en.wikipedia.org/wiki/Continuous_knapsack_problem)
3. Scheduling problem

**Examples**

The greedy method is quite powerful and works well for a wide range of problems. Many algorithms can be viewed as applications of the Greedy algorithms, such as \(includes but is not limited to\):

1. [Minimum Spanning Tree](https://www.hackerearth.com/practice/algorithms/graphs/minimum-spanning-tree/tutorial/)
2. [Dijkstra’s algorithm for shortest paths from a single source](https://www.hackerearth.com/practice/algorithms/graphs/shortest-path-algorithms/tutorial/)
3. [Huffman codes \( data-compression codes \)](https://en.wikipedia.org/wiki/Huffman_coding)



More on GeeksforGeeks

{% embed data="{\"url\":\"https://www.geeksforgeeks.org/greedy-algorithms/\#greedyAlgorithmsInGraphs\",\"type\":\"link\",\"title\":\"Greedy Algorithms - GeeksforGeeks\",\"description\":\"Recent Articles on Greedy Algorithms ! Standard Greedy Algorithms Greedy Algorithms in Graphs Greedy Algorithms in Operating Systems Greedy Algorithms in Arrays Approximate Greedy Algorithms… Read More »\",\"icon\":{\"type\":\"icon\",\"url\":\"https://www.geeksforgeeks.org/wp-content/uploads/gfg\_200X200.png\",\"width\":200,\"height\":200,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://www.geeksforgeeks.org/wp-content/uploads/gfg\_200X200.png\",\"width\":200,\"height\":200,\"aspectRatio\":1}}" %}

