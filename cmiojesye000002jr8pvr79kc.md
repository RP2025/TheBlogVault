---
title: "Solving Advent of Code Day 1: Find the Secret Entrance"
seoTitle: "Code Challenge: Advent of Day 1"
seoDescription: "Explore Advent of Code challenges with detailed solutions and code walkthroughs. Discover problem-solving insights and efficient coding strategies"
datePublished: Tue Dec 02 2025 12:11:23 GMT+0000 (Coordinated Universal Time)
cuid: cmiojesye000002jr8pvr79kc
slug: solving-advent-of-code-day-1-find-the-secret-entrance
canonical: https://medium.com/p/404ba45e457a
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764677365861/513ed2bd-09a3-46f5-841b-1889af51b6e0.png
tags: python, programing, advent-of-code, adventofcode2022, adventofcode2025

---

It's that time of year again! For developers, December isn't just about hot cocoa and holiday lights; it’s about **Advent of Code (AoC)**, the annual series of festive programming puzzles that challenge our logic, coding skills, and sometimes, our sanity. 😅

I've been tackling this year's challenges, and I've decided to document my journey and share my solutions. AoC is more than just solving a puzzle; it's an opportunity to learn new tricks, explore different data structures, and refine your problem-solving muscle.

In this article, I'll walk you through my thought process for some of the earlier challenges and explain the code I developed to solve them. All of my solutions, including the full code for the challenges discussed here, are available on my GitHub repository.

You can find all the code for my Advent of Code solutions here: [\[RP2025/AdventOfCode\]](https://github.com/RP2025/adventofcode)

## **DAY 01 : SECRET ENTRANCE**

The challenge is to track a position on a 100-unit circular track, starting at position 50, given a series of directional moves (e.g., L5, R120). The task is to count how many times we either land exactly on position 0 (Part 1) or pass through it (Part 2).

### **Part 1: Direct Simulation with Modulo**

***Goal:*** Count instances where a move finishes exactly at position  
***Approach:*** We leverage the modulo operator (%) to handle the circular nature of the track. For each move:

1. **Update the position:** Add or subtract the distance based on the direction.
    
2. **Apply modulo 100:** This ensures the position wraps correctly around the track.
    
3. **Check for zero:** If the resulting position is `0`, we increment our counter.
    

No intermediate steps need to be tracked, only the final position after each move matters. This makes the solution both simple and efficient.

### **Part 2: Accounting for Multiple Crossings (Optimized)**

***Goal:*** Count every time the path crosses position 0, including multiple crossings within a single long move.  
***Approach:*** Instead of simulating each individual step, we analyze the distance mathematically:

1. **Full cycles:** Using integer division (//), we determine how many complete 100-unit laps the move covers. Each full cycle guarantees a crossing at position 0, so we immediately add that to our count.
    
2. **Partial move check:** Using the modulo operator (%), we calculate the remaining distance that does not complete a full cycle. Then, we check if this partial move is enough to cross position 0 from the current position. If it is, we count one additional hit.
    
3. **Update the position:** Finally, we apply the total move modulo 100 to set the correct starting point for the next move.
    

This approach efficiently handles long moves without iterating through each unit, while still counting every crossing accurately.

If you’d like to explore my full solutions or see how I approached other days of Advent of Code, you can check out the complete code here:

👉 **GitHub Repository:** [\[RP2025/AdventOfCode\]](https://github.com/RP2025/adventofcode)  
👉 **Connect with me on LinkedIn:** [*RAKSHAPAHARIYA*](https://www.linkedin.com/in/rakshapahariya/)

I’ll continue documenting my approaches throughout the month,hope you follow along, and feel free to share your own solutions too!