---
title: "Advent of Code Day 10 --- Optimizing Dual-Mode Machines"
seoTitle: "Advent of Code Day 10: Optimized Dual-Mode Machine Solver Using BFS"
seoDescription: "Learn how to solve Advent of Code Day 10 efficiently with modular Python code. Explore XOR bitmask BFS for indicator lights, CP-SAT integer programming for "
datePublished: Wed Dec 10 2025 20:08:31 GMT+0000 (Coordinated Universal Time)
cuid: cmj0fz7br000602ie4h75gxj9
slug: advent-of-code-day-10-optimizing-dual-mode-machines
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765397191475/22a0bca0-979d-45d7-b886-914d6e65d9b3.png
tags: programming-blogs, python, adventofcode

---

### A Deep Dive into Bitmask Logic, Linear Algebra, and Smart State Search

## Introduction

Day 10 of Advent of Code brings a refreshing twist---**two completely
different machine modes**, two different interpretations of the same
input... and a beautiful opportunity to write elegant, modular, and
scalable problem-solving code.

This blog breaks down: - How Part 1 and Part 2 differ\
- The underlying theory\
- How to unify them into one clean solution\
- Why the final approach is optimized\
- Pseudocode (not real code!)\
- How modular design + algorithmic insight save the day

------------------------------------------------------------------------

## Part 1 --- Indicator Lights (XOR Mode)

### Problem Summary

Machines have indicator lights like:

    [.#..#]

Buttons toggle light positions --- pressing a button flips certain
indices using XOR.

Goal:\
**Find the minimum number of button presses required to transform all
indicator lights into the desired pattern.**

### Theoretical Insight

-   Each state of lights can be represented as a **bitmask**.
-   Each button is also a bitmask.
-   A press → `state XOR button_mask`.

This forms a graph where: - Nodes = bitmasks\
- Edges = pressing any button\
- Edge cost = 1

Thus, **BFS** guarantees the minimum steps.

### Pseudocode Snippet

    function solve_part1(target_mask, button_masks):
        queue = [0]       // start from all lights off
        dist = {0: 0}

        while queue not empty:
            cur = pop(queue)
            if cur == target_mask:
                return dist[cur]

            for b in button_masks:
                nxt = cur XOR b
                if nxt not in dist:
                    dist[nxt] = dist[cur] + 1
                    push(queue, nxt)

### Why This Works

BFS is ideal because: - All edges have unit weight\
- State space is small (2\^N)\
- XOR operations are O(1)

This guarantees optimality *and* speed.

------------------------------------------------------------------------

## Part 2 --- Joltage Configuration (Linear Mode)

### Problem Summary

Now the machine ignores the indicator lights completely.\
Instead, we have:

    {3,5,4,7}

Each button increments certain counters: - Pressing button j adds +1 to
each counter in its list. - You can press buttons unlimited times. -
Find **minimum total button presses** to reach the target vector.

This is no longer XOR --- this is **integer linear algebra**.

### Theoretical Interpretation

We solve:

    A * x = target

Where: - `A` is a (k × n) matrix (k counters, n buttons) - `x` is how
many times each button is pressed - Minimize sum(x)

This is a **linear Diophantine system + optimization**.

### Why BFS/Dijkstra Fails

-   Counters may go up to hundreds.
-   State space becomes astronomical → impossible to BFS.

### Correct Solution

Use **Integer Linear Programming** (ILP) via CP-SAT: - Fast - Exact -
Handles large dimensions - Finds minimal sum(x)

### Pseudocode Snippet

    function solve_part2(target, button_sets):
        create CP-SAT model
        variables x[j] = integer >= 0

        for each counter i:
            sum of x[j] over buttons affecting i == target[i]

        minimize sum(x)

        solve model
        return optimal sum(x)

------------------------------------------------------------------------

## Unified Modular Program Design

Instead of writing two giant separate programs, we combine them neatly:

### Key Architectural Principles

-   **Shared parser**: Both parts use the same input lines; parsing
    happens once.
-   **Mode-based solvers**:
    -   Part 1: XOR BFS\
    -   Part 2: ILP solver\
-   **Reusable data structures**:
    -   Bitmasks\
    -   Button index lists\
-   **Single pass input processing**: Cleaner + faster

### High-Level Pseudocode

    parse input:
        extract light pattern
        extract target vector
        extract button lists

    for each machine:
        part1_total += solve_part1(...)
        part2_total += solve_part2(...)

    print part1_total, part2_total

------------------------------------------------------------------------

## Why This Dual-Solver Architecture is Optimal

### ✔ Clean Separation of Concerns

Part 1 uses **bitmasks + BFS**.\
Part 2 uses **integer programming**.\
Two very different mathematical problems, both solved with their ideal
tools.

### ✔ Modular Functions

-   `solve_part1()`
-   `solve_part2()`
-   `parse_input_line()`

Easy to test, reuse, and maintain.

### ✔ Highly Scalable

-   BFS handles up to 2\^20 states effortlessly.\
-   CP-SAT handles thousands of constraints.

### ✔ Zero Redundant Work

Each line is parsed once and fed into both solvers.

### ✔ Algorithmic Optimality

-   BFS → guaranteed shortest path\
-   ILP → guaranteed minimum press count

This ensures correctness across **all** AoC test cases.

------------------------------------------------------------------------

## Why This Solution is Developer-Friendly

### ✦ Easy Debugging

Modular solver functions\
Clear intermediate representations\
Traceable constraints

### ✦ Easy to Extend

Want Part 3?\
Just plug another "solver module".

### ✦ Clean Pseudocode

Readable even for non-Python developers.

------------------------------------------------------------------------

## Final Thoughts

Day 10 beautifully demonstrates how **different problem models require
different mathematical tools** --- and how a well-architected program
can integrate them seamlessly.

By mixing: - bitwise operations\
- BFS\
- constraint programming\
- modular system design

...you build a solution that is not only correct but also elegant and
scalable.

------------------------------------------------------------------------

## Connect With Me

-   **GitHub**: https://github.com/RP2025\
-   **LinkedIn**: https://www.linkedin.com/in/raksha-pahariya-409842227/

Stay tuned for more Advent of Code breakdowns, system design notes, and
quantum-computing explainers 🚀
