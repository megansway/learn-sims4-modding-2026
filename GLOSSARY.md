# Sims 4 Modding Glossary

A quick-reference glossary for common modding terms in Sims 4.

This file is meant for fast lookups. If you want a deeper explanation of a topic, check the course or the tuning reference library.

---

## A

### Affordance

An interaction-related tuning resource that represents something a Sim can potentially do.

You may see terms such as:

- super affordance
- mixer affordance
- resume affordance

The exact role depends on the system using it.

### Actor

The Sim performing an interaction.

For example:

```text
Actor
  |
  v
Performs Interaction
  |
  v
Target
```

In a social interaction, the Actor is usually the Sim initiating the action.

### Autonomy

The system that helps decide what Sims choose to do without direct player input.

Autonomy can consider things such as:

- available interactions
- tests
- buffs
- traits
- commodities
- scores
- weights
- current context

An interaction being available does not automatically mean the game will choose it autonomously.

---

## B

### Buff

A gameplay state or effect applied to a Sim.

Buffs can be:

- visible moodlets
- hidden gameplay states
- temporary effects
- internal markers used by other systems

Buffs may affect mood, autonomy, tests, interactions, or other gameplay behavior.

### Buff Replacement

A system where one buff is replaced with another based on a condition such as a trait.

Conceptually:

```text
Normal Buff
    |
    v
Trait Detected
    |
    v
Trait-Specific Buff
```

---

## C

### Commodity

A type of statistic commonly used for dynamic values that may rise, fall, decay, or influence ongoing simulation behavior.

Examples can include motives and hidden autonomy-driving values.

A useful beginner model is:

```text
Commodity
    |
    v
Dynamic Statistic
```

### Conflict

A situation where two mods or tuning resources compete to modify the same game resource.

For example:

```text
EA Interaction
    |
    +--> Mod A Override
    |
    +--> Mod B Override
```

Only one effective version of the overridden resource can be used.

---

## D

### Definition

An object resource that identifies the actual object created or used by the game.

Recipes may reference a definition when specifying a final crafted product.

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

---

## E

### Enum

A predefined named value used by tuning.

In extracted XML, enums commonly appear with the `E` tag.

For example:

```xml
<E>ADULT</E>
```

or:

```xml
<E>PERSONALITY</E>
```

---

## F

### Final Product

The object or result produced by a recipe or crafting process.

Final-product tuning may include:

- object definition
- tags
- states
- quality
- value
- loot

---

## I

### Instance ID

A numeric identifier used by the game to identify a specific tuning or resource instance.

For example:

```text
16823
```

may point to:

```text
trait_Ambitious
```

Instance IDs are important because tuning resources often reference one another by ID.

### Interaction

A gameplay action that a Sim, object, or system can perform.

Interactions can involve:

- tests
- participants
- outcomes
- loot
- autonomy
- statistics
- recipes
- animations
- other interactions

### Interaction Queue

The list of interactions a Sim is currently performing or waiting to perform.

Interactions may be added, interrupted, canceled, or reordered depending on gameplay.

---

## L

### LastException

A diagnostic file generated when The Sims 4 encounters certain Python or simulation errors.

LastException files can provide clues about:

- what system failed
- which interaction or object was involved
- which Python module raised an error
- whether a custom script mod appears in the error path

A LastException is evidence, not always a complete diagnosis.

### Loot

A system used to apply gameplay effects or changes.

Loot may:

- add buffs
- remove buffs
- add or remove traits
- modify statistics
- change relationships
- affect objects
- trigger other loot

A useful way to remember it is:

```text
Tests:
"Should this happen?"

Loot:
"What happens?"
```

---

## M

### Mixer Interaction

An interaction that typically runs within the context of another interaction.

A simplified example:

```text
Main Interaction
      |
      +--> Mixer
      +--> Mixer
      +--> Mixer
```

Mixer interactions are commonly encountered in social and other layered interaction systems.

### Mod

A file or group of files that changes, adds, or extends how The Sims 4 behaves.

Mods may include:

- tuning
- package resources
- custom assets
- Python scripts
- combinations of these systems

### Mood

A Sim's emotional state, such as Confident, Inspired, or Bored.

Buffs can contribute to moods through mood type and mood weight.

### Mood Weight

A value that determines how strongly a buff contributes to a mood.

Mood weight is not the same thing as buff duration.

### Motive

A need-like gameplay value such as Hunger or Energy.

Motives are commonly implemented using commodity/statistic systems.

---

## O

### Object State

A state applied to an object that changes or tracks some aspect of its current condition.

Examples might include:

- quality
- value-related state
- use state
- broken state
- special crafted state

### Outcome

A possible result of an interaction.

An interaction may have different outcomes such as:

```text
Success
Failure
```

