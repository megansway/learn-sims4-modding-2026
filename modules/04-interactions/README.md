# Module 04 — Interactions

Interactions are one of the most important systems to understand in Sims 4 modding because they often sit at the center of many other tuning resources.

An interaction can define what a Sim does, when they are allowed to do it, who or what participates, what tests must pass, what happens during the interaction, and what happens afterward.

A useful way to think about interactions is:

> **Interactions are the bridge between player or autonomous choices and actual gameplay behavior.**

---

## What You Will Learn

By the end of this module, you should understand:

- what interactions are
- the difference between super interactions and mixer interactions
- how interactions use tests
- how interactions use participants
- how interactions connect to loot
- how autonomy can affect interaction selection
- how outcomes can change what happens afterward
- why interactions often reference many other tuning resources
- how to trace an interaction from availability to result

---

# The Big Picture

An interaction is rarely just one isolated action.

A simplified interaction may look like:

```text
Interaction
    |
    +--> Tests
    |
    +--> Participants
    |
    +--> Animation / Behavior
    |
    +--> Outcomes
    |
    +--> Loot
    |
    +--> Autonomy
```

This is why interaction tuning can become large very quickly.

Instead of trying to understand every field at once, break the interaction into parts.

---

# What Is an Interaction?

An interaction represents an action that a Sim, object, or other game system can perform.

Examples include:

- talking to another Sim
- cooking
- painting
- sitting
- sleeping
- using a computer
- repairing an object
- crafting
- traveling
- performing social actions
- autonomous behavior

Some interactions are visible in the pie menu.

Others are used internally and may never be directly selectable by the player.

---

## In Plain English

An interaction answers questions like:

```text
What action is happening?

Who is doing it?

What is the target?

When is it allowed?

What happens during it?

What happens after it finishes?
```

---

# Interaction Tuning Is Highly Connected

A typical interaction may depend on several other resources.

For example:

```text
Interaction
    |
    +--> Test: Skill Level
    |
    +--> Test: Trait
    |
    +--> Buff
    |
    +--> Loot
    |
    +--> Animation
    |
    +--> Statistic
    |
    +--> Outcome
```

This makes interactions a very good place to practice reference tracing.

---

# Super Interactions

A **super interaction** is a primary interaction that can usually run as a complete action on its own.

Examples might include:

```text
Cook Meal
Use Computer
Take Shower
Paint
Talk to Sim
```

A super interaction often controls the main behavior or process.

Conceptually:

```text
Sim
 |
 v
Super Interaction
 |
 v
Complete Action
```

---

# Mixer Interactions

A **mixer interaction** is typically an interaction that runs within the context of another interaction.

Instead of acting as the entire activity, it mixes additional behavior into an existing interaction context.

A simplified example might be:

```text
Ongoing Social Interaction
        |
        v
Mixer Interaction
        |
        v
Specific Social Action
```

Conceptually:

```text
Super Interaction
      |
      +--> Mixer
      +--> Mixer
      +--> Mixer
```

This is why mixer interactions are commonly encountered while investigating social interactions and other systems that support multiple actions within a larger context.

> [!IMPORTANT]
> Super and mixer interactions are related, but they do not serve exactly the same role. When investigating interaction tuning, first identify which kind of interaction you are looking at.

---

# A Useful Mental Model

Think of it like this:

```text
Super Interaction
= the main activity

Mixer Interaction
= an action performed inside that activity
```

For example:

```text
Conversation
    |
    +--> Tell Joke
    +--> Compliment
    +--> Argue
```

The exact implementation can vary, but this distinction is useful when first learning the system.

---

# Interaction Availability

Before an interaction can run, the game may check whether it is currently valid.

That is where tests become important.

Conceptually:

```text
Player selects interaction
        |
        v
Run Tests
   /         \
Pass         Fail
 |            |
 v            v
Run          Reject / Hide / Disable
```

Tests may check things such as:

- age
- traits
- buffs
- relationship state
- skill level
- object state
- location
- time
- current situation
- participant conditions

---

# Interaction Tests

Suppose an interaction should only be available to Sims with a certain skill.

Conceptually:

```text
Interaction
    |
    v
Skill Test
    |
    v
Skill >= 5?
   /      \
 Yes      No
  |        |
  v        v
Allow     Block
```

