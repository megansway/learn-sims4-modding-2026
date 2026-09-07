# Module 01 — Tuning Foundations

This module introduces the core concepts you will see repeatedly while reading and editing Sims 4 tuning.

You do **not** need to memorize every tuning type here.

The goal is to learn how the pieces fit together so that when you encounter unfamiliar tuning later, you have a framework for understanding it.

---

## What You Will Learn

By the end of this module, you should understand:

- what tuning resources are
- how instance IDs and references work
- why Sims 4 tuning is highly interconnected
- what snippets are
- what loot actions are
- what tests do
- how buffs, traits, statistics, commodities, recipes, and interactions differ
- how to trace behavior across multiple tuning resources

---

# The Big Picture

The Sims 4 is not usually defined by one giant file that contains everything about a feature.

Instead, gameplay is often built from many smaller tuning resources that reference one another.

A simplified example might look like this:

```text
Interaction
    |
    +--> Tests
    |
    +--> Recipe
    |
    +--> Loot
           |
           +--> Buff
           |
           +--> Statistic
           |
           +--> Trait
