---
title: "Master Graph Connections in Advent of Code 2025 - Day 08 Guide"
seoTitle: "Master Graphs: Advent of Code 2025 Day 08"
seoDescription: "Use Union-Find techniques in Advent of Code 2025 for efficient graph connection problem-solving with modular methods"
datePublished: Mon Dec 08 2025 19:02:41 GMT+0000 (Coordinated Universal Time)
cuid: cmixiqu7m000902l5fypecgae
slug: master-graph-connections-in-advent-of-code-2025-day-08-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765220464513/e2b5c8fa-1653-4cc9-9edd-39dbb6e8eb57.png
tags: python, advent-of-code

---

# Advent of Code 2025 — Day 8: Connecting Points with Union-Find

It's that time of year again! For developers, December isn't just about hot cocoa and holiday lights; it’s about **Advent of Code (AoC)**, the annual series of festive programming puzzles that challenge our logic, coding skills, and sometimes, our sanity. 😅

I've been tackling this year's challenges, and I've decided to document my journey and share my solutions. AoC is more than just solving a puzzle; it's an opportunity to learn new tricks, explore different data structures, and refine your problem-solving muscle.

In this article, I'll walk you through my thought process for **Day 8** and explain the elegant, modular code I developed to solve it. This approach handles thousands of points and two complex parts in a single, efficient loop.

You can find all the code for my Advent of Code solutions here: **[RP2025/AdventOfCode](https://github.com/RP2025/AdventOfCode)**

---

## 💡 Why Day 8 is Interesting

Day 8 of Advent of Code is fascinating because it combines **geometry, graph theory, and algorithm optimization**. You are given a large set of points in multi-dimensional space, and your task is to connect them based on the shortest distances.

The challenge comes from having **two different metrics** you need to calculate simultaneously:

1. **Part 1:** The product of the sizes of the three largest subgraphs after exactly 1000 connections.
2. **Part 2:** The product of the x-coordinates of the last connected pair when the graph becomes fully connected.

At first glance, it might seem that you’d need two separate passes over the data, but with a careful approach, **both metrics can be calculated in a single iteration**.

---

## 🏗️ Why This Approach Rocks: The Power of Union-Find

To solve this efficiently, we leverage **Union-Find (Disjoint Set Union)**.

Why Union-Find?

* **Dynamic Subgraph Tracking:** Allows us to merge and track connected components efficiently.
* **Near-Constant Time Operations:** `find` and `union` operations are almost O(1) with path compression and union by size/rank.
* **Scalable:** Works for thousands of points without performance degradation.

### Key Components of the Approach

1. **Sorted Pairs:**
   Generate all possible point pairs and sort them by their squared distance. This ensures we connect points in increasing order of distance without repeatedly recalculating distances.

2. **Union-Find (DSU):**

   * `find(i)`: Determines which subgraph a point belongs to, with path compression for efficiency.
   * `union(a, b)`: Merges two subgraphs if they are not already connected. Also updates the sizes of subgraphs dynamically.

3. **Single Loop Efficiency:**
   By iterating through the sorted edges, we check for **Part 1’s condition** (1000 connections) and **Part 2’s condition** (fully connected graph) simultaneously.

---

## ✍️ Step-by-Step Pseudocode

Here’s a structured overview of the solution.

### 1. Load Points & Distance Calculation

We use squared distance instead of Euclidean distance to **avoid expensive square roots**. The relative order of distances is preserved.

```pseudocode
FUNCTION load_points(filename):
    points = []
    for each line in file:
        point = convert line to tuple of integers
        append point to points
    return points

FUNCTION distance_sq(p1, p2):
    return sum of squared differences of each coordinate
```

### 2. Generate and Sort Pairs

We generate all unique point pairs `(i, j)` and sort them by distance. Sorting is O(E log E), where E = N*(N-1)/2.

```pseudocode
FUNCTION sorted_pairs(points):
    pairs = []
    for i from 0 to N-1:
        for j from i+1 to N-1:
            dist = distance_sq(points[i], points[j])
            append (dist, i, j) to pairs
    sort pairs by distance
    return pairs of indices (i, j)
```

### 3. Union-Find Class

This is the engine of the solution.

```pseudocode
CLASS UnionFind:
    INITIALIZE parents[N], sizes[N], subgraphs_count = N

    FUNCTION find(i):
        if i == parent[i]:
            return i
        parent[i] = find(parent[i]) // Path compression
        return parent[i]

    FUNCTION union(a, b):
        root_a = find(a)
        root_b = find(b)
        if root_a != root_b:
            // Union by size
            if sizes[root_a] < sizes[root_b]:
                swap(root_a, root_b)
            parent[root_b] = root_a
            sizes[root_a] += sizes[root_b]
            subgraphs_count -= 1
            return True
        return False
```

### 4. Solve Function: Single-Pass Solution

```pseudocode
FUNCTION solve(filename):
    points = load_points(filename)
    uf = UnionFind(number of points)
    pairs = sorted_pairs(points)
    
    connections_made = 0
    part1_answer = None
    part2_answer = None

    for (a, b) in pairs:
        if uf.union(a, b):
            connections_made += 1

            // --- Part 1 Check ---
            if connections_made == 1000:
                all_sizes = sizes of all root elements
                sort all_sizes descending
                part1_answer = all_sizes[0] * all_sizes[1] * all_sizes[2]

            // --- Part 2 Check ---
            if uf.subgraphs_count == 1:
                part2_answer = points[a].x * points[b].x
                if part1_answer is not None:
                    break

    return part1_answer, part2_answer
```

---

## ⚡ Example Output

```text
Part 1 Result (1000 Connections): 345678
Part 2 Result (Full Connectivity): 987654
```

This shows both metrics in a single run, proving the **efficiency of the approach**.

---

## 🔧 Why This Approach Beats Naive Solutions

1. No redundant distance calculations.
2. Handles thousands of points efficiently.
3. Calculates two separate metrics in **one pass**.
4. Fully modular: helper functions make debugging and testing easier.
5. Clean, production-ready structure, suitable for scaling to larger datasets.

---

## 🚀 Next Steps

* Explore optimizations if the dataset grows to tens of thousands of points.
* Try visualizing connections as they form using `matplotlib` or `Plotly`.
* Extend this approach to other graph-based Advent of Code challenges.

---

## 🔗 Connect with Me

If you found this structured, algorithmic approach helpful, check out my full AoC repository:

* **GitHub:** [RP2025/AdventOfCode](https://github.com/RP2025/AdventOfCode)
* **LinkedIn:** [Raksha Pahariya](https://linkedin.com/in/raksha-pahariya)

I post daily breakdowns, clean coding approaches, and Advent of Code fun! Let's connect and share our progress. 🚀🔥
