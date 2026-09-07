# Module 02 — Reading EA Tuning

Reading EA tuning can feel overwhelming at first because a single gameplay feature may be spread across several different resources.

The goal of this module is not to teach you every XML tag or tuning class. The goal is to teach you how to **investigate** tuning.

By the end of this module, you should be able to open an unfamiliar tuning file, identify what kind of resource it is, find the important references, follow those references, and build a reasonable picture of how the gameplay behavior works.

---

## What You Will Learn

In this module, you will learn how to:

- identify the type of tuning you are looking at
- read the top-level tuning information
- recognize names, instance IDs, and references
- distinguish configuration from referenced behavior
- follow tuning chains
- identify tests and loot
- determine what a resource references
- determine what references a resource
- avoid wasting time on unrelated fields
- build a mental map of a gameplay system

---

# The Most Important Rule

When reading Sims 4 tuning, do not assume that the file you opened contains the entire behavior.

A resource may only define one part of a larger system.

For example:

```text
Interaction
    |
    v
Test
    |
    v
Loot
    |
    v
Buff
```

If you open only the interaction, you may never see the final buff directly.

That does not mean the interaction is unrelated.

It means the behavior is spread across multiple resources.

---

# Start With the Top Line

When you open extracted tuning, the first line often tells you a lot.

For example:

```xml
<I c="Buff"
   i="buff"
   m="buffs.buff"
   n="Buff_Example"
   s="123456789">
```

The exact values will vary, but the structure is useful.

---

## `c` — Class

The `c` value identifies the tuning class.

For example:

```xml
c="Buff"
```

or:

```xml
c="Trait"
```

or:

```xml
c="LootActions"
```

This is one of the fastest ways to understand what kind of resource you are looking at.

Ask:

> What class is this?

That gives you your first clue about the resource's purpose.

---

## `i` — Tuning Type

The `i` value identifies the tuning type.

For example:

```xml
i="buff"
```

or:

```xml
i="trait"
```

or:

```xml
i="recipe"
```

This may not always use the exact same terminology you would use casually when talking about the resource.

For example, loot tuning can use:

```xml
i="action"
```

while still being a `LootActions` class.

So read both the class and tuning type together.

---

## `m` — Module

The `m` value identifies the Python module associated with the tuning class.

For example:

```xml
m="buffs.buff"
```

or:

```xml
m="traits.traits"
```

You do not need to understand the Python module immediately.

For now, treat it as useful context.

It can help answer:

> What game system does this tuning belong to?

---

## `n` — Name

The `n` value is the tuning name.

For example:

```xml
n="trait_Ambitious"
```

or:

```xml
n="recipe_Painting_Abstract_Small"
```

Names are extremely useful when searching extracted tuning.

They often contain clues about:

- the system
- the interaction
- the object
- the trait
- the pack
- the situation
- the intended effect

Do not treat names as perfect documentation, but do use them as clues.

---

## `s` — Instance ID

The `s` value is the instance ID.

For example:

```xml
s="16823"
```

This ID is extremely important because other tuning resources can reference it.

Conceptually:

```text
Resource A
   |
   v
16823
   |
   v
trait_Ambitious
```

When you are following references, instance IDs are often what connect one resource to another.

---

# Your First Pass Through a File

When opening unfamiliar tuning, do **not** immediately read every line from top to bottom.

Instead, do a quick first pass.

Look for:

1. the class
2. the tuning type
3. the tuning name
4. the instance ID
5. obvious references
6. tests
7. loot
8. major lists or sections

The goal of the first pass is:

> "What am I looking at?"

not:

> "Can I explain every field?"

---

# Names Are Clues, Not Proof

Tuning names are often descriptive.

For example:

```text
loot_Phone_Game
```

strongly suggests loot related to phone gameplay.

Likewise:

```text
trait_Ambitious
```

clearly points toward the Ambitious trait.

But names should be treated as clues.

The actual XML tells you what the resource does.

A useful rule is:

> Use the name to guess. Use the tuning to verify.

---

# Learn to Recognize References

One of the most important skills in Sims 4 modding is recognizing when a number refers to another resource.

