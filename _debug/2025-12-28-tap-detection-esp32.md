---
layout: post
title: "Tap Detection Almost Broke Me"
date: 2025-12-28
excerpt: "I spent two days debugging a gesture classifier on ESP32 QMI8658 IMU."
---
## What I'm Building

POPO bot touch is an autonomous emotional desk companion/robot. It's a cube-shaped robot with a screen face that responds to how you move it. You can't command it. Instead, it maintains internal "pressure" states (rest, social, curiosity, overstimulation) that shift based on your interactions.
For this project, I am using the ESP32-S3-LCD-1.69 (Waveshare) board, which comes with a QMI8658 IMU.

**Why I needed gesture detection:**
- **Taps** → Build social pressure → POPO shows HAPPY, GIGGLE, SMUG expressions (gifs)
- **Shakes** → Build curiosity pressure → POPO shows SCHEMING expression (gif)
- **7 movements + pause** → Triggers phone finder utility (a feature that will link to my phone)
- **Continuous interaction** → Builds overstimulation → POPO shows ANNOYED expression (gif)

The same tap can mean different things depending on POPO's current mood. But first, I needed to detect what you actually did.

**Since I am not an engineer by trade, Claude is my coding partner.**

## I Thought I Was Done

This is what my testing setup looked like:

![POPO development setup](/assets/images/popo-phase3-dec28.jpg)

I got the QMI8658 accelerometer working fast. Used magnitude-based detection (`sqrt(x² + y² + z²)`) for orientation independence. Set a simple threshold at 1.0: below = tap, above = shake.

Tested it 10 times. **~85% accuracy.** Looked good.

Then I actually started using it.


## Three Things That Broke

### My Keyboard Was Triggering Taps
```
Movement ended - Duration: 42ms, Peak: 0.32
→ Classified as: TAP
```
I wasn't even touching POPO. Just typing. The accelerometer sitting on my desk was counting keyboard vibrations as taps. My phone finder feature (7 taps = activate) kept triggering while I worked.

### I Couldn't Shake It Gently
```
Movement ended - Duration: 153ms, Peak: 0.54
→ Classified as: TAP
```
I picked up POPO and gave it a gentle shake. Peak force: 0.54 (below my 1.0 threshold). The code said "tap." But my hand knew I'd just shaken it — the 153ms duration felt completely different from my usual 50ms taps. The code only looked at force.

### Firm Taps Felt Wrong
```
Movement ended - Duration: 51ms, Peak: 1.12
→ Classified as: SHAKE
```
I tapped POPO deliberately but firmly. Peak force 1.12 (above threshold). Classified as shake. **Now POPO's emotions were backwards.** Firm taps triggered curiosity pressure (SCHEMING) instead of social pressure (HAPPY). I was confusing it by tapping too enthusiastically.


## I Recorded Everything

I spent an afternoon tapping and shaking my board. Recorded 50+ interactions. Copied every Serial Monitor line into a text file. Pasted it into Claude. Stared at the numbers.

**My taps:** 0.42-1.12 force, **always 51-102ms duration**  
**My shakes:** 0.54-1.70 force, **always 150-357ms duration**

The pattern was obvious once I saw it written out: A tap at 1.12 force (51ms) feels completely different from a shake at 1.16 force (204ms). Same force. Different gesture.

I'd been measuring the wrong thing. Force tells you *intensity*. Duration tells you *intention*.


## I Built a Hybrid System

### First: Filter the Noise
```cpp
if (shakeForce > 0.35) {  // Start tracking
  isActive = true;
}

if (peakForce < 0.45) {  // Ignore weak movements
  return;
}
```
This filtered out desk vibrations (0.25-0.4) while keeping deliberate interactions (0.45+). No more false triggers from typing.

### Then: Force + Duration Classification
```
if (peakForce < 0.65) {
  return TAP;  // Gentle = always tap
}
else if (peakForce >= 1.0) {
  return (duration >= 150) ? SHAKE : TAP;  // Strong: check duration
}
else {
  return (duration >= 180) ? SHAKE : TAP;  // Gray zone: check duration
}
```

I tested it again. **~85% → 95% accuracy.**

Then I remembered: This is calibrated for the *bare board.* Once I add the PETG shell, forces will dampen by 20-30%. That 1.12 firm tap becomes 0.78. That 0.54 gentle shake drops to 0.38 (below my noise threshold).

I'd have to recalibrate everything. Two days of work for temporary numbers.


## Then I Realized Something

I was solving the wrong problem.

POPO doesn't need to know *what* I did with perfect precision. It needs to know I *deliberately chose to do it.*

The phone finder doesn't care if movement #3 was a tap or shake. It cares that I did 7 intentional movements **and then stopped.** That pause — that's what proves intent.

### I Added a Confirmation Window
```
// After 7 movements
if (movementCount == 7) {
  waitingForPhoneFinderConfirmation = true;
  confirmationWindowStart = now;
}

// Did they stop? (1 second with no new movements)
if (now - confirmationWindowStart > 1000) {
  triggerPhoneFinder();  // They stopped = intentional
}

// Or did they keep going?
if (waitingForPhoneFinderConfirmation && newMovementDetected) {
  movementCount = 0;  // Just playing, reset
}
```

I tested it:
```
Tap 1, 2, 3, 4, 5, 6, 7 → [I stop for 1 second]
PHONE FINDER ACTIVATED! ✓

Tap 1, 2, 3, 4, 5, 6, 7, 8, 9, 10... [I keep going]
→ Continued tapping, resetting counter ✓
```

**No false triggers. Ever.**

Suddenly shell recalibration wasn't scary anymore. My critical interactions don't depend on perfect classification — they depend on **counting discrete events and detecting pauses.** The noise filter might need adjustment (0.45 → 0.35). That's it.

The hybrid classification still runs (it affects POPO's mood), but interactions rely on **count + intentionality,** not classification accuracy.


## What I Learned

**Gestures are multi-dimensional.**  
I thought force would be enough. It wasn't. I needed force + duration, and sometimes direction, repetition, context matter too.

**Design for forgiveness, not precision.**  
My first design: "Phone finder triggers on exactly 7 taps."  
My second design: "7 movements + you choose to stop."

The pause proves intent. Not the classification.

**Test real usage, not controlled tests.**  
My controlled tests (stable desk, deliberate gestures) gave 85%. Real usage (desk vibrations, casual interactions, different angles) broke it immediately. I should have recorded 50-100 messy real interactions first.

**Temporary calibration still teaches you things.**  
Yeah, the numbers will change after I add the shell. But I learned what dimensions matter, what the failure modes look like, how to structure the decision tree. The *numbers* change. The *logic* doesn't.


## Looking Forward

One month ago, I didn't know what an ESP32 was. I'd never written a line of code. But here I am, building my dream desktop companion while debugging accelerometer thresholds and building autonomous behaviour systems.

AI isn't replacing my thinking—it's amplifying it. Claude is my execution partner, helping me translate ideas into working code while I focus on design decisions and what makes POPO feel alive.

This is what excites me most: **technology is no longer gatekept by credentials.** An art major can build robots. A beginner can ship real hardware. The barrier isn't knowledge anymore—it's willingness to iterate.

If you're reading this and thinking "I could never do that," I promise: you absolutely can. Start messy. Test constantly. Let AI handle the syntax while you handle the thinking.

POPO exists because I didn't wait to become an expert first. 
