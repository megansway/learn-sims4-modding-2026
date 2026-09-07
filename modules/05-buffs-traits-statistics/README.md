# Module 05 — Buffs, Traits, Statistics & Commodities

Buffs, traits, statistics, and commodities are some of the most common systems used to describe what is happening to a Sim.

They often work together.

A trait may give a Sim a buff. A buff may affect a mood. A statistic may track progress. A commodity may rise or decay over time. Tests may check any of them, and loot may modify them.

A useful way to think about this module is:

```text
What is true about the Sim?
        |
        +--> Trait
        |
        +--> Buff
        |
        +--> Statistic
        |
        +--> Commodity
```

The important part is learning what role each one plays.

---

## What You Will Learn

By the end of this module, you should understand:

- what buffs represent
- what traits represent
- how buffs and traits differ
- what statistics are
- what commodities are
- how commodities relate to statistics
- how tests inspect these systems
- how loot modifies them
- how traits can provide or replace buffs
- how these systems influence interactions and autonomy
- how to decide which resource to inspect when debugging gameplay

---

# Buffs

A **buff** represents a gameplay state or effect currently applied to a Sim.

Buffs can be visible to the player, such as moodlets, or hidden and used internally by the game.

A useful mental model is:

```text
Something happens
        |
        v
Buff is added
        |
        v
Sim enters a state
        |
        +--> Mood may change
        +--> Tests may detect it
        +--> Other systems may react
```

---

## Buffs Can Be Visible

A visible buff may have:

- a name
- a description
- an icon
- a mood type
- a mood weight
- a duration

For example:

```text
Buff
 |
 +--> Name
 +--> Description
 +--> Icon
 +--> Mood
 +--> Duration
```

These are the kinds of buffs players usually recognize as moodlets.

---

## Buffs Can Be Hidden

Not every buff appears in the UI.

A hidden buff may exist only to tell the game:

```text
This Sim is currently in State X.
```

Other systems can then test for that state.

Conceptually:

```text
Hidden Buff
    |
    v
Trait / Interaction / Test detects it
    |
    v
Behavior changes
```

So when debugging gameplay, do not assume that every important buff is visible.

---

# Traits

A **trait** represents a more persistent characteristic or gameplay marker.

Traits may represent:

- personality
- rewards
- hidden states
- roles
- progression
- special gameplay identity
- system-specific markers

A useful mental model is:

```text
Sim
 |
 +--> Trait
       |
       +--> Buffs
       +--> Tests
       +--> Conflicts
       +--> Special behavior
```

Traits usually describe something that is more consistently true about the Sim than a temporary buff.

---

# Buffs vs Traits

This is one of the most useful distinctions to learn.

| Buff | Trait |
|---|---|
| Represents a current state or effect | Represents a more persistent characteristic or marker |
| Often temporary | Often long-term |
| Frequently added and removed | Usually changes less often |
| Can affect mood | Can influence many systems |
| Can be hidden | Can also be hidden |
| Can be provided by traits | Can provide or replace buffs |

A simple way to remember it is:

```text
Trait:
"What is generally true about this Sim?"

Buff:
"What is affecting this Sim right now?"
```

There are exceptions, especially with hidden system traits and buffs, but this is a useful beginner model.

---

# Traits Can Give Buffs

Traits often connect directly to buffs.

Conceptually:

```text
Trait
   |
   v
Buff
   |
   v
Gameplay State
```

For example, a personality trait may provide a hidden trait-related buff that helps drive ongoing behavior.

This means the trait itself may not contain every visible effect.

Sometimes the trait simply connects the Sim to other resources that handle those effects.

---

# Traits Can Replace Buffs

Traits can also alter reactions by replacing one buff with another.

Conceptually:

```text
Game wants to apply normal buff
            |
            v
Trait detected
            |
            v
Use trait-specific replacement buff
```

This is a powerful pattern because the original event does not need to change.

The trait changes the resulting reaction.

---

# Trait Conflicts

Traits can also define incompatibilities.

Conceptually:

```text
Trait A
   |
   +--> Conflicts with Trait B
```

This allows the game to prevent certain combinations in contexts where that conflict is enforced.

When creating or editing traits, conflicts can be just as important as the trait's positive behavior.

---

# Statistics

A **statistic** is a numeric value tracked by the game.

Statistics can represent things like:

- skills
- motives
- hidden counters
- progress
- relationship values
- scores
- internal gameplay values

A simple mental model is:

```text
Statistic
   |
   v
Current Value
```

For example:

```text
Painting Skill = 6
```

or:

```text
Hidden Progress = 42
```

Other systems can read or modify that value.

---

# How Statistics Are Used

A statistic can be:

```text
Read
Tested
Increased
Decreased
Reset
Compared to a threshold
```

Conceptually:

```text
Statistic
   |
   +--> Test Value
   |
   +--> Change Value
   |
   +--> Trigger Behavior
```

This makes statistics extremely useful for progression and state tracking.

---

# Skills Are Statistics

Skills are commonly implemented through the game's statistic systems.

Conceptually:

```text
Statistic
   |
   v
Skill
   |
   v
Painting Skill
```

This is why a recipe or interaction may reference what looks like a statistic tuning when checking skill level.

For example:

```text
Recipe
   |
   v
Skill Test
   |
   v
Painting Skill >= 4
```

---

# Commodities

A **commodity** is a type of statistic commonly used for values that change over time or drive ongoing simulation behavior.

A useful beginner model is:

```text
Statistic
    |
    +--> General tracked value

Commodity
    |
    +--> Dynamic tracked value
         that may increase, decrease,
         decay, or influence behavior
```

Commodities are often used for things like:

- motives
- hidden needs
- autonomy-driving values
- decay-based systems
- temporary internal progression

---

# Statistics vs Commodities

A commodity is related to statistics, but the terms are not always interchangeable.

A simplified comparison:

| Statistic | Commodity |
|---|---|
| General numeric value tracked by the game | A statistic commonly used for dynamic or time-sensitive values |
| May stay stable until changed | Often changes or decays over time |
| Can represent skills and counters | Often represents motives or hidden drivers |
| Can be tested and modified | Can be tested and modified |

A useful mental model is:

```text
Commodity
    is a specialized kind of
Statistic
```

The exact technical distinction can vary depending on the system, so inspect the resource class when deeper accuracy matters.

---

# Motives and Commodities

Motives are a familiar example of values that behave like commodities.

Conceptually:

```text
Hunger
   |
   v
Commodity Value
   |
   +--> Decays
   |
   +--> Tested by autonomy
   |
   +--> Changed by interactions
```

For example:

```text
Hunger low
   |
   v
Autonomy evaluates food interactions
```

This is how a numeric value can influence behavior.

---

# How Tests Use These Systems

Tests can inspect buffs, traits, statistics, and commodities.

For example:

```text
Does Sim have Trait X?
```

```text
Does Sim have Buff Y?
```

```text
Is Painting Skill >= 5?
```

```text
Is Commodity below threshold?
```

Conceptually:

```text
Game State
   |
   v
Test
   |
   +--> Trait?
   +--> Buff?
   +--> Statistic?
   +--> Commodity?
```

This is one reason these systems matter so much.

They provide the state that tests evaluate.

---

# How Loot Uses These Systems

Loot often modifies these same systems.

Conceptually:

```text
Loot
 |
 +--> Add Buff
 +--> Remove Buff
 +--> Add Trait
 +--> Remove Trait
 +--> Increase Statistic
 +--> Decrease Commodity
```

This gives us a very common gameplay pattern:

```text
Test
 |
 v
Pass
 |
 v
Loot
 |
 v
Change Sim State
```

---

# A Full Example

Suppose an interaction rewards a Sim for succeeding at something.

Conceptually:

```text
Interaction
    |
    v
Skill Test
    |
   Pass
    |
    v
Loot
    |
    +--> Add Confident Buff
    |
    +--> Increase Hidden Statistic
```

Now multiple systems are connected:

```text
Interaction
Test
Skill Statistic
Loot
Buff
Hidden Statistic
```

This is typical Sims 4 tuning.

---

# Buffs and Mood

A buff may contribute to a mood.

Conceptually:

```text
Buff
   |
   +--> Mood Type
   |
   +--> Mood Weight
```

For example:

```text
Confident Buff
      |
      v
Confident Mood
```

Multiple buffs can influence the Sim's current emotional state.

---

# Traits and Mood

Traits may influence mood indirectly through buffs.

Conceptually:

```text
Trait
   |
   v
Trait-Specific Buff
   |
   v
Mood
```

This is why changing a trait does not always mean changing the trait resource itself.