For example:

```xml
<T n="buff_type">12650</T>
```

may point to another buff tuning.

Or:

```xml
<T n="skill">16708</T>
```

may point to a skill/statistic tuning.

Or:

```xml
<T n="super_affordance">38889</T>
```

may point to an interaction.

The extracted tuning may include a comment that helps:

```xml
<T n="skill">
16708
<!-- Skill: statistic_Skill_AdultMajor_Painting -->
</T>
```

That comment is extremely helpful because it tells you what the ID refers to.

---

# Comments Are Your Friend

Extracted EA tuning often includes comments like:

```xml
<!-- Buff: Buff_Trait_Ambitious -->
```

or:

```xml
<!-- Trait: trait_Lazy -->
```

or:

```xml
<!-- Mood: Mood_Confident -->
```

These comments are not the actual reference themselves.

The number is the reference.

But the comment tells you what the reference resolves to.

Conceptually:

```text
12650
  |
  v
Buff_Trait_Ambitious
```

Use those comments whenever they are available.

They save a huge amount of time.

---

# Follow References With a Purpose

You do not need to follow every reference.

That is one of the easiest ways to get overwhelmed.

Instead, ask:

> What behavior am I actually trying to understand?

If you are investigating:

> "Why does this interaction give a buff?"

follow the references related to:

- outcomes
- loot
- buffs

You probably do not need to investigate every animation, audio sting, icon, or unrelated test.

---

## Example

Imagine you open:

```text
Interaction
```

and find:

```text
Loot ID: 123456
```

You open the loot and find:

```text
Buff ID: 987654
```

You open the buff and find:

```text
Mood Type: Confident
```

Now you have:

```text
Interaction
    |
    v
Loot
    |
    v
Buff
    |
    v
Confident Mood
```

That is enough to answer:

> "Why does this interaction make the Sim Confident?"

You do not need to understand every field in all three files.

---

# Read Outward and Backward

When investigating a tuning resource, there are two directions you can look.

## Outward

Ask:

> What does this resource reference?

For example:

```text
Recipe
   |
   +--> Skill
   +--> Loot
   +--> Interaction
   +--> Final Product
```

This tells you what the resource depends on.

---

## Backward

Ask:

> What references this resource?

For example:

```text
Interaction A ----\
Interaction B ----- > Shared Loot
Interaction C ----/
```

This tells you where the resource is used.

Both directions matter.

A resource may make perfect sense internally but still affect more systems than you expect because it is shared.

---

# Why Backward References Matter

Suppose you find a loot tuning and change it because one interaction uses it.

If five other interactions also reference that same loot, your change may affect all six interactions.

So before modifying shared tuning, ask:

> What else uses this?

This is especially important for:

- snippets
- loot
- tests
- buffs
- traits
- statistics
- shared interactions
- filters

---

# Look for Tests

Tests usually answer:

> Should this happen?

When you see something like:

```xml
<V t="trait">
```

or:

```xml
<V t="skill_test">
```

or:

```xml
<V t="buff">
```

you may be looking at a condition.

A conceptual example:

```text
Does Sim have Trait X?
        |
      Pass
        |
        v
Continue
```

When behavior is unavailable, hidden, rejected, or conditional, tests are often the first place to investigate.

---

# Look for Loot

Loot usually answers:

> What happens?

A resource may contain or reference loot that:

- adds buffs
- changes statistics
- modifies relationships
- applies object states
- adds traits
- removes traits
- triggers more loot
- changes other gameplay state

A conceptual pattern:

```text
Interaction
    |
    v
Loot
    |
    v
Effect
```

If you are trying to figure out why something changes after an event, loot is often important.

---

# Separate Conditions From Effects

This distinction will save you a lot of confusion.

```text
Test
 = Should this happen?

Loot
 = What happens?
```

Example:

```text
Painting Skill >= 4?
        |
       Yes
        |
        v
Allow Recipe
```

That is a test.

Then:

```text
Crafting Completes
        |
        v
Run Loot
        |
        v
Modify Finished Object
```

That is an effect.