The interaction itself may not contain the full definition of the skill.

It may reference a statistic or skill tuning.

---

# Participants

Interactions need to know who or what is involved.

Common conceptual participants include:

```text
Actor
Target
Object
Target Sim
Household
Other Participant
```

The **Actor** is usually the Sim performing the interaction.

The **Target** is usually the Sim or object the interaction is directed toward.

For example:

```text
Actor:
Sim A

Target:
Sim B
```

In a social interaction:

```text
Sim A
  |
  v
Compliment
  |
  v
Sim B
```

Participant definitions matter because tests and loot often target specific participants.

---

# Why Participants Matter

Imagine loot that adds a buff.

You still need to answer:

> Add the buff to whom?

Conceptually:

```text
Interaction
    |
    v
Loot
    |
    +--> Actor
    |
    +--> Target
```

If the wrong participant is selected, the effect may apply to the wrong Sim.

> [!TIP]
> When an interaction produces the correct effect on the wrong Sim or object, participant tuning is one of the first things to inspect.

---

# Interaction Outcomes

Some interactions can produce different results.

For example:

```text
Interaction
    |
    v
Outcome
   /    \
Success Failure
  |       |
  v       v
Loot A   Loot B
```

The result may depend on:

- tests
- skill
- relationship
- random chance
- mood
- traits
- statistics
- other gameplay state

Different outcomes may run different loot.

---

# Outcomes and Loot

A common conceptual structure is:

```text
Interaction
    |
    v
Outcome
    |
    +--> Success Loot
    |
    +--> Failure Loot
```

This means the interaction may control **when** an outcome occurs, while loot controls **what happens** because of that outcome.

For example:

```text
Successful Interaction
        |
        v
Loot
        |
        +--> Add Buff
        +--> Increase Relationship
```

---

# Interaction → Test → Loot

This is one of the most important chains to recognize.

```text
Interaction
    |
    v
Tests
    |
   Pass
    |
    v
Interaction Runs
    |
    v
Outcome
    |
    v
Loot
    |
    v
Gameplay Effect
```

If you can follow that chain, a large amount of interaction tuning becomes easier to understand.

---

# Autonomy

Not every interaction is chosen directly by the player.

The game can also select interactions autonomously.

Autonomy systems help determine which interactions Sims may choose on their own.

Conceptually:

```text
Possible Interactions
        |
        v
Autonomy Evaluation
        |
        +--> Tests
        +--> Scores
        +--> Weights
        +--> Sim State
        |
        v
Interaction Selected
```

The exact autonomy system is complex, but the important beginner concept is:

> An interaction can be available to the player without necessarily being attractive to autonomy, and autonomy can evaluate more than simple availability.

---

# Availability vs Autonomy

These are not the same thing.

```text
Available
= Can the interaction happen?

Autonomous
= Would the game choose it on its own?
```

An interaction can be:

```text
Player-available
but
not autonomously chosen
```

or:

```text
Available
and
highly attractive to autonomy
```

This distinction matters when modifying Sim behavior.

---

# Interaction Queues

When Sims perform interactions, those actions usually participate in the interaction queue.

Conceptually:

```text
Queue
 |
 +--> Interaction 1
 +--> Interaction 2
 +--> Interaction 3
```

Some interactions can be interrupted.

Some may block others.

Some may behave differently depending on what the Sim is already doing.

This is one reason an interaction can behave correctly in isolation but strangely when combined with other actions.

---

# Interaction Context

Interactions do not happen in a vacuum.

Their behavior may depend on context such as:

- who initiated them
- what object is targeted
- whether the interaction was autonomous
- what social context is active
- current routing
- active buffs or traits
- current situation
- previous interactions

This context can influence tests, outcomes, or available behavior.

---

# Social Interactions

Social interactions are a useful example of how connected interaction tuning can become.

A simplified social chain might be:

```text
Sim A
  |
  v
Social Interaction
  |
  +--> Target = Sim B
  |
  +--> Relationship Tests
  |
  +--> Trait Tests
  |
  +--> Outcome
          |
          +--> Relationship Loot
          +--> Buff Loot
```