The visible effect may live in a connected buff.

---

# Statistics and Progression

Statistics are often used to track progress toward something.

For example:

```text
Statistic = 0
     |
     v
Interaction runs
     |
     v
+10
     |
     v
Statistic = 10
```

Tests can then inspect thresholds:

```text
Statistic >= 50?
```

This pattern appears in many progression systems.

---

# Commodities and Decay

A commodity may change automatically over time.

Conceptually:

```text
Commodity = 100
      |
      v
Time passes
      |
      v
Commodity = 80
      |
      v
Commodity = 60
```

Other tuning can react as the value changes.

This makes commodities useful for ongoing simulation systems.

---

# How These Systems Affect Autonomy

Autonomy may consider:

- buffs
- traits
- motives
- commodities
- statistics
- interaction scores
- current context

Conceptually:

```text
Sim State
   |
   +--> Trait
   +--> Buff
   +--> Commodity
   |
   v
Autonomy Evaluation
   |
   v
Choose Interaction
```

This means changing a buff or commodity can sometimes affect what Sims choose to do even if you never edit the interaction itself.

---

# How to Decide Which Resource to Inspect

When debugging, start with the question.

## "Why does the Sim have this moodlet?"

Look at:

```text
Buff
```

Then determine what added it.

---

## "Why does this Sim behave differently all the time?"

Look at:

```text
Trait
```

and any connected buffs or autonomy effects.

---

## "Why is this interaction locked until level 5?"

Look at:

```text
Statistic / Skill Test
```

---

## "Why does this value keep decreasing?"

Look at:

```text
Commodity
```

and its decay behavior.

---

## "Why did this state change after an interaction?"

Look at:

```text
Loot
```

then determine which buff, trait, statistic, or commodity it changed.

---

# A Useful Connection Map

These four systems often connect like this:

```text
Trait
   |
   +--> Buff
   |
   v
Sim State
   |
   +--> Statistic
   |
   +--> Commodity
   |
   v
Tests
   |
   v
Interaction
   |
   v
Loot
   |
   +--> Buff
   +--> Trait
   +--> Statistic
   +--> Commodity
```

This is not the only possible structure, but it captures a very common pattern.

---

# Common Beginner Mistakes

## Assuming Buff Means Moodlet

Buffs can be hidden.

---

## Assuming Traits Contain All Their Behavior

Traits often reference buffs or other resources.

---

## Treating Every Numeric Value as the Same Kind of Statistic

Different statistic classes may serve different roles.

---

## Assuming Commodity Means Motive

Motives are one common use, but commodities are broader than visible needs.

---

## Editing the Buff When the Trait Is the Real Cause

Sometimes the buff is only a symptom of trait behavior.

---

## Editing the Trait When the Buff Controls the Visible Effect

The opposite can also happen.

Follow the chain.

---

## Ignoring Tests

Even if the trait, buff, or statistic exists, another system may only react when a test passes.

---

# A Good Debugging Order

When working with Sim state, ask:

```text
1. What state is visible in-game?
2. Is it represented by a buff, trait, statistic, or commodity?
3. What added or changed it?
4. Is loot involved?
5. Are tests checking it?
6. Does it influence autonomy or interactions?
7. Is another resource replacing or modifying it?
```

That sequence usually gives you a much clearer picture.

---

# If You Remember Only 5 Things

1. **Buffs usually represent current states or effects.**
2. **Traits usually represent more persistent characteristics or markers.**
3. **Statistics are numeric values tracked by the game.**
4. **Commodities are statistics commonly used for dynamic, changing, or decay-based values.**
5. **Tests read these systems; loot often changes them.**

---

# Practice Before Moving On

Choose one Sim-related gameplay effect and trace it.

For example:

```text
Trait
   |
   v
Buff
   |
   v
Mood
```

or:

```text
Interaction
   |
   v
Loot
   |
   v
Statistic
```

Answer:

1. What resource represents the state?
2. What caused it?
3. Is it temporary or persistent?
4. Can tests detect it?
5. Can loot modify it?
6. Does it influence another system?

The goal is to practice identifying the role of each resource rather than memorizing individual XML fields.

---

## Reference Library

For deeper explanations, use:

[The Tuning Type Reference](../../reference/tuning-types/README.md)

---

# Next Module

Continue to:

[Module 06 — Recipes & Crafting](../06-recipes-and-crafting/README.md)
