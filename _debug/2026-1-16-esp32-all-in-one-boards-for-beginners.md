---
layout: post
title: "ESP32 All-in-One Boards for Beginners: Why All-in-One Boards Work Better for AI-Assisted Prototyping"
date: 2026-01-16
excerpt: "A beginner’s perspective on choosing all-in-one ESP32 boards over modular hardware when working with AI coding assistants."
---

## What I Was Choosing Between

I'm building POPO—an autonomous desk companion. I'm not an engineer. My coding partner is Claude.

<div class="youtube-container">
  <iframe 
    src="https://www.youtube.com/embed/Lmhfj56NkSQ"
    title="POPO development short"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen>
  </iframe>
</div>

**If the video doesn’t appear here, you can watch it on YouTube:**
Here: https://youtube.com/shorts/Lmhfj56NkSQ

When I started planning the hardware, I compared two approaches:

**Modular parts**
- XIAO ESP32-S3 microcontroller
- Separate 1.69" LCD display
- QMI8658 motion sensor
- Female-to-female jumper wires


**All-in-one**
- Waveshare ESP32-S3-LCD-1.69: everything integrated: Display, sensors, charging, USB on one 30×40mm board


On paper, modular seemed obviously better. More flexible. "Real engineering." I could swap components if something broke. Learn how each part worked.

Then I realized I was asking the wrong question.

## The Wrong Question

**What I asked:** "Which components do I need?"

**What I should have asked:** "What kind of setup will work with a non-engineer beginner with AI as the co-developer?"

AI can write code. AI can debug logic errors. AI can optimize timing.

But AI cannot tell you if wire #7 came loose or diagnose whether your sensor isn't responding because of bad wiring or wrong I2C address.

When you're working with AI as a development partner, hardware choice affects the collaboration model.

## What Workflow Actually Looks Like

**With the all-in-one board:**

AI gives me two possibilities:
- Board defective? (test with manufacturer's example code)
- Code wrong? (here's what to fix)

If example code works → my code is the problem. AI can fix that.
If example code fails → board is broken. Replace it.

No ambiguous middle ground of "maybe something came unplugged."

## The Library Problem

When I received the Waveshare ESP32-S3-LCD-1.69 in early December, I downloaded the sample code and tested the board with the help of AI. However, Compiler error: `SensorLib.h: No such file or directory 'happened.

Checked: library installed ✓. Still didn't work.

**If I were to make everything with modular parts, this could mean:**
- Wrong library version
- Bad wiring is affecting detection
- I2C address mismatch
- Breadboard connection issue
- Pin assignment wrong

**But with all-in-one, this could only mean:**
- Example code outdated OR library structure changed

I then dug into the library folder. Found two files: `SensorLib.h` (base class) and `SensorQMI8658.hpp` (actual sensor). I asked AI ahbout it and it turns out the manufacturer examples used old syntax.

So we changed two lines. Compiled clean. The sensor worked.

**20 minutes total.**

With modular, I'd have spent 20 minutes just checking wiring before even looking at the library.

## One Month Later

It's now January. POPO has been in active development for one full month.

**What I've developed with AI's help:**
- Tap detection timing 
- Emotion selection and rules
- GIF animation speed calculations
- Gesture detection threshold tuning
- Flip gesture reliability problems
and more.

**What I've never debugged:**
- Loose wire connections
- I2C address conflicts
- Power supply issues
- "Which pin did I connect this to?"
- Sensor not detected on bus
- Hardware connection problems

Every development session was in code. Most problems had a solution that AI could help me implement in code, and this is a big win for a beginner in electronics.


## The Trade-off

**What I gave up:**

Component-level repair. If the display breaks, I replace the whole board instead of just the display. 

Electronics education. Didn't learn about pull-up resistors, signal timing or voltage regulation. This matters if your goal is to become an electrical engineer. Doesn't matter if your goal is building a working desk companion.

Future flexibility. Can't easily add different sensors later. Limited to what's on the board. But four months in, I haven't needed anything the board doesn't have: display, IMU, RTC, buzzer, battery charging.

**What I got:**

**Faster prototype.** POPO's brain was functional in 2 weeks. State machine, emotions, sensor detection, all working. If I'd spent those 2 weeks wiring everything, I'd still be fighting hardware instead of designing personality.

**Confidence in foundation.** When something doesn't work, I know it's code. Psychologically powerful when learning. I don't second-guess "maybe I wired it wrong 3 weeks ago." The hardware layer is stable. I focus on behavior.

**Time on what matters.** I wanted to build a creature-like autonomous companion. That's about timing, personality, and emotional continuity. Not I2C bus topology. The all-in-one board let me spend 4 months iterating on what makes POPO feel alive.

## The Insight

When you're building with AI, integrated boards keep you in the domain where AI can help: writing code that brings your idea to life.

All-in-one boards aren't about avoiding complexity. They're about choosing which complexity you engage with.

If you're starting a hardware project with AI as your coding partner, ask yourself: what do I want to spend my debugging time on? Personality tuning and feature design? Or wire connections and component compatibility?

Both are valid. They're just different goals.

POPO exists because I chose to focus on behavior, not breadboards.

*Want to see the actual process? [Learn about POPO Bot Touch ](/debug/)*