This is why a seemingly simple conversation option can involve many resources.

---

# Object Interactions

Objects can provide interactions to Sims.

Conceptually:

```text
Object
  |
  +--> Interaction A
  +--> Interaction B
  +--> Interaction C
```

For example:

```text
Easel
 |
 +--> Paint
 +--> Resume Painting
 +--> Scrap Painting
```

If an interaction only appears on a particular object, part of the investigation may involve the object's tuning or affordance lists.

---

# Affordances

You will frequently encounter the word **affordance** when reading Sims 4 tuning.

In practical terms, an affordance is interaction-related tuning that represents something an actor can potentially do.

You may encounter terms such as:

```text
super_affordance
affordance
mixer
resume_affordance
```

A useful beginner mental model is:

```text
Affordance
= an available interaction/action definition
```

The exact role depends on the context.

---

# Super Affordances

Objects and systems may reference **super affordances**.

For example, a crafting recipe may reference a super affordance responsible for one stage of the crafting process.

Conceptually:

```text
Recipe
   |
   v
Super Affordance
   |
   v
Crafting Interaction
```

This is another example of recipes and interactions working together rather than existing separately.

---

# Resume Affordances

Crafting systems may use a special interaction for unfinished work.

Conceptually:

```text
Unfinished Object
       |
       v
Resume Affordance
       |
       v
Continue Crafting
```

This is why you may see interaction references inside recipe tuning that seem specifically related to resuming rather than beginning the action.

---

# Interaction Names

The interaction's tuning name can provide clues about:

- the object involved
- the action
- the social category
- whether it is a mixer
- whether it is a super interaction
- whether it is a staging interaction
- whether it belongs to a specific pack or system

For example, a name like:

```text
canvas_PaintPainting_Staging_Small
```

suggests:

```text
Canvas
Painting
Staging
Small
```

Use names to orient yourself, but verify behavior from the tuning.

---

# How Interactions Connect to Buffs

An interaction may cause a buff indirectly.

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

If you are investigating:

> "Why does using this interaction make the Sim Confident?"

do not assume the mood information lives in the interaction.

The interaction may only trigger loot.

The loot may add the buff.

The buff may define the mood.

---

# How Interactions Connect to Traits

Traits may influence interactions through tests.

For example:

```text
Interaction
    |
    v
Trait Test
    |
    v
Does Sim Have Trait?
```

Traits may also influence outcomes, autonomy, or participant behavior.

Conceptually:

```text
Trait
  |
  v
Changes Interaction Eligibility
```

or:

```text
Trait
  |
  v
Changes Interaction Outcome
```

---

# How Interactions Connect to Statistics

Interactions frequently read or modify statistics.

Examples include:

```text
Skill
Motive
Hidden Statistic
Relationship Value
Progress Tracker
```

Conceptually:

```text
Interaction
    |
    +--> Test Statistic
    |
    +--> Modify Statistic
```

The statistic itself is usually a separate resource.

---

# How Interactions Connect to Recipes

Crafting interactions and recipes often work together.

Conceptually:

```text
Interaction
    |
    v
Recipe
    |
    v
Final Product
```

or:

```text
Recipe
   |
   v
Crafting Phase Interaction
```

So if you are investigating a crafting interaction, you may need to inspect both the interaction and the recipe.

---

# A Practical Interaction Investigation

Suppose you find an interaction you want to modify.

Start by asking:

```text
What does this interaction do?
```

Then work through:

```text
1. What type of interaction is it?
2. What participants does it use?
3. What tests control availability?
4. Does it reference a recipe?
5. Does it have outcomes?
6. Does it run loot?
7. Does it modify statistics?
8. Does autonomy use it?
```

You do not need all eight answers for every mod.

Use the ones relevant to your goal.

---

# Example: Why Is This Interaction Unavailable?

Start with the interaction.

Look for:

```text
Tests
```

You might find:

```text
Trait Test
Skill Test
Buff Test
Object State Test
```

Then determine which one is failing.

Conceptually:

```text
Interaction
    |
    v
Tests
    |
    +--> Trait Pass
    +--> Skill Pass
    +--> Object State Fail
                         |
                         v
                  Interaction Blocked
```

---

# Example: Why Did This Interaction Give a Buff?