---

# Learn the Common XML Containers

You do not need to memorize XML, but recognizing a few common structures helps.

---

## `<T>`

A `T` usually contains a single tunable value.

Example:

```xml
<T n="mood_weight">1</T>
```

Think:

```text
One value
```

---

## `<L>`

An `L` represents a list.

Example:

```xml
<L n="buffs">
```

Think:

```text
Multiple entries
```

---

## `<U>`

A `U` is commonly a structured group of tunables.

Example:

```xml
<U n="quality_adjustment">
```

Think:

```text
A group of related settings
```

---

## `<V>`

A `V` commonly represents a variant or choice between possible structures.

Example:

```xml
<V t="state_change">
```

Think:

```text
This specific option/variant is being used
```

The exact meaning depends on the tuning.

---

## `<E>`

An `E` commonly represents an enum value.

Example:

```xml
<E>ADULT</E>
```

or:

```xml
<E>PERSONALITY</E>
```

Think:

```text
One named option from a predefined set
```

---

# Do Not Over-Memorize XML Letters

Knowing what `T`, `L`, `U`, `V`, and `E` generally mean is useful.

But the `n="..."` and `t="..."` values usually tell you much more.

For example:

```xml
<L n="conflicting_traits">
```

is more informative than simply knowing that `L` means list.

Likewise:

```xml
<V t="skill_test">
```

is more useful than memorizing that `V` represents a variant.

Focus on meaning, not just syntax.

---

# Read Sections as Concepts

Large tuning files often become easier when you stop seeing them as hundreds of XML lines and start seeing them as sections.

For example, a recipe might contain:

```text
Recipe
 |
 +--> Skill Test
 |
 +--> Crafting Cost
 |
 +--> Crafting Phases
 |
 +--> Final Product
 |
 +--> Quality
 |
 +--> Loot
```

Now the file feels less like:

> "A giant wall of XML"

and more like:

> "Several understandable systems grouped together."

---

# Use Indentation

Indentation helps show which tunables belong inside which section.

For example:

```xml
<U n="final_product">
    <L n="apply_tags">
        ...
    </L>
    <L n="loot_list">
        ...
    </L>
</U>
```

Conceptually:

```text
Final Product
    |
    +--> Tags
    |
    +--> Loot
```

If you get lost, follow the indentation.

---

# A Practical Investigation Workflow

When investigating EA tuning, use this process.

## Step 1 — Define the Question

Do not start with:

> "I want to understand this entire file."

Start with something specific:

> "Why does this interaction give this buff?"

or:

> "What controls the price of this painting?"

or:

> "Why is this option unavailable?"

A specific question gives you a direction.

---

## Step 2 — Find the Starting Resource

Identify the tuning closest to the behavior you are investigating.

This might be:

- an interaction
- a recipe
- a buff
- a trait
- an object
- a situation
- another resource

---

## Step 3 — Identify the Resource

Read:

```text
Class
Type
Name
Instance ID
```

Write them down if necessary.

---

## Step 4 — Find Relevant Sections

Ignore unrelated information.

If your question is about eligibility, look for:

```text
tests
requirements
skill tests
traits
buff tests
```

If your question is about an outcome, look for:

```text
loot
outcomes
buffs
states
statistics
```

---

## Step 5 — Follow References

When you find an important ID, locate that resource.

Repeat as needed.

---

## Step 6 — Draw the Chain

Even a tiny text diagram helps.

For example:

```text
Interaction
   |
   v
Loot
   |
   v
Buff
```

or:

```text
Recipe
   |
   +--> Skill Test
   |
   +--> Final Product
           |
           v
          Loot
```

This turns a pile of XML into a system you can understand.

---

## Step 7 — Stop When You Have the Answer

This is important.

You do not need to understand every connected resource.

If your original question has been answered, stop.

You can always investigate more later.

---

# Example Investigation: Why Is a Recipe Locked?

Suppose a recipe is unavailable.

Start with:

```text
Recipe
```

Look for tests.

You find:

```text
Skill Test
    |
    v
Painting Skill >= 4
```

Now you have a likely explanation.