Different outcomes can run different loot or effects.

### Override

A modded resource that replaces an existing EA resource by using the same resource identity.

Conceptually:

```text
EA Resource
    |
    v
Modded Override
```

Overrides can conflict when multiple mods replace the same resource.

---

## P

### Participant

A Sim, object, household, or other entity involved in an interaction, test, or loot action.

Common participant concepts include:

- Actor
- Target
- Target Sim
- Object
- Household

Participant configuration helps determine who or what receives an effect.

### Python Script Mod

A mod that uses Python code to create or control gameplay behavior.

Script mods commonly use the `.ts4script` file extension.

---

## R

### Recipe

A tuning resource that defines something the game can create, craft, prepare, or produce and the rules surrounding that process.

A recipe may define or reference:

- skill requirements
- crafting cost
- crafting phases
- final product
- quality
- value
- loot
- interactions

Recipes are not limited to food.

### Reference

A connection from one tuning resource to another.

For example:

```text
Interaction
    |
    v
Loot ID
    |
    v
Loot Tuning
```

Following references is one of the most important Sims 4 modding skills.

### Resource

A piece of game data stored in a package or tuning system.

Different resources can represent things such as:

- tuning
- objects
- strings
- images
- interactions
- traits
- buffs
- recipes

### Resume Affordance

An interaction used to continue an unfinished activity or crafting process.

For example:

```text
Unfinished Painting
       |
       v
Resume Affordance
       |
       v
Continue Painting
```

---

## S

### Sim Filter

A system used to identify Sims matching certain conditions.

Filters may be used by gameplay systems that need to locate Sims with specific traits, ages, roles, relationships, or other properties.

### Snippet

A reusable tuning resource that stores configuration other tuning can reference.

Snippets can contain many different kinds of data depending on their snippet class.

A useful mental model is:

```text
Multiple Resources
       |
       v
Shared Snippet
```

### Statistic

A numeric value tracked by the game.

Statistics can represent:

- skills
- counters
- progression
- motives
- hidden values
- relationship values
- other gameplay data

### Super Interaction

A primary interaction that can generally run as a complete action.

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

### Super Affordance

An affordance that points to or represents a super interaction.

Objects, recipes, or other systems may reference super affordances to provide actions.

---

## T

### Target

The Sim or object an interaction is directed toward.

For example:

```text
Actor
  |
  v
Interaction
  |
  v
Target
```

### Test

A condition check used to determine whether something is valid, allowed, available, or appropriate.

Tests may check things such as:

- traits
- buffs
- skills
- ages
- relationships
- object states
- time
- location
- statistics

A useful way to remember tests is:

> **Tests ask: "Should this happen?"**

### Test Set

A reusable group of one or more tests.

Test sets may be stored as snippet tunings so the same conditions can be reused in multiple places.

### Trait

A persistent characteristic or gameplay marker attached to a Sim.

Traits may represent:

- personality
- rewards
- hidden states
- roles
- progression
- system markers

Traits can affect buffs, tests, interactions, autonomy, and other systems.

### Tuning

Configuration data used by The Sims 4 to define gameplay behavior.

Tuning can describe things such as:

- interactions
- buffs
- traits
- recipes
- statistics
- loot
- tests
- situations
- autonomy
- objects

### Tuning Class

The class that determines what kind of tuning resource is being defined.

In extracted XML, this is commonly shown with:

```xml
c="..."
```

For example:

```xml
c="Buff"
```

### Tuning Type

The resource's tuning category.

In extracted XML, this is commonly shown with:

```xml
i="..."
```

For example:

```xml
i="buff"
```

The tuning type and tuning class should usually be read together.

---

## X

### XML

A text-based markup format used to represent much of the game's extracted tuning data.

Common structures include:

```text
T = single tunable value
L = list
U = structured group
V = variant
E = enum
```

You do not need to memorize XML immediately. Focus first on names, values, references, lists, and structure.

---

## Quick Concept Guide

If you only need a fast reminder:

| Concept | Think of it as... |
|---|---|
| Interaction | What the Sim does |
| Test | Should this happen? |
| Loot | What happens? |
| Buff | What is affecting the Sim right now? |
| Trait | What is generally true about the Sim? |
| Statistic | A tracked numeric value |
| Commodity | A dynamic tracked value |
| Recipe | What is being made and the rules for making it |
| Snippet | Reusable tuning data |
| Participant | Who or what the tuning applies to |
| Instance ID | The identifier used to point to a resource |
| Override | A modded replacement for an EA resource |

---

## Need More Detail?

For structured lessons, use:

[Course Map](COURSE-MAP.md)

For deeper explanations of individual tuning systems, use:

[Tuning Type Reference](reference/tuning-types/README.md)
