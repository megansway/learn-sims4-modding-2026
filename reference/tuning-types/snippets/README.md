# Snippet Tunings

> **Status:** ◆ Draft  
> **Last verified:** 2026-09-07  
> **Game version:** To be verified against current extracted tuning  
> **Pack requirements:** Varies by snippet

---

## In Plain English

A **snippet tuning** is a reusable chunk of tuning data that other resources can reference.

Instead of defining the same configuration over and over again in multiple interactions, traits, buffs, situations, or systems, the game can define that information once inside a snippet and reuse it elsewhere.

A useful way to think about snippets is:

> **A snippet is reusable tuning that other tuning can plug into.**

---

## Mental Model

```text
Interaction / Trait / Buff / System
                |
                v
          Snippet Reference
                |
                v
      Reusable Tuning Data
```

The snippet itself is usually not the thing that starts gameplay behavior. Another resource references it and uses the data stored inside it.

---

## What Does a Snippet Actually Do?

There is no single answer, because **snippet** is a broad category.

Different snippet types can store very different kinds of data. One snippet might contain a reusable list of interactions, while another might contain loot, tests, filters, tunable values, or configuration for a specific gameplay system.

That means you should not assume two snippets behave the same way just because both are categorized as snippets.

> [!IMPORTANT]
> "Snippet" describes a reusable tuning resource category. The snippet's **class or type** is what tells you what kind of data it actually contains.

---

## Why Does the Game Use Snippets?

Reusability is the main reason.

Imagine three interactions all need the same set of tests:

```text
Interaction A ----\
Interaction B ----- > Shared Test Configuration
Interaction C ----/
```

Without a reusable resource, that same configuration might need to be duplicated three times.

With a snippet, the game can define the shared information once:

```text
Interaction A ----\
Interaction B ----- > Snippet
Interaction C ----/
```

This makes tuning easier to reuse and maintain.

---

## Where Will I See Snippets?

You may encounter snippet references while working with many different systems, including:

- interactions
- buffs
- traits
- situations
- autonomy
- filters
- loot
- crafting systems
- relationship systems
- object behavior
- other reusable gameplay configuration

Because snippets are so flexible, they can appear in places that initially seem unrelated.

---

## How Snippets Connect to Other Tuning

A snippet often sits between one tuning resource and another piece of reusable configuration.

For example:

```text
Interaction
    |
    v
Snippet
    |
    +--> Tests
    +--> Loot
    +--> Interaction List
    +--> Filters
```

Or:

```text
Trait
   |
   v
Snippet
   |
   v
Reusable Loot Configuration
```

The exact relationship depends on the snippet type.

The important thing is that a snippet often acts as a reusable middle layer.

---

## What Commonly References Snippets?

Depending on the system, snippets may be referenced by:

- interactions
- traits
- buffs
- situations
- services
- objects
- autonomy systems
- crafting systems
- other tuning resources

There is no single universal list because snippet usage depends heavily on the snippet's class.

---

## What Can Snippets Reference?

Again, this varies by type.

A snippet may contain or reference things such as:

- loot actions
- tests
- test sets
- buffs
- statistics
- commodities
- interactions
- Sim filters
- object filters
- relationship tuning
- tunable lists
- other snippets
- other tuning resources

> [!NOTE]
> A snippet should be read based on what its specific tunables are doing, not based only on the fact that it is a snippet.

---

## What Does a Snippet Look Like in Tuning?

A simplified example might look conceptually like this:

```xml
<I c="ExampleSnippetClass"
   i="snippet"
   m="example.module"
   n="example_snippet_name"
   s="123456789">

    <L n="example_list">
        <T>111111111</T>
        <T>222222222</T>
    </L>

</I>
```

This is only an illustrative example.

The important parts to recognize are:

- the resource is identified as a snippet
- it has a specific class
- it has a name
- it has an instance ID
- it contains tunables defined by that snippet class

The tunables are what determine what the snippet is actually storing.

---

## Important Things to Look For

When you open a snippet, ask:

1. What is the snippet class?
2. What tunables does it contain?
3. What other resources does it reference?
4. What tuning references this snippet?
5. Is the snippet storing data, tests, loot, lists, or another reusable configuration?
6. Does the behavior you care about happen inside the snippet, or does the snippet point somewhere else?

These questions are usually more useful than simply asking:

> "What does this snippet do?"

