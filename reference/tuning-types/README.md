# Sims 4 Tuning Type Reference

This section is an expandable reference for tuning types and tuning-related systems used by The Sims 4.

You do not need to read these pages in order.

Use this section when you encounter something in EA tuning and want to know:

> What is this?

> What does it actually do?

> Why is it here?

> What other tuning does it connect to?

---

# How to Use This Reference

Each tuning page is designed in layers.

If you only need a quick answer, read the beginning.

If you want to understand how the system works, keep going.

If you need technical details, XML examples, edge cases, or troubleshooting information, those sections will be further down the page.

---

# Documentation Status

We will use these markers while building the library:

| Symbol | Meaning |
|---|---|
| ✅ | Documented |
| 🟡 | Draft / still expanding |
| ⬜ | Not documented yet |
| 🔍 | Needs further investigation |
| 🔄 | Needs re-verification after a game update |

A page being marked as documented does **not** mean it can never be improved.

The Sims 4 changes over time, and this reference should change with it.

---

# Core Tuning Types

These are some of the tuning concepts you are likely to encounter frequently.

| Status | Tuning Type |
|---|---|
| ⬜ | Buff |
| ⬜ | Trait |
| ⬜ | Statistic |
| ⬜ | Commodity |
| ⬜ | Interaction |
| ⬜ | Loot |
| ⬜ | Snippet |
| ⬜ | Recipe |
| ⬜ | Situation |
| ⬜ | Situation Goal |
| ⬜ | Aspiration |
| ⬜ | Career |
| ⬜ | Service |
| ⬜ | Sim Filter |

This is **not yet a complete list**.

The index will expand as we inventory and document the game's current tuning.

---

# Browse by Area

## Sims and Sim State

Examples include:

- Buffs
- Traits
- Statistics
- Commodities
- Motives
- Skills
- Relationship-related tuning

---

## Interactions and Behavior

Examples include:

- Super interactions
- Mixer interactions
- Social interactions
- Tests
- Loot
- Autonomy-related tuning
- Interaction outcome systems

---

## Crafting and Production

Examples include:

- Recipes
- Recipe lists
- Ingredients
- Crafting-related tuning
- Result objects

---

## Situations and Goals

Examples include:

- Situations
- Situation goals
- Goal sets
- Roles
- Role states
- Scoring systems

---

## Progression and Rewards

Examples include:

- Aspirations
- Careers
- Milestones
- Rewards
- Unlock systems

---

## World and Simulation Systems

Examples include:

- Services
- Sim filters
- Population systems
- Venue-related tuning
- Scheduling systems
- World-related systems

---

# Every Reference Page Should Answer

Whenever possible, each tuning reference should explain:

1. **What is it?**
2. **What does it do?**
3. **Where will I encounter it?**
4. **How does it connect to other tuning?**
5. **What commonly references it?**
6. **What does it commonly reference?**
7. **What important tunables might I see?**
8. **What does its XML usually look like?**
9. **How might a modder use or modify it?**
10. **What are common mistakes or misconceptions?**
11. **What questions do beginners commonly have about it?**

---

# Page Format

Most tuning pages will follow this general structure:

```text
In Plain English

Mental Model

What Does It Do?

Where Will I See It?

How It Connects to Other Tuning

What References It?

What Does It Reference?

Important Concepts / Tunables

Reading the XML

Common Modding Uses

Common Mistakes

FAQ

Advanced Notes

If You Remember Only 3 Things
```

Not every tuning type will need every section.

The structure should serve the explanation, not the other way around.

---

# Accuracy and Verification

Whenever possible, technical pages should include:

```text
Last verified:
Game version:
Pack requirements:
Status:
```

Information should be checked against current game tuning whenever possible rather than relying only on old tutorials or forum posts.

---

# A Note About Names

The names modders use for systems do not always perfectly match the names used internally by the game.

Some concepts are:

- tuning resource types
- tuning classes
- snippet types
- tunable structures
- systems implemented partly in Python
- community shorthand

This reference will try to clearly identify those distinctions when they matter.

---

# Start Here

If you are new to tuning, start with the course first:

[Module 01 — Tuning Foundations](../../modules/01-tuning-foundations/README.md)

If you are here because you found a specific tuning type, use this index and jump directly to the relevant page.
