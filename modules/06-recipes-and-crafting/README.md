# Module 06 — Recipes & Crafting

Crafting in The Sims 4 is usually built from several connected systems rather than one single tuning resource.

A recipe may define what is being made, but interactions, skills, tests, object definitions, loot, quality systems, costs, and other tuning can all participate in the final result.

A useful mental model is:

```text
Sim starts crafting
        |
        v
Interaction
        |
        v
Recipe
        |
        +--> Requirements
        +--> Cost
        +--> Crafting Phases
        +--> Skill
        +--> Final Product
        +--> Quality
        +--> Loot
```

The goal of this module is to help you recognize those connections and understand where to look when changing a crafting system.

---

## What You Will Learn

By the end of this module, you should understand:

- what recipe tuning represents
- why recipes are not limited to food
- how recipes connect to crafting interactions
- how crafting phases work conceptually
- how skill requirements and tests affect recipes
- how crafting costs are configured
- how recipes define or reference final products
- how quality and value can be influenced
- how loot can affect crafted results
- how resume interactions fit into crafting systems
- how to investigate a crafting mod without changing unrelated resources

---

# Recipes Are Not Just Food

The word **recipe** can be misleading if you think only about cooking.

In The Sims 4, recipe tuning can be used for many kinds of production or crafting behavior.

Depending on the system, recipes may be involved with things like:

- cooking
- baking
- drinks
- painting
- woodworking
- fabrication
- flower arranging
- other craftable or producible objects

So a better definition is:

> **A recipe defines something the game can produce and the rules around producing it.**

---

# Recipe vs Interaction

This distinction is extremely important.

```text
Interaction
= what the Sim does

Recipe
= what is being made and the rules for making it
```

A crafting interaction may control the Sim's behavior, animation, timing, routing, or action flow.

The recipe may control:

- the product
- cost
- requirements
- skill
- quality
- value
- crafting phases
- result data

They work together, but they are not the same thing.

---

# A Simple Crafting Chain

A basic crafting system might look like:

```text
Player selects action
        |
        v
Crafting Interaction
        |
        v
Recipe
        |
        v
Final Product
```

A more realistic system may look like:

```text
Player / Autonomy
        |
        v
Interaction
        |
        v
Recipe
        |
        +--> Tests
        +--> Skill
        +--> Cost
        +--> Phases
        |
        v
Final Product
        |
        +--> Quality
        +--> Tags
        +--> States
        +--> Loot
```

This is why changing one craftable item can sometimes require investigating several resources.

---

# Recipe Classes

Not every recipe tuning has the exact same structure.

Different crafting systems may use different recipe classes.

Conceptually:

```text
recipe
 |
 +--> Cooking Recipe Class
 |
 +--> Painting Recipe Class
 |
 +--> Crafting Recipe Class
 |
 +--> Other Specialized Recipe Classes
```

The top-level class tells you which kind of recipe you are dealing with.

> [!IMPORTANT]
> Do not assume that a field found in one recipe class exists in every other recipe class.

---

# Recipe Categories

Recipes may belong to categories that help organize them in menus or crafting systems.

Conceptually:

```text
Crafting Menu
     |
     +--> Category A
     |      |
     |      +--> Recipe
     |      +--> Recipe
     |
     +--> Category B
            |
            +--> Recipe
```

For example, a painting system might separate recipes by style.

A cooking system might organize recipes differently.

Categories help the game decide where and how a recipe appears.

---

# Base Recipes

Some recipes may reference a more general base recipe.

Conceptually:

```text
Base Recipe
    |
    +--> Variant A
    +--> Variant B
    +--> Variant C
```

A specific recipe can build on shared configuration rather than redefining everything.

This is another example of Sims 4 tuning being highly reference-based.

---

# Recipe Availability

Before a recipe can be used, the game may check requirements.

These can include:

- skill level
- age
- traits
- buffs
- object state
- pack or system conditions
- additional tests
- other eligibility rules

Conceptually:

```text
Recipe Selected
      |
      v
Run Tests
   /       \
 Pass      Fail
  |          |
  v          v
Allow       Block
```

---

# Skill Requirements

Recipes often use skills.

A simplified example:

```text
Recipe
   |
   v
Skill Test
   |
   v
Painting Skill >= 4
```

The skill itself is usually a separate statistic resource.

So the relationship may be:

```text
Recipe
   |
   v
Skill Test
   |
   v
Skill Statistic
```

This is why understanding statistics helps when reading recipe tuning.

---

# Additional Tests

Recipes may contain more than one requirement.

Conceptually:

```text
Recipe
   |
   +--> Skill Test
   |
   +--> Trait Test
   |
   +--> Sim Info Test
   |
   +--> Object Test
```

Not every recipe uses all of these.

The important thing is to look for test sections when a recipe is unavailable or behaves conditionally.

---

# Crafting Costs

A recipe can define the cost of making something.

For example:

```text
Recipe
   |
   v
Crafting Cost
   |
   v
50 Simoleons
```

This is separate from the value of the final product.

That distinction matters.

```text
Crafting Cost
    !=
Finished Object Value
```

A Sim may spend one amount to create an object that later sells for a different amount.

---

# Crafting Phases

Some recipes use multiple phases.

Conceptually:

```text
START_PHASE
      |
      v
Create / Prepare Object
      |
      v
Crafting Phase
      |
      v
Final Product
```

A phase may reference a super interaction that controls what the Sim does during that stage.

This is especially useful for crafting processes that are not instantaneous.

---

# Why Phases Matter

A crafting interaction may not be one single action.

Instead:

```text
Start
  |
  v
Create Initial Object
  |
  v
Work on Object
  |
  v
Finish
```

If a craftable object is not progressing correctly, the problem may be in:

- the recipe
- the phase
- the phase interaction
- the object
- the resume behavior

You may need to trace more than one resource.

---

# Super Affordances in Crafting

Recipes may reference super affordances for crafting phases.

Conceptually:

```text
Recipe
   |
   v
Crafting Phase
   |
   v
Super Affordance
   |
   v
Sim Performs Action
```

This is one of the clearest examples of recipes and interactions working together.

---

# Resume Affordances

Crafting systems often need a way to resume unfinished work.

Conceptually:

```text
Unfinished Crafted Object
          |
          v
Resume Affordance
          |
          v
Continue Crafting
```

The resume interaction may be separate from the interaction used to begin the craft.

This is useful when investigating:

> "Why can the Sim start this craft but not resume it?"

---

# Final Products

Recipes usually need to define or reference what gets created.

Conceptually:

```text
Recipe
   |
   v
Final Product
   |
   v
Object Definition
```

The final-product configuration may also include:

- states
- tags
- quality
- value
- loot
- other result behavior

---

# Object Definitions

The final product often references an object definition.

This is the resource that tells the game what actual object is created.

So:

```text
Recipe
   |
   v
Final Product
   |
   v
Definition
   |
   v
Created Object
```

If the wrong object appears after crafting, the final-product definition is an important place to inspect.

---

# Tags on Crafted Objects

Recipes may apply tags to the resulting object.

Conceptually:

```text
Crafted Object
     |
     +--> Crafted Object Tag
     +--> Category Tag
     +--> Genre Tag
```

Other game systems can use these tags to:

- categorize the object
- filter it
- recognize it
- allow it in specific systems
- apply special behavior

---

# Object States

A recipe may apply object states to the finished product.

Conceptually:

```text
Final Product
      |
      +--> Quality State
      +--> Special State
      +--> Conditional State
```

Some states may always be applied.

Others may depend on tests.

---

# Conditional Product States

A recipe can apply a state only when a condition is true.

Conceptually:

```text
Does Sim have Trait X?
        |
       Yes
        |
        v
Apply Special Object State
```

This lets the same recipe produce slightly different results depending on the creator.

---

# Quality

Crafted objects often have quality systems.

Conceptually:

```text
Base Quality
     |
     +--> Skill
     +--> Traits
     +--> Buffs
     +--> Other Modifiers
     |
     v
Final Quality
```

The exact formula depends on the crafting system.

The important concept is that quality usually results from several inputs rather than one fixed value.

---

# Skill and Quality

Skill often influences quality.

Conceptually:

```text
Low Skill
   |
   v
Lower Expected Quality

High Skill
   |
   v
Higher Expected Quality
```

Recipes may contain values that control how much skill contributes.

This means changing a skill requirement and changing quality behavior are separate modding goals.

---

# Masterworks and Special Results

Some crafting systems support special high-quality results, such as masterpieces.

Conceptually:

```text
Base Chance
     |
     +--> Skill Requirement
     +--> Mood
     +--> Trait
     +--> Buff
     |
     v
Special Result Chance
```

This is a good example of recipes connecting to several Sim-state systems at once.

---

# Mood and Crafting

Mood can influence crafting results.

Conceptually:

```text
Inspired
   |
   v
Increase Special Result Chance
```

or:

```text
Bored
   |
   v
Decrease Special Result Chance
```

The recipe may contain tests that detect the Sim's current mood and adjust the result.

---

# Traits and Crafting

Traits can also affect crafting.

Conceptually:

```text
Trait
   |
   +--> Better Quality
   +--> Higher Value
   +--> Different Object State
   +--> Different Result Chance
```

The exact behavior varies by recipe and crafting system.

---

# Buffs and Crafting

Buffs may influence:

- recipe weighting
- quality
- special results
- final-product behavior
- autonomy

Conceptually:

```text
Buff
 |
 v
Recipe Modifier
 |
 v
Different Crafting Result
```

This is another reason crafting systems can become highly interconnected.

---

# Loot and Final Products

A recipe may run loot when the final product is created.

Conceptually:

```text
Crafting Completes
        |
        v
Final Product
        |
        v
Loot
        |
        +--> Modify Quality
        +--> Change Sim State
        +--> Progress Another System
```

This means the recipe may not contain the full final consequence.

You may need to follow the loot references.

---

# Value

The value of a crafted object can depend on several factors.

Conceptually:

```text
Base Value
    |
    +--> Quality
    +--> Skill
    +--> Trait
    +--> Special State
    |
    v
Final Simoleon Value
```

This is separate from the crafting cost.

---

# Quality-Based Value

Different quality states can modify the object's value.

For example:

```text
Poor
   |
   v
Lower Value

Normal
   |
   v
Normal Value

Outstanding
   |
   v
Higher Value
```

This is one reason changing quality can affect the economy of a crafting mod even if you never directly edit price fields.

---

# Skill-Based Value

Some recipes may use a value curve based on skill.

Conceptually:

```text
Skill Level
    |
    v
Value Multiplier
    |
    v
Final Object Value
```

This allows the same recipe to become more profitable as the Sim improves.

---

# Autonomy and Recipes

Recipes may participate in autonomy.

Conceptually:

```text
Possible Recipes
      |
      v
Autonomy Evaluation
      |
      +--> Weight
      +--> Buffs
      +--> Availability
      |
      v
Recipe Selected
```

This matters when Sims choose crafting behavior on their own.

---

# Recipe Weighting

A recipe can have a weight that influences autonomous selection.

Other states may modify that weight.

Conceptually:

```text
Base Recipe Weight
        |
        +--> Trait/Buff Modifier
        |
        v
Adjusted Weight
```

Availability and desirability are separate concepts.

A recipe can be allowed but rarely selected autonomously.

---

# A Full Crafting Map

A more complete crafting system might look like:

```text
Sim
 |
 v
Crafting Interaction
 |
 v
Recipe
 |
 +--> Category
 |
 +--> Tests
 |     |
 |     +--> Skill
 |     +--> Traits
 |
 +--> Cost
 |
 +--> Crafting Phases
 |     |
 |     +--> Super Interactions
 |
 +--> Final Product
       |
       +--> Object Definition
       +--> Tags
       +--> States
       +--> Quality
       +--> Value
       +--> Loot
```

