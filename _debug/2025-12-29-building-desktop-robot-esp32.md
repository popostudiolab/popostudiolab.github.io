---
layout: post
title: "Building a Desktop Robot Without an Engineering Degree: ESP32 and AI"
date: 2025-12-29
excerpt: "I'm building an autonomous emotional robot called POPO using ESP32-S3 hardware and AI as my coding partner. No soldering, no formal programming training—just curiosity and accessible tools."
---

## I've Always Wanted a Desktop Robot

Not just any robot. One that could sense my emotions and react to them. One that reflected my aesthetic, not someone else's mass-produced design. Something quiet, small, and alive in its own way.

My inspirations were Dasai Mochi (those soft, expressive creatures) and various AI bots I'd seen online. I loved their personality, their unpredictability, their refusal to just be tools.

But I never thought I could actually build one myself.

## The Realization

One day I discovered I could code phone apps with AI as my partner. Not vibe coding where you paste templates, but actually creating an app from scratch with AI handling the execution while I designed every detail.

That's when the idea of POPO came to me.

If I could build apps this way, why not robots? 

I saw the possibility: I could create something I'd feel proud of. Something that could grow with me, because I coded it. Not a kit someone else designed. Not a tutorial I followed. Mine.

## The Vision: Multiple POPO Bots, Each with Unique Personalities

After learning what ESP32 boards could do, the vision took shape. I would create several POPO bots, each with distinct personalities and interaction methods.

POPO Bot Touch would be first: 100% movement-based interaction. No touchscreen. No voice. Just something that sits quietly as a companion, responding only to how you move it.

I would 3D print the shells and accessories (with my partner's help on the technical printing side). The entire thing would be handmade, from code to case.

## The Hardware Problem

I searched "how to make my own desktop robot" and found video after video showing circuit design, soldering, breadboards, component sourcing.

I felt overwhelmed.

All that hardware work before I could even start coding? As a beginner with no engineering background, I wasn't sure where to begin.

Then I thought: what if there's a board that already has most of the functions I want? What if I could just plug in a battery and start coding?

## Finding the Right Hardware: ESP32-S3 for Beginners

After searching on Amazon with keywords like "ESP32 display board", "ESP32 motion sensor", "all-in-one ESP32 development board", I found it:

**Waveshare ESP32-S3-LCD-1.69**

This board had everything I needed already assembled:
- ESP32-S3 chip (WiFi and Bluetooth built-in)
- Motion sensors (QMI8658 IMU)
- 240×280 color display
- Battery connector with no soldering required
- USB-C for power and programming

I could focus on building the robot's behavior instead of learning circuit design.

## Validating the Approach with AI

I started chatting with Claude to confirm everything would work technically.

- Can this board detect taps and shakes?
Yes.  
- Can I display animated GIFs on the screen?
Yes.   
- Can it connect to my phone via Bluetooth?
Yes.  
- Can I build everything without an expert understanding of the hardware?
Yes.  

I decided then: I would build POPO and document everything publicly.

That's how this site came into being.

## How AI Changed What's Possible for Non-Engineers

I could not have dreamed of building my own robots if AI hadn't become widely available.

I believe AI is a technology that develops our abilities and intuition rather than replacing them. It amplifies what we can imagine and create.

Before AI as a coding partner, "build a desktop robot" meant:
- Years of electrical engineering study
- Learning circuit design from scratch
- Mastering embedded systems programming
- Or giving up and buying someone else's kit

With AI as a learning partner:
- I design the robot's behavior and personality
- I make the creative and interaction design decisions
- I solve the conceptual problems
- AI helps me execute the technical implementation I'm still learning

The barrier isn't knowledge anymore. It's willingness to iterate and learn in public.

## For Anyone Who Thinks They Can't Do This

If you're reading this and thinking you could never build your own robot, I want you to know: I'm not an engineer. One month ago, I didn't know what an ESP32 was. I had never written embedded systems code.

But here I am, building an autonomous emotional desk companion while debugging accelerometer thresholds and designing pressure-based behavior systems.

By documenting my progress here, I hope to show what becomes possible when you combine curiosity with accessible tools and AI as a learning partner.

You don't need an engineering degree to build hardware. You need to be willing to start before you're ready.

Start messy. Test constantly. Let AI handle the technical execution while you handle the vision and design.


*Want to see the actual process? [Learn about POPO Bot Touch ](/debug/)*
