---
layout: post
title: "Coding an Arduino Project as a Non-Engineer: My Experience Using ChatGPT, Claude, and Antigravity"
date: 2026-01-21
excerpt: "I built this Arduino project almost entirely with ChatGPT and Claude. This post documents the confusion, coupling, and refactoring pain I actually lived through as a beginner."
---

## Working on an Arduino Project Without an Engineering Background

For most of this project, I didn’t sit down and write code line by line.

I talked.

I brainstormed with ChatGPT and Claude, asked for code, pasted it into Arduino, compiled, fixed errors, and repeated. This wasn’t a shortcut — it was the only way I could keep moving as a non-engineer building something physical.

At the beginning, this approach worked better than I expected.

## Starting With One Large Arduino File

Before I understood anything about software architecture, my Arduino project naturally became:

- one very large `.ino` file  
- a few small headers for hardware or assets  
- everything important living in the same place  

At the time, this felt manageable. I could scroll, search, and vaguely remember where things were. As long as the project stayed small, this didn’t feel like a problem.

But the project didn’t stay small.

## How the File Slowly Became Unmanageable

POPO went through many stages of refinement. I added and removed features, rewrote behavior rules, changed timing logic, and replaced entire interaction ideas.

The file didn’t just grow longer — it became tangled.

Over time, I noticed things like:

- state logic touching display logic  
- utilities sharing variables with emotion timing  
- timers reused for completely different purposes  

Nothing failed loudly.  
But when behavior felt “off,” I couldn’t tell *where* to look.

Every bug felt like it might be anywhere.

## Debugging by Copy-Pasting Into AI Chats

When something didn’t work, my process looked like this:

1. Copy a large chunk of Arduino code  
2. Paste it into ChatGPT or Claude  
3. Ask why something was happening  
4. Apply a suggested fix  
5. Compile again  

This worked — but only up to a point.

As the file grew, this became exhausting. Each time, I had to decide *how much context* to paste. Too little, and the answer missed something. Too much, and I lost track of what I was even asking about. Since I am not a traditional coder, even code patching could be a challenge.

I wasn’t just debugging code.  
I was struggling to *see* the structure of my own project.

## Using Antigravity to See the Code Differently

When I started using Antigravity, the biggest change wasn’t that it “fixed” anything.

It showed me where things lived.

Instead of treating the project as one long scroll, I could see responsibilities separated into smaller pieces. Plus, before Arduino, I was using AI to code in VS Code so the IDE felt familiar.
Antigravity can see the entire folder and assess it better than I can copy and paste everything. I tried AI in VS Code before, but for some reason, Antigravity IDE felt easier to use.

Antigravity also generated detailed implementation notes during refactoring. I didn’t blindly accept them. I reviewed them with ChatGPT, asked questions, approved changes, and then applied them.

This was the first time I felt like I could *observe* what was changing.

## One Actual Mistake I Made

The problem wasn’t Antigravity behaving strangely.

It was my misunderstanding of how **Arduino treats a project**.

Arduino doesn’t really have a strong concept of a “project” in the way modern IDEs do. What matters is the **sketch folder** that contains the `.ino` file. That’s the version Arduino will compile — regardless of how many copies exist elsewhere.

I copied my code into a separate Antigravity folder, opened that folder in Antigravity, assuming I was safely working on a duplicate. But Arduino was still opening and compiling the original sketch folder.

I didn’t notice this at first.

So while I thought I was refactoring a copy, I was actually changing the version I meant to keep untouched.

That was a real mistake, and it took time to sort out.


## One Thing I Didn’t Understand Yet

The second issue wasn’t a mistake — it was a misunderstanding.

Antigravity isn’t Arduino-native. It doesn’t compile Arduino code. Arduino doesn’t know anything about Antigravity.

So whenever I hit compile errors, I had to:

- copy error messages from Arduino  
- paste them into Antigravity  
- explain what failed  
- sync fixes back manually  

At the time, this felt clumsy and slow. Looking back, it was simply a boundary I hadn’t understood yet. Each tool was doing exactly what it was designed to do — I just hadn’t learned how to place them cleanly in the workflow.

## What Changed After Refactoring the Large File

After breaking the large `.ino` file into smaller, more focused files, something shifted.

Instead of thinking about how I am going to improve my code with in a big file, I have more freedom to change the modules and think about the modules seperately.
This is something I did not know about Coding and AI coding-it is better to have everything modular and improve my prototypes from there.

When something behaved strangely, I at least knew *where* it probably lived. I could open one file instead of scanning everything. 

## Why I’m Writing This Down

This post is a record of what I actually went through while coding an Arduino project as a beginner, relying heavily on AI tools, and slowly realizing where that approach breaks down.

The code is still evolving.

But this is the point where the project stopped feeling like a blur and started feeling like something I could reason about — even without an engineering background.
