---
title: "Advent of Code Day 9 guide using maths! Movie Theater Tiles"
datePublished: Tue Dec 09 2025 17:56:31 GMT+0000 (Coordinated Universal Time)
cuid: cmiyvtlyt000102iiccmj7dzj
slug: advent-of-code-day-9-guide-using-maths-movie-theater-tiles
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765302848904/0443d524-66ce-47b3-8ae0-e26d34819e72.png
tags: python, coding, adventofcode

---

### A Clean, Modular & Powerful Approach (Pseudocode + Theory)

Advent of Code Day 9 presented a fascinating challenge — a grid-based puzzle inspired by a movie theater floor. The problem involves **red tiles** on a grid and asks for the **largest rectangle using red tiles as opposite corners**. Part 2 adds a twist: the rectangle can include **green tiles**, which are defined by the path between consecutive red tiles and the polygon's interior.

This blog explores a **modular solution**, including **pseudocode**, theoretical explanations, and practical implementation using Python and Shapely.

---

## Problem Summary

You're given a **list of red tile coordinates** on a 2D grid. The goals are:

* **Part 1:** Identify the largest rectangle using any two red tiles as opposite corners.
* **Part 2:** Only consider rectangles where all interior tiles are **red or green**. Green tiles are:

  * Tiles connecting consecutive red tiles (horizontal or vertical line)
  * All tiles inside the polygon formed by the red-green loop

This creates a **geometry problem** on a discrete grid with constraints.

---

## Theoretical Insights

1. **Grid-based rectangle calculation:**

   * Rectangle area = `(width * height)`
   * Width and height are calculated **inclusively**.
   * Any two red tiles define a candidate rectangle.

2. **Polygon theory for Part 2:**

   * The ordered red tiles form a **closed loop**.
   * Interior + edges of the polygon define the **green tiles**.
   * Valid rectangles must **lie completely inside** this polygon.

3. **Efficiency considerations:**

   * Check all pairs of red tiles → `O(n^2)` combinations.
   * For Part 2, polygon containment check reduces the need to enumerate all green tiles individually.
   * Using `shapely.prepared.prep` optimizes repeated containment checks.

4. **Modular approach:**

   * `parse_points()` → reads and formats the input
   * `rectangle_area()` → computes inclusive area between two points
   * `part1()` → checks all red-red rectangles
   * `part2()` → checks red-red rectangles that lie inside red-green polygon

---

## Pseudocode Snippets

### 1️⃣ Parse Input

```text
FUNCTION parse_points(file_path):
    points = empty list
    FOR each line in file:
        convert line to (x, y) tuple
        append to points
    RETURN points
```

### 2️⃣ Compute Rectangle Area

```text
FUNCTION rectangle_area(point_a, point_b):
    width = abs(point_a.x - point_b.x) + 1
    height = abs(point_a.y - point_b.y) + 1
    RETURN width * height
```

### 3️⃣ Part 1 --- Maximum Rectangle Without Constraints

```text
FUNCTION part1(points):
    max_area = 0
    FOR each pair (a, b) in combinations(points, 2):
        area = rectangle_area(a, b)
        IF area > max_area:
            max_area = area
    RETURN max_area
```

### 4️⃣ Part 2 --- Maximum Rectangle Inside Red-Green Polygon

```text
FUNCTION part2(points):
    polygon = construct_polygon(points)
    prepared_polygon = prepare(polygon)
    max_area = 0

    FOR each pair (a, b) in combinations(points, 2):
        rect = create_axis_aligned_rectangle(a, b)
        IF prepared_polygon.contains(rect):
            max_area = max(max_area, rectangle_area(a, b))

    RETURN max_area
```

---

## Implementation Insights

* **Polygon containment:** Instead of manually marking every green tile, the polygon approach captures both boundary and interior in a single structure.
* **Shapely optimization:** `prep(polygon)` allows fast repeated containment checks, crucial for large inputs.
* **Inclusive counting:** Ensure width and height include both endpoints, matching AoC's definition.
* **Scalability:** Modular design allows easy adaptation for larger grids or different tile rules.

---

## Final Output Format

```text
points = parse_points("DAY09/Movie.txt")
print("Part 1:", part1(points))
print("Part 2:", part2(points))
```

**Example Output:**

```
Part 1: 50
Part 2: 24
```

---

## Why This Solution is SEO-Friendly & Viral

* Covers both **theory and practical code**.
* Includes **step-by-step pseudocode** for beginners and advanced users.
* Highlights Python + Shapely usage, popular among AoC participants.
* Clear explanation of **geometry and grid-based reasoning**.
* Modular and scalable, making it a reference for other AoC challenges.
* Ready for sharing on **blogs, LinkedIn, GitHub, or Dev.to**.

---

## Connect with Me

If you liked this breakdown, check out more of my work:

* **LinkedIn**: [https://linkedin.com/in/raksha-pahariya-409842227](https://linkedin.com/in/raksha-pahariya-409842227)
* **GitHub**: [https://github.com/RP2025](https://github.com/RP2025)

I post daily breakdowns, clean coding approaches, Advent of Code solutions, and tutorials for Python, grids, and algorithmic challenges.
