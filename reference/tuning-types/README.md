# Sims 4 Tuning Type Reference

This section is a growing reference for tuning types and tuning-related systems used by The Sims 4.

You do not need to read these pages in order. Use this section when you encounter something in EA tuning and want to know what it is, what it does, why it is there, and how it connects to other tuning.

---

## How to Use This Reference

Each tuning page is designed in layers. If you only need a quick answer, read the beginning. If you want to understand how the system works, keep going. If you need technical details, XML examples, edge cases, or troubleshooting information, those sections will be further down the page.

The goal is to make each page useful whether you are reading for thirty seconds or studying the system in depth.

---

## Documentation Status

The reference library uses these symbols to show the current state of each page:

| Symbol | Meaning |
|---|---|
| ✓ | Up-to-date  |
| ◆ | Draft |
| ■ | Planned |
| ▲ | Needs investigation |
| ↻ | Needs re-verification |

A page marked as documented can still be improved later. The Sims 4 changes over time, and this reference should change with it.

---

## Core Tuning Types

These are some of the tuning types and tuning-related systems you are likely to encounter frequently while reading EA tuning.

| Status | Tuning Type |
|---|---|
| ◆ | Buff |
| ◆ | Trait |
| ◆ | Statistic |
| ◆ | Commodity |
| ◆ | Interaction |
| ◆ | Loot |
| ◆ | Snippet |
| ◆ | Recipe |
| ◆ | Test |
| ■ | Situation |
| ■ | Situation Goal |
| ■ | Aspiration |
| ■ | Career |
| ■ | Service |
| ■ | Sim Filter |

> [!NOTE]
> This is not yet a complete list of every tuning type or tuning-related system in the game. The index will expand as the library grows and as current game tuning is inventoried.

---

## Browse by Area

### Sims and Sim State

This area covers tuning that describes or tracks a Sim's state, characteristics, progress, or internal values.

Examples include:

- buffs
- traits
- statistics
- commodities
- motives
- skills
- relationship-related tuning

### Interactions and Behavior

This area covers tuning that controls what Sims can do, when they can do it, and what happens as a result.

Examples include:

- super interactions
- mixer interactions
- social interactions
- tests
- loot
- autonomy-related tuning
- interaction outcomes

### Crafting and Production

This area covers systems where Sims create, prepare, assemble, or produce something.

Examples include:

- recipes
- recipe lists
- ingredients
- crafting interactions
- result objects

### Situations and Goals

This area covers structured gameplay scenarios and the systems that control their participants, goals, progression, and completion.

Examples include:

- situations
- situation goals
- goal sets
- roles
- role states
- scoring systems

### Progression and Rewards

This area covers tuning related to long-term progress, unlocks, rewards, and structured development.

Examples include:

- aspirations
- careers
- milestones
- rewards
- unlock systems

### World and Simulation Systems

This area covers tuning that helps manage the wider simulation rather than a single interaction or Sim state.

Examples include:

- services
- Sim filters
- population systems
- venue-related tuning
- scheduling systems
- world-related systems

---

## What Every Reference Page Should Explain

Whenever possible, each tuning reference should answer these questions:

1. What is it?
2. What does it do?
3. Where will I encounter it?
4. How does it connect to other tuning?
5. What commonly references it?
6. What does it commonly reference?
7. What important tunables might I see?
8. What does its XML usually look like?
9. How might a modder use or modify it?
10. What are common mistakes or misconceptions?
11. What questions do beginners commonly have about it?

Not every tuning type will need every section, but these questions give the library a consistent framework.

---

## How Reference Pages Are Structured

Most tuning pages will follow a format similar to this:

```text
In Plain English

Mental Model

What Does It Do?

Where Will I See It?

How It Connects to Other Tuning

What References It?

What Does It Reference?

Important Concepts and Tunables

Reading the XML

Common Modding Uses

Common Mistakes

FAQ

Advanced Notes

If You Remember Only 3 Things
```

The order may change when another structure makes more sense for the topic.

> [!IMPORTANT]
> The structure should serve the explanation. Not every tuning type needs to be forced into the exact same template if doing so would make the page harder to understand.

---

## How We Use Callouts

Reference pages may use GitHub's built-in alerts when information deserves extra attention.

### Notes

Notes provide useful context, terminology, exceptions, or clarification.

```md
> [!NOTE]
> Helpful context goes here.
```

### Tips

Tips provide practical modding advice, shortcuts, or troubleshooting suggestions.

```md
> [!TIP]
> Helpful advice goes here.
```

### Important

Important alerts highlight core information that should not be skipped.

```md
> [!IMPORTANT]
> Important information goes here.
```

### Warnings

Warnings are used when something may cause conflicts, breakage, or other problems.

```md
> [!WARNING]
> A possible problem goes here.
```

### Caution

Caution alerts are used when something should be changed carefully or verified before proceeding.

```md
> [!CAUTION]
> Information requiring extra care goes here.
```

Callouts should be used selectively. If everything is highlighted, nothing feels important.

---

## Accuracy and Verification

Whenever possible, technical reference pages should include:

```text
Last verified:
Game version:
Pack requirements:
Status:
```

Information should be checked against current game tuning whenever possible rather than relying only on older tutorials, forum posts, or community explanations.

> [!NOTE]
> Older information can still be useful, but it should be treated as something to verify rather than automatically assumed to still be correct.

---

## A Note About Names

The terms modders use for a system do not always match the game's internal terminology perfectly.

A concept might be described as a:

- tuning resource type
- tuning class
- snippet type
- tunable structure
- Python-backed system
- community shorthand term

When that distinction matters, the reference page should explain it. This is especially important because two people may use the same word while referring to slightly different things.

---

## How Tuning Connects

One of the most important goals of this reference library is to show how systems connect to one another.

A tuning page should not only answer:

> "What is this?"

It should also help answer:

> "Why did I encounter this while looking at something else?"

For example:

```text
Interaction
    |
    +--> Tests
    |
    +--> Loot
    |      |
    |      +--> Buff
    |      +--> Statistic
    |      +--> Trait
    |
    +--> Recipe
```

Understanding these relationships is often more useful than memorizing individual XML fields.

---

## Start Here

If you are completely new to tuning, start with:

[Module 01 — Tuning Foundations](../../modules/01-tuning-foundations/README.md)

If you are here because you found a specific tuning type, use this index and jump directly to the relevant reference page.