because the answer depends on the snippet type.

---

## Following a Snippet Reference

Imagine you are investigating an interaction and find a reference to a snippet.

```text
Interaction
    |
    v
Snippet ID: 123456789
```

You locate the snippet and discover:

```text
Snippet
    |
    v
Loot Reference: 987654321
```

Then the loot contains:

```text
Loot
  |
  v
Add Buff
```

The complete behavior is therefore:

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

The snippet was only one part of the chain.

This is why following references is so important when reverse-engineering Sims 4 tuning.

---

## Snippet vs Loot

These are easy to confuse because a snippet can contain or reference loot.

| Snippet | Loot |
|---|---|
| Reusable tuning container/configuration | Gameplay effect or set of effects |
| Can store many kinds of data | Usually changes game state |
| May reference loot | May be referenced by a snippet |
| Meaning depends heavily on snippet class | Meaning depends on the loot actions inside it |

A snippet may help organize loot, but a snippet is not automatically loot.

---

## Snippet vs Test

A snippet can also contain reusable tests or test-related configuration.

| Snippet | Test |
|---|---|
| Reusable tuning resource | Condition check |
| Can store or reference tests | Determines whether something passes or fails |
| Broad category | Specific gameplay purpose |

If a snippet contains tests, the snippet is acting as a reusable container for those conditions.

---

## Common Modding Uses

Modders may work with snippets when they want to:

- add entries to reusable lists
- alter reusable configuration
- change shared tests
- change shared loot
- adjust filters
- modify systems used by multiple interactions
- reuse their own configuration across multiple tunings

Because snippets can be referenced in many places, changing one can sometimes affect multiple systems at once.

> [!WARNING]
> If several tuning resources reference the same snippet, editing or overriding that snippet may affect all of them. Always check where the snippet is used before assuming the change is isolated.

---

## Common Mistakes

### Assuming all snippets have the same structure

They do not.

Different snippet classes can have completely different tunables.

### Assuming the snippet itself performs the final effect

Sometimes it does contain the important configuration, but sometimes it simply points to loot, tests, filters, or other resources.

### Ignoring what references the snippet

A snippet can be shared by multiple systems. Understanding what uses it is often just as important as understanding what is inside it.

### Modifying a shared snippet without checking its other uses

A change intended for one interaction may affect other interactions that reference the same snippet.

---

## FAQ

### Is a snippet a type of interaction?

No.

A snippet is its own reusable tuning resource category. An interaction may reference a snippet.

### Does a snippet always contain loot?

No.

Some snippets may contain or reference loot, but others can store entirely different kinds of data.

### Does a snippet run on its own?

Usually not in the way an interaction or event does. Another system generally references the snippet and uses its data.

### Why did I find a snippet while looking at an interaction?

Because the interaction may rely on reusable configuration stored in that snippet.

### Why does the snippet seem incomplete?

Because the snippet may reference other tuning resources. Follow those references.

### Can multiple resources use the same snippet?

Yes. Reuse is one of the main reasons snippets exist.

### Are snippets only used by Base Game systems?

No. Pack-specific systems can use snippets too.

### Can snippets reference other snippets?

Depending on the tuning and snippet class, reusable tuning can reference other reusable resources. Always inspect the actual fields rather than assuming the relationship.

---

## Advanced Notes

The most important technical detail about snippets is that **the snippet class matters more than the word "snippet" by itself**.

When investigating a snippet, identify its class and inspect the tunables available to that class. That tells you what role the snippet plays in the system.

In practice, this means that documenting "snippets" as one single concept is useful for understanding the overall pattern, but individual snippet classes may eventually deserve their own reference pages.

For example, the reference library may later separate commonly encountered snippet families into their own entries if they behave differently enough to justify it.

---

## If You Remember Only 3 Things

1. **Snippets are reusable tuning resources.**
2. **Different snippet classes can contain completely different kinds of data.**
3. **When a snippet does not explain the full behavior, follow its references and check what references it.**

---

## Related Reference Pages

- [Loot Actions](../loot/README.md)
- [Tests](../tests/README.md)
- [Interactions](../interactions/README.md)

---

## Related Course Material

- [Module 01 — Tuning Foundations](../../../modules/01-tuning-foundations/README.md)
- [Module 02 — Reading EA Tuning](../../../modules/02-reading-ea-tuning/README.md)
