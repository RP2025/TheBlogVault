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

It's that time of year again! For developers, December isn't just about hot cocoa and holiday lights; it’s about **Advent of Code (AoC)**, the annual series of festive programming puzzles that challenge our logic, coding skills, and sometimes, our sanity. 😅

I've been tackling this year's challenges, and I've decided to document my journey and share my solutions. AoC is more than just solving a puzzle; it's an opportunity to learn new tricks, explore different data structures, and refine your problem-solving muscle.

In this article, I'll walk you through my thought process for **Day 8** and explain the code I developed to solve it. All of my solutions, including the full code for the challenges discussed here, are available on my GitHub repository.

You can find all the code for my Advent of Code solutions here: [RP2025/AdventOfCode](https://github.com/RP2025/adventofcode)

---

### A Clean, Modular & Powerful Approach (Pseudocode Only) for Day 8 AoC

When I saw **Day 8** of Advent of Code, I knew this puzzle would be *incredibly satisfying*. A set of points in space, two parts to solve, and a need for **dynamic subgraph tracking**.
So I decided to write a **clean, modular solution** that works beautifully on **Google Colab**, is easy to debug, and scales effortlessly for thousands of points.

Below is the full blog-style write‑up in Markdown, ready for upload on **Hashnode**.
(No actual code, only structured **pseudocode** and reasoning.)

---

## Problem Summary

You're given a list of points in space (each point has `x, y, z, …` coordinates).
The tasks:

1. Connect points **based on increasing distance**.
2. Track **subgraph sizes dynamically**.
3. Compute two metrics:

* **Part 1** → Product of sizes of the three largest subgraphs after exactly 1000 connections.
* **Part 2** → Product of `x` coordinates of the last connected pair when the graph becomes fully connected.

---

## Why This Approach Rocks

* **Union-Find (Disjoint Set)** → dynamic subgraph tracking in **almost constant time**.
* **Sorted pairs** → ensures we connect points in the correct distance order.
* **Single loop** solves **both parts simultaneously** → efficient and elegant.
* **Modular helper functions** → `load_points`, `distance_sq`, `sorted_pairs` make debugging and testing easier.
* **Perfect for Colab** — readable, scalable, and no unnecessary dependencies.

---

## Pseudocode

### Load Points

Read points from a file and store as tuples.

```text
FUNCTION load_points(filename):
    points = []
    for each line in file:
        convert line to tuple of integers
        append to points
    return points
```

### Distance Between Two Points

```text
FUNCTION distance_sq(p1, p2):
    return sum of squared differences of each coordinate
```

### Generate Sorted Pairs

Sort all pairs of points by distance.

```text
FUNCTION sorted_pairs(points):
    pairs = all 2-combinations of points indices
    sort pairs by distance_sq
    return pairs
```

### Union-Find Data Structure

Tracks subgraphs dynamically with path compression and union by size.

```text
CLASS UnionFind:
    INITIALIZE parents, sizes, subgraphs_count

    FUNCTION find(i):
        while i != parent[i]:
            path compression
            i = parent[i]
        return i

    FUNCTION union(a, b):
        find parents
        if same parent: return False
        merge smaller into larger
        decrement subgraphs_count
        return True
```

### Solve Both Parts

Loop over sorted pairs and track answers.

```text
FUNCTION solve(filename):
    points = load_points(filename)
    uf = UnionFind(number of points)
    pairs = sorted_pairs(points)
    part1_answer = None
    part2_answer = None

    for index, (a, b) in enumerate(pairs):
        uf.union(a, b)
        if index == 999:
            part1_answer = product of 3 largest uf.sizes
        if uf.subgraphs_count == 1 and part2_answer is None:
            part2_answer = points[a].x * points[b].x
            if part1_answer is not None: break

    return part1_answer, part2_answer
```

---

### Final Output Format

```text
part1, part2 = solve('input.txt')
print Part 1 result
print Part 2 result
```

---

## Connect with Me

If you liked this breakdown, check out more of my work:

* **LinkedIn**: [https://linkedin.com/in/raksha-pahariya](https://linkedin.com/in/raksha-pahariya)
* **GitHub**: [https://github.com/RP2025](https://github.com/RP2025)

I post daily breakdowns, clean coding approaches, and Advent of Code fun!
Let's make this go viral 🚀🔥