Follow the effect chain.

```text
Interaction
    |
    v
Outcome
    |
    v
Loot
    |
    v
Buff
```

Then inspect the buff to understand the actual mood or state.

---

# Example: Why Is the Wrong Sim Getting the Effect?

Check participants.

```text
Interaction
    |
    v
Loot
    |
    v
Participant = Target
```

If you expected:

```text
Actor
```

then the problem may be participant targeting rather than the loot itself.

---

# Example: Why Does a Sim Keep Doing This Autonomously?

Look at more than availability.

Investigate:

```text
Interaction
    |
    +--> Autonomy Settings
    +--> Tests
    +--> Weights
    +--> Buff Influence
    +--> Trait Influence
```

The interaction may be valid and also highly attractive to the autonomy system.

---

# Common Interaction Investigation Map

A useful note while modding might look like:

```text
Interaction:
Example_Interaction

Type:
Super Interaction

Actor:
Sim

Target:
Object

Tests:
Skill >= 3

Outcome:
Success

Loot:
Example_Loot

Effect:
Add Example_Buff
```

That is often enough to understand the relevant behavior without documenting the entire file.

---

# Common Beginner Mistakes

## Assuming the Interaction Contains Everything

It often does not.

Follow references.

---

## Confusing Availability With Outcome

Tests may control whether the interaction can run.

Loot may control what happens afterward.

These are separate questions.

---

## Ignoring Participants

Participant configuration can completely change who receives an effect.

---

## Ignoring Autonomy

An interaction behaving correctly when clicked does not automatically mean its autonomous behavior is correct.

---

## Editing a Shared Interaction

Multiple objects or systems may use the same interaction.

Check where it is referenced before assuming a change is isolated.

---

## Confusing Recipes With Interactions

The interaction is the action.

The recipe defines the crafted product and production rules.

They often work together.

---

## Assuming All Interactions Are Player-Facing

Some interactions are internal, staging-related, or otherwise not directly selectable.

---

# When to Edit the Interaction

The interaction is a likely place to edit when your goal involves:

- availability
- participant behavior
- interaction-specific tests
- autonomy behavior
- interaction outcomes
- target restrictions
- interaction flow
- what actions are available

---

# When Not to Edit the Interaction

The relevant change may live somewhere else.

If you want to change:

```text
Mood
```

look at the buff.

If you want to change:

```text
Post-interaction effect
```

look at loot.

If you want to change:

```text
Crafted product
```

look at the recipe or final product.

If you want to change:

```text
Persistent Sim characteristic
```

look at the trait.

If you want to change:

```text
Tracked numeric value
```

look at the statistic or commodity.

Knowing when **not** to edit the interaction is just as important as knowing when to edit it.

---

# A Good Interaction Debugging Order

When an interaction does not behave as expected, check:

```text
1. Is the interaction available?
2. Are the tests passing?
3. Are the participants correct?
4. Does the interaction start?
5. Does the expected outcome occur?
6. Does the expected loot run?
7. Does the correct participant receive the effect?
8. Is autonomy affecting the behavior?
```

This keeps you from jumping randomly between resources.

---

# If You Remember Only 5 Things

1. **Interactions define actions, but they often rely on other tuning for conditions and effects.**
2. **Super interactions are generally the main action; mixer interactions usually operate within another interaction context.**
3. **Tests control whether behavior is valid, while loot commonly controls what happens afterward.**
4. **Participants determine who or what the interaction and its effects apply to.**
5. **If an interaction does not contain the behavior you are looking for, follow its references.**

---

# Practice Before Moving On

Choose one EA interaction and answer:

```text
What kind of interaction is it?

Who is the actor?

What is the target?

What tests does it use?

Does it reference loot?

Does it reference a recipe?

What happens on success?

What happens on failure?

Could autonomy choose it?
```

You do not need to understand every field.

The goal is to identify the interaction's role and its major connections.

---

## Reference Library

For deeper explanations of individual interaction-related systems, use:

[The Tuning Type Reference](../../reference/tuning-types/README.md)

---

# Next Module

Continue to:

[Module 05 — Buffs, Traits, Statistics & Commodities](../05-buffs-traits-statistics/README.md)
