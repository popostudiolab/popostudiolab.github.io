---
layout: post
title: "Why Tap Counting Failed on My ESP32 Robot (and What Worked Instead)"
date: 2026-01-09
excerpt: "I tried to make an ESP32 robot count 7 taps using an IMU. What looked like a signal processing problem turned into a lesson about intent, trust, and physical interaction."
---


## The Feature I Thought Was Simple

POPO is an autonomous emotional desk companion I’m building on an ESP32.

The idea was straightforward:
tap it **7 times**, and it finds your phone via Bluetooth.

In early tests — no animations, just serial logs — it mostly worked.
Tap and shake detection using the QMI8658 IMU hovered around **85% accuracy**.

Then I added animated GIF expressions.

Everything fell apart.


## Where Things Started to Slip

### GIFs and Time

The `AnimatedGIF` library blocks the main loop for **100–200ms per frame**.

That doesn’t sound dramatic until you realize:
a tap can be over before the loop runs again.

So taps that happened during animation playback were never seen.
They didn’t fail.
They simply never existed.

### The Body Is Messy

I kept staring at my code, but the real problem wasn’t there.

One physical tap is not one clean signal.

It’s impact, bounce, wobble, vibration.
The desk moves.
The shell flexes.
The hand hesitates.

Debouncing helped — until it didn’t.

Too short, and one tap became three.
Too long, and fast taps collapsed into one.

There was no “correct” value.
Just tradeoffs.

### Categories Blur in Real Life

On paper:
- taps are gentle
- shakes are strong

In reality, the ranges overlap.

Firm taps exceeded gentle shakes.
Grip strength mattered.
USB cables dampened motion.
Typing nearby created noise.

I pushed accuracy to around **85–90%**.

And then I stopped.

Because a phone finder that fails once in ten tries doesn’t feel “mostly working.”
It feels unreliable.


## The Point Where I Paused

At this stage, AI suggestions were getting more sophisticated.

Kalman filters.
FFT analysis.
Template matching.

All valid.
All impressive.
All expensive — in time and attention.

Each path meant weeks of calibration, data collection, tuning.
And even then, I’d still be fighting physics and human behavior.

That’s when I realized something uncomfortable:

I was trying to make the system *precise*,
when what I actually needed was for it to be *trustworthy*.


## Rethinking the Problem

Before the flip gesture, I tried something simpler: **tilting**.

Tilt POPO to the side for a few seconds.
Enter utility mode.

It seemed obvious.
Orientation instead of counting.

It also quietly failed in every way that mattered.

Small hand adjustments retriggered logic.
Returning to neutral overshot.
Emotional idle movement interfered.
The device felt unsure of itself.

More importantly — *I* felt unsure using it.

Did I tilt enough?
Did I tilt too much?
Why did it open again?

I kept adding guards and timers.
Each fix solved one problem and created another.

Eventually it clicked:

Tilt is continuous.
Utilities need discrete intent.


## The Shift That Changed Everything

Instead of asking *how much* POPO was tilted, I asked a different question:

Is POPO clearly in a different physical state?

That’s when I tried flipping it onto its back.

To enter utility mode, POPO must:
- be laid flat on its back for **300ms**
- then be returned upright for **200ms**

This isn’t a gesture.
It’s a transition.

And that distinction mattered more than I expected.

Being on its back is unambiguous.
It doesn’t drift.
It doesn’t bounce.
It survives animation blocking.

Even if the loop stalls, POPO is still on its back when execution resumes.

For the first time, the system felt calm.

When the flip is confirmed:
- a short beep plays
- animations pause
- a simple menu appears

Only then do taps matter.


## Letting Go of Exact Numbers

Inside the menu, I stopped caring about exact counts.

Instead of:
“Was that tap number 7?”

The system asks:
“Was that a little tapping or a lot?”

The rules became:
- **1–4 taps → Fortune**
- **5+ taps → Phone Finder**

The UI hints:
- `2 = Fortune`
- `7 = Phone`

It’s guidance, not enforcement.

Internally, small miscounts don’t matter anymore.
Intent still gets through.


## One More Quiet Decision

While selecting a utility, POPO stops being expressive.

No emotional reactions.
No annoyed responses.
No GIF playback.

Just input.

For a moment, it’s a tool.
Then it returns to being a character.

That separation turned out to be essential.


## What Happened After

In real use:

- I tapped twice → it counted twice → Fortune worked
- I tapped about seven times → it counted six → Phone Finder still worked

Even when the numbers were “wrong,” the outcome felt right.

That was new.


## What This Changed for Me

I started thinking this was a signal processing problem.

It wasn’t.

It was an interaction problem — shaped by physics, time, and trust.

AI was excellent at offering smarter algorithms.
It was less helpful at saying:
*maybe you don’t need them.*

The most important question turned out not to be:
“How do I detect this more accurately?”

But:
“What signal already survives the real world?”

For this hardware, the answer wasn’t taps.

It was orientation.


*Want to see the actual process? [Learn about POPO Bot Touch ](/debug/)*

**Hardware:** ESP32-S3-LCD-1.69, QMI8658 IMU  
**Reflection:** Robust interaction beats precise detection
