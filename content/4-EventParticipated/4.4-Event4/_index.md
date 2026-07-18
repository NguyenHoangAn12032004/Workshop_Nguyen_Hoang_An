---
title: "Event 4: Cloud Architect - Game Show"
date: 2026-06-20
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---
# Cracking Case Studies Under Pressure: My Experience at the Cloud Architect Game Show

Most tech events follow a predictable pattern: a slide deck, a speaker, and a quiet Q&A. The **Cloud Architect Game Show**, organized by the AWS Study Group, was the complete opposite. It was a high-octane, competitive arena where 8 teams of randomly paired contestants had to solve complex AWS system design problems under a ticking clock. 

Participating as a contestant wasn't just a test of what I knew—it was a lesson in how to think, collaborate, and make decisions under immense time pressure.

![Cloud Architect Game Show](/images/4-EventParticipated/event4-gameshow.png)

---

## The Adrenaline of the Head-to-Head Arena

The energy in the room was palpable. Five of us, who had never met before the event, were grouped into a team and immediately thrown into a knockout tournament format. 

We were paired against another team, taking turns answering case studies that grew progressively harder. Knowing that a tie at the end of the round would trigger a high-risk "Sudden Death" tie-breaker question kept our nerves on edge. Every second counted, and there was no room for hesitation.

### The Decoupling Debate: SQS vs. Kinesis
One of our most memorable moments came during a question about handling order processing bottlenecks on Black Friday. The prompt asked how to decouple microservices without losing a single message. 
Under the ticking clock, our team entered a rapid-fire debate: *Should we use Amazon SQS or Amazon Kinesis?* 
*   "SQS is simple, point-to-point, and guarantees delivery," one teammate argued.
*   "But what if we need to replay the stream or have multiple consumers?" another countered.

We had to quickly strip away ambiguity, list the trade-offs, and align on SQS because the requirement focused on simple decoupling rather than real-time stream processing. Making that choice together in less than 30 seconds was a massive rush.

---

## Tactical Risk Management: Hope vs. Defense

What made the game show incredibly engaging was the addition of strategy cards. We had to manage risks in real time, deciding when to play defensively and when to go on the offensive.

*   **Playing it Safe (Minimum Risk):** We played this card on a complex network configuration question where we had too many unconfirmed assumptions. If we got it wrong, we wouldn't lose points; if we got it right, we'd get half points. It saved our scoreboard when we realized our initial logic was slightly off.
*   **The Big Gamble (Star of Hope):** In a later round, faced with a decoupling scenario we felt highly confident in, we activated the *Star of Hope*. It was a high-stakes moment—a correct answer meant double points (x2), but a mistake meant a massive double deduction. The relief and excitement when our answer lit up green on the screen was the high point of our tournament.

---

## What I Took Home

Beyond the scoreboard, the game show left me with three profound architectural lessons:

1.  **No Room for Ambiguity:** Under pressure, choosing a service just because it "sounds right" is a quick way to fail. You must identify the exact constraints of the problem and verify your assumptions.
2.  **Surgical Communication beats Fast Talking:** When you only have seconds to talk to your team, you learn to skip filler words. We practiced direct, logic-driven exchanges: verifying the limits of the service and mapping them to the requirements.
3.  **Minimalism Wins:** The best architecture is the simplest one that solves the problem. We learned not to over-engineer solutions with complex file systems (like EFS) when simple static storage (like S3) is all the business case called for.

## Bringing the Game Show to Daily Work

This experience changed how I approach my daily engineering tasks:
*   **Treating Design as a Case Study:** Before integrating a new AWS service into a project, I now list verification criteria (load, cost, latency constraints) as if I'm defending a choice on the game show stage.
*   **Structured Problem Solving:** When a bug or architectural bottleneck occurs, I avoid guessing based on intuition. Instead, I break the issue down into clear, testable assumptions, validating them one by one.

It was an unforgettable, high-energy playground that proved cloud architecture isn't just about reading documentation—it is about analytical thinking, teamwork, and making the right trade-offs when the pressure is on.