You do not need to inspect the recipe's loot, value curve, masterpiece logic, or final-product tags just to answer the availability question.

---

# Example Investigation: Why Did My Sim Get a Buff?

Start with the interaction or event.

You find:

```text
Loot Reference
```

Follow it:

```text
Loot
   |
   v
Buff Reference
```

Follow again:

```text
Buff
   |
   v
Mood / Gameplay State
```

Now the chain is:

```text
Interaction
    |
    v
Loot
    |
    v
Buff
```

That is the behavior you were looking for.

---

# Example Investigation: Why Does a Trait Change a Reaction?

Start with the trait.

You may find:

```text
Trait
   |
   v
Buff Replacements
```

Then:

```text
Normal Buff
    |
    v
Trait-Specific Buff
```

Now the behavior makes sense.

The event itself did not necessarily change.

The trait changed the resulting buff.

---

# When a File Seems Unrelated

Sometimes you follow a reference and arrive at something that seems completely unrelated.

Before assuming you made a mistake, ask:

1. Is this resource reusable?
2. Does it contain another reference?
3. Is it acting as a middle layer?
4. Is the behavior split across multiple systems?

For example:

```text
Interaction
    |
    v
Snippet
    |
    v
Loot
    |
    v
Buff
```

The snippet may look like a detour, but it is part of the chain.

---

# Common Beginner Traps

## Trying to Understand Everything

You do not need to.

Follow the parts relevant to your question.

---

## Assuming the First File Contains the Answer

Often it does not.

Follow references.

---

## Ignoring IDs

IDs are how many resources connect.

Treat them as important.

---

## Ignoring Comments

Extracted tuning comments can save you a lot of time by identifying referenced resources.

---

## Editing Shared Tuning Without Checking Usage

A resource may be used by several systems.

Check what references it.

---

## Assuming Similar Names Mean the Same Thing

Names are helpful clues, but always inspect the actual tuning.

---

## Reading Every Line With Equal Importance

Not every field matters to your current question.

Learn to prioritize.

---

# Build a Tuning Map

When working on a mod, it can help to keep a small note like:

```text
Feature:
Small Abstract Painting

Recipe:
recipe_Painting_Abstract_Small

Skill:
statistic_Skill_AdultMajor_Painting

Crafting Interaction:
canvas_PaintPainting_Staging_Small

Final Product:
Definition 15922

Loot:
loot_Crafting_Recipe_QualityModifiers
```

This becomes your map of the system.

For larger mods, these maps can save a huge amount of time when you return to the project later.

---

# A Good Question Checklist

When reading unfamiliar tuning, ask:

- What type of resource is this?
- What does its name suggest?
- What is its instance ID?
- What are the major sections?
- Are there tests?
- Is there loot?
- What IDs does it reference?
- What do those IDs point to?
- What references this resource?
- Which connections matter to my question?
- Where does the final gameplay effect happen?

If you can answer those, you usually understand enough to keep moving.

---

# If You Remember Only 5 Things

1. **Do not expect one tuning file to contain an entire gameplay system.**
2. **Use class, type, name, and instance ID to identify what you are looking at.**
3. **Follow references only when they matter to the question you are trying to answer.**
4. **Tests usually explain conditions; loot usually explains effects.**
5. **Draw the resource chain. Understanding the connections matters more than memorizing every XML field.**

---

# Practice Before Moving On

Choose an EA tuning resource and answer these questions without trying to understand the entire file:

1. What is the tuning class?
2. What is the tuning type?
3. What is its name?
4. What is its instance ID?
5. Name three other resources or systems it references.
6. Does it contain tests?
7. Does it contain or reference loot?
8. What behavior do you think this resource is mainly responsible for?

If you can do that, you are already reading tuning much more effectively.

---

## Reference Library

If you encounter a tuning type you do not recognize while practicing, use:

[The Tuning Type Reference](../../reference/tuning-types/README.md)

The reference library explains individual tuning types in more depth.

---

# Next Module

Continue to:

[Module 03 — Your First Tuning Mod](../03-first-tuning-mod/README.md)
