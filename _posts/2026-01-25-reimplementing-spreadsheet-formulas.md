---
layout    : posts
title     : "Reimplementing spreadsheet formulas"
author    : Dennis Lim
date      : 2026-01-25 22:30:00 +0900
categories: computer science
---

New project this month and it is the most fun I have had with code in a while. We are building a calculation engine that evaluates spreadsheet formulas. Not a spreadsheet. The engine underneath one.

Why a tax company needs this: the domain experts, the accountants, think in spreadsheets. Every calculation they know how to do, they have a sheet for. When product asks "how is this tax computed" the answer is a workbook with forty tabs and formulas that reference each other across all of them. Translating that into code by hand is slow, error prone, and every year the law changes and the sheet changes and the code drifts from the sheet.

So instead: make the sheet the source of truth. Build an engine that reads the workbook, understands the formulas, and computes the same answers the spreadsheet would. The accountants maintain the sheet. The engine runs it. No translation step.

This sounds simple until you start listing what "understands the formulas" means.

References. A1, ranges, cross sheet references, named ranges. Relative and absolute. Easy.

Functions. SUM, IF, VLOOKUP, INDEX, MATCH, and then a long tail. The tail is where the accountants live. Every sheet uses some function I had never heard of and every one of them has edge case behavior that the spreadsheet application decided in 1995 and never documented.

Array formulas and spill. A formula that returns a range instead of a value, and fills the cells below and to the right. The rules for how far it spills, and what happens when it hits something, are subtle and I have already been wrong about them twice.

Errors. #VALUE!, #REF!, #N/A, and the way they propagate. A cell that errors poisons everything that references it, except when a function specifically catches it. Getting error propagation to match exactly is most of what "the same answers" means.

Dependency order. Cells reference cells reference cells. Build the graph, topologically sort, evaluate. Detect cycles. Handle the case where the graph changes because a formula's references depend on a value.

The core is in Rust. Correctness first, and then it turned out the performance was there too. There is a TypeScript layer for the product and a small debugger app so the accountants can step through a calculation and see where their answer diverges from ours. That debugger has been the most valuable piece so far. Every divergence it shows is a rule I did not know.

How I am testing it: regression cases from real sheets. Take a real workbook, take the answers the spreadsheet gives, assert we match. The suite grows every time someone finds a difference. It is at a few hundred cases and I expect thousands by summer.

I am building this with an agent doing most of the typing. That is a separate post. What I will say here is that the regression suite is what makes that possible. Without a thousand cases that say "this is the right answer", neither I nor the agent would know when we broke something. Tests were always the thing that let you move fast. Now they are the thing that lets anything move at all.

Formula engines are a solved problem in the sense that several exist. They are an unsolved problem in the sense that none of them match the spreadsheet on our sheets. The gap between those two sentences is my year.
