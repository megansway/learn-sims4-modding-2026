# Course Map

This course is designed to teach Sims 4 modding from the ground up, with an emphasis on understanding how tuning works, how resources connect, and how to investigate gameplay systems without getting lost.

You can follow the modules in order or jump to the section you need.

---

## Module 00 — Start Here

[Open Module 00](modules/00-start-here/README.md)

This module introduces the basic vocabulary and concepts you will see throughout the course.

You will learn:

- what a Sims 4 mod is
- package mods vs script mods
- what tuning is
- what XML is
- how Sims 4 resources connect to one another
- which tools you are likely to encounter
- how the course is organized

---

## Module 01 — Tuning Foundations

[Open Module 01](modules/01-tuning-foundations/README.md)

This module introduces the core tuning concepts that appear repeatedly throughout the game.

You will learn about:

- tuning resources
- instance IDs
- references
- snippet tunings
- loot actions
- tests
- buffs
- traits
- statistics
- commodities
- recipes
- interactions
- how these systems connect

---

## Module 02 — Reading EA Tuning

[Open Module 02](modules/02-reading-ea-tuning/README.md)

This module teaches you how to investigate unfamiliar EA tuning without trying to understand every line at once.

You will learn how to:

- identify tuning classes and types
- read names, instance IDs, and modules
- recognize references
- follow tuning chains
- identify tests and loot
- read XML structure
- investigate only the parts relevant to your question
- build tuning maps

---

## Module 03 — Your First Tuning Mod

[Open Module 03](modules/03-first-tuning-mod/README.md)

This is the first hands-on module.

You will learn how to:

- choose a simple modding goal
- find the relevant EA tuning
- identify the field you need to change
- make a small tuning edit
- package the resource
- install and test the mod
- compare expected and actual results
- revise the mod methodically

---

## Module 04 — Interactions

[Open Module 04](modules/04-interactions/README.md)

This module explains how interaction tuning works and why interactions often connect to many other resources.

You will learn about:

- interactions
- super interactions
- mixer interactions
- affordances
- participants
- tests
- outcomes
- loot
- autonomy
- social interactions
- object interactions
- crafting interactions
- interaction debugging

---

## Module 05 — Buffs, Traits, Statistics & Commodities

[Open Module 05](modules/05-buffs-traits-statistics/README.md)

This module focuses on the systems used to describe and track Sim state.

You will learn about:

- visible and hidden buffs
- persistent traits
- buff and trait differences
- trait-provided buffs
- buff replacements
- statistics
- skills
- commodities
- motives
- decay
- tests
- loot
- autonomy
- how these systems work together

---

## Module 06 — Recipes & Crafting

[Open Module 06](modules/06-recipes-and-crafting/README.md)

This module explains how crafting systems are built from multiple connected resources.

You will learn about:

- recipe tuning
- recipe classes
- recipe categories
- base recipes
- crafting costs
- skill requirements
- crafting phases
- super affordances
- resume affordances
- final products
- object definitions
- tags
- object states
- quality
- masterpiece logic
- value
- loot
- autonomy

---

## Module 07 — Debugging & Maintenance

[Open Module 07](modules/07-debugging-and-maintenance/README.md)

This module teaches you how to debug problems methodically and keep mods maintainable after game updates.

You will learn about:

- expected vs actual behavior
- debugging one layer at a time
- clean testing
- mod conflicts
- shared overrides
- LastException files
- broken references
- invalid XML
- patch maintenance
- comparing old and new EA tuning
- tracking compatibility
- development notes
- version control
- patch-day workflows

---

# Recommended Learning Path

If you are completely new to Sims 4 modding, follow the modules in order:

```text
Module 00
Start Here
    |
    v
Module 01
Tuning Foundations
    |
    v
Module 02
Reading EA Tuning
    |
    v
Module 03
Your First Tuning Mod
    |
    v
Module 04
Interactions
    |
    v
Module 05
Buffs, Traits,
Statistics & Commodities
    |
    v
Module 06
Recipes & Crafting
    |
    v
Module 07
Debugging & Maintenance
```

The early modules focus on understanding what you are looking at.

The later modules focus more on using that knowledge to modify, investigate, and maintain gameplay systems.

---

# If You Already Know the Basics

You don't have to follow the course in order.

Use the module closest to your question:

| If you want to learn... | Start here |
|---|---|
| What tuning is | [Module 01](modules/01-tuning-foundations/README.md) |
| How to read extracted tuning | [Module 02](modules/02-reading-ea-tuning/README.md) |
| How to make a simple tuning mod | [Module 03](modules/03-first-tuning-mod/README.md) |
| How interactions work | [Module 04](modules/04-interactions/README.md) |
| How buffs, traits, statistics, and commodities work | [Module 05](modules/05-buffs-traits-statistics/README.md) |
| How recipes and crafting work | [Module 06](modules/06-recipes-and-crafting/README.md) |
| How to debug or update a mod | [Module 07](modules/07-debugging-and-maintenance/README.md) |

---

# Reference Library

The reference library isn't part of the course, it is a place to quickly find the definitions/info on a concept whenever you need it. 

Use the reference library when your question is:

> "I found this tuning. What is it?"

or:

> "How does this resource connect to everything else?"

Start here:

[Tuning Type Reference](reference/tuning-types/README.md)

The reference library will continue to grow over time as more tuning types and gameplay systems are documented.

---

# Course Complete?

If you have finished all eight modules, you now have a foundation for:

- reading Sims 4 tuning
- following references
- understanding tests and loot
- identifying Sim-state systems
- investigating interactions
- understanding crafting systems
- making small tuning edits
- debugging problems
- maintaining mods after patches

From here, the best next step is to build something small and use the reference library whenever you encounter an unfamiliar system.

You do not need to memorize everything.

The goal is to know how to find the answer.