You do not need to inspect every branch for every mod.

Follow the branch related to your question.

---

# How to Investigate a Crafting Problem

Start by defining the exact issue.

## "The recipe does not appear."

Investigate:

```text
Category
Tests
Skill Requirement
Availability
```

---

## "The recipe costs too much."

Investigate:

```text
Crafting Cost
```

---

## "The wrong object is created."

Investigate:

```text
Final Product
Definition
```

---

## "The Sim cannot resume the craft."

Investigate:

```text
Resume Affordance
Crafting Object
Phase System
```

---

## "The quality is wrong."

Investigate:

```text
Quality Adjustment
Skill
Traits
Buffs
Loot
```

---

## "The object sells for too much."

Investigate:

```text
Value
Quality Modifiers
Skill Curves
Object States
```

---

## "Something happens after crafting that I don't understand."

Investigate:

```text
Final Product
Loot
```

---

# Common Beginner Mistakes

## Assuming Recipe Means Food

Recipe tuning is broader than cooking.

---

## Editing the Interaction When the Recipe Controls the Value

The crafting action and production rules are separate.

---

## Editing the Recipe When the Object Definition Is Wrong

The result object may be a separate resource.

---

## Ignoring Crafting Phases

Multi-step crafting may rely on several interactions.

---

## Ignoring Resume Behavior

Starting and resuming a craft can use different interactions.

---

## Treating Cost and Value as the Same Thing

They are separate.

---

## Ignoring Loot

Some important final-product behavior may happen through loot.

---

## Changing Quality Without Considering Value

Quality can affect the object's Simoleon value.

---

## Assuming One Recipe Class Represents Every Crafting System

Different recipe classes can expose different tunables.

---

# A Good Crafting Investigation Order

When a crafting system does not behave as expected, check:

```text
1. Is the recipe available?
2. Are the tests passing?
3. Is the correct crafting interaction running?
4. Are the phases progressing?
5. Is the correct final product created?
6. Are the expected states and tags applied?
7. Is quality being calculated correctly?
8. Is loot running?
9. Is the resulting value correct?
10. Can the object be resumed if unfinished?
```

This keeps the investigation focused.

---

# When to Edit the Recipe

The recipe is a likely place to edit when your goal involves:

- crafting cost
- skill requirements
- recipe categories
- crafting phases
- final-product configuration
- quality
- masterpiece behavior
- value scaling
- recipe-specific loot
- autonomy weight
- resume interactions

---

# When to Look Somewhere Else

If your goal is mainly:

```text
How the Sim performs the action
```

look at the interaction.

If your goal is:

```text
What the final object fundamentally is
```

look at the object definition.

If your goal is:

```text
What happens after crafting
```

look at loot.

If your goal is:

```text
What condition blocks the recipe
```

look at the tests.

If your goal is:

```text
What numeric skill is being tracked
```

look at the statistic.

---

# If You Remember Only 5 Things

1. **Recipes are not limited to food; they define many kinds of craftable or producible content.**
2. **The interaction defines the action, while the recipe defines the product and production rules.**
3. **Recipes can connect to tests, skills, phases, object definitions, quality, value, loot, and autonomy.**
4. **Cost and final-product value are separate concepts.**
5. **When debugging crafting, follow only the branch of the system related to your specific problem.**

---

# Practice Before Moving On

Choose one EA recipe and build a simple map:

```text
Recipe:
Category:
Skill Requirement:
Crafting Cost:
Crafting Interaction:
Final Product:
Quality System:
Loot:
Resume Interaction:
```

You do not need to fill every field if the recipe does not use it.

The goal is to recognize how the crafting system is divided across resources.

---

## Reference Library

For deeper explanations of recipe and crafting-related resources, use:

[The Tuning Type Reference](../../reference/tuning-types/README.md)

---

# Next Module

Continue to:

[Module 07 — Debugging & Maintenance](../07-debugging-and-maintenance/README.md)
