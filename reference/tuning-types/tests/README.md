# Tests

> **Status:** ✓ Documented  
> **Last verified:** 2026-09-07  
> **Game version:** 1.127.41.1030  
> **Pack requirements:** Varies by test

---

## In Plain English

A **test** checks whether a condition is true.

Tests are used throughout The Sims 4 to decide whether something is allowed, available, valid, or appropriate before the game continues with a piece of behavior.

A useful way to remember them is:

> **Tests answer: "Should this happen?"**

---

## Mental Model

```text
Game wants to do something
        |
        v
Run Test
    /       \
  Pass      Fail
   |          |
   v          v
Continue     Stop / Reject / Hide
```

The exact result of a failed test depends on the system using it, but the basic idea is the same: tests are conditions.

---

## Verified Example

The sample used here is:

```text
testSet_ActorIsGhost
```

Its top-level tuning is:

```xml
<I c="TestSetInstance"
   i="snippet"
   m="event_testing.tests"
   n="testSet_ActorIsGhost"
   s="101697">
```

This tells us several useful things immediately.

---

## Reading the Top-Level Tuning

### `c="TestSetInstance"`

```xml
c="TestSetInstance"
```

This identifies the class as:

```text
TestSetInstance
```

That means the resource represents a reusable set of tests.

---

### `i="snippet"`

```xml
i="snippet"
```

This test set is stored as a:

```text
snippet
```

So this sample gives us a direct example of how snippets and tests can connect:

```text
Snippet
   |
   v
Reusable Test Set
```

> [!IMPORTANT]
> A test does not have to be its own standalone top-level tuning in every context. Tests can also appear inside other tuning structures, and reusable test sets can be packaged as snippets.

---

### `m="event_testing.tests"`

```xml
m="event_testing.tests"
```

This identifies the associated Python module as:

```text
event_testing.tests
```

That is a strong clue that this tuning belongs to the game's event-testing system.

---

### `n="testSet_ActorIsGhost"`

```xml
n="testSet_ActorIsGhost"
```

This is the tuning name.

The name is very descriptive:

```text
testSet
Actor
IsGhost
```

It strongly suggests that this test set checks whether the actor is a ghost.

---

### `s="101697"`

```xml
s="101697"
```

This is the instance ID of the test-set snippet.

Other tuning can reference this ID to reuse the same condition.

---

## The `test` List

The core of the sample is:

```xml
<L n="test">
```

Inside that is another list containing the actual test:

```xml
<V t="trait">
```

So conceptually:

```text
Test Set
   |
   v
test
   |
   v
Trait Test
```

This tells us the test set contains a trait-based condition.

---

## The Test Type

The selected test variant is:

```xml
<V t="trait">
```

That means this test is using a:

```text
trait
```

test.

Different test variants can check completely different conditions.

For example, other tests may evaluate things such as:

- traits
- buffs
- skills
- moods
- ages
- relationships
- object states
- location
- time
- statistics
- participants
- many other gameplay conditions

> [!IMPORTANT]
> The test type determines what the test actually checks. Do not assume every test has the same fields as this trait test.

---

## What This Test Actually Checks

Inside the trait test is:

```xml
<L n="whitelist_trait_types">
    <E>GHOST</E>
</L>
```

This means the test allows the trait type:

```text
GHOST
```

Conceptually:

```text
Actor
  |
  v
Trait Test
  |
  v
Is the actor a GHOST?
    /        \
  Yes        No
   |          |
   v          v
 Pass        Fail
```

So `testSet_ActorIsGhost` is a reusable condition that checks whether the tested Sim qualifies as a ghost through this trait-type test.

---

## Whitelists

This sample uses:

```text
whitelist_trait_types
```

A whitelist means:

> Only values matching the listed entries are accepted.

In this case:

```text
Allowed trait type:
GHOST
```

So if the tested Sim matches that trait type, the condition passes.

---

## Tests Can Be Reusable

Because this test set is a snippet, another tuning resource can reference it instead of recreating the same ghost check repeatedly.

Conceptually:

```text
Interaction A ----\
Interaction B ----- > testSet_ActorIsGhost
Situation -------/
```

That gives us a clean example of why snippet tunings are useful.

The game can define:

```text
"Actor must be a ghost"
```

once and reuse it wherever needed.

---

## How Tests Connect to Other Tuning

Tests are often used before another behavior is allowed to continue.

A conceptual chain might be:

```text
Interaction
    |
    v
Test Set
    |
    v
Actor Is Ghost?
    |
   Pass
    |
    v
Interaction Continues
```

Or:

```text
Loot Trigger
    |
    v
Test
    |
   Pass
    |
    v
Loot Runs
```

The test itself does not usually create the final gameplay effect.

It determines whether another system is allowed to proceed.

---

## Tests vs Loot

Tests and loot are commonly used together but serve opposite roles.

| Tests | Loot |
|---|---|
| Evaluate conditions | Apply effects |
| "Should this happen?" | "What happens?" |
| Can allow or reject behavior | Changes game state |
| Usually read information | Usually modifies information or state |

A simple example:

```text
Is Actor a Ghost?
        |
       Yes
        |
        v
Run Loot
        |
        v
Apply Effect
```

The ghost check is the test.

The effect afterward is the loot.

---

## Tests vs Snippets

This sample shows that these concepts can overlap structurally.

| Test | Snippet |
|---|---|
| A condition check | A reusable tuning resource |
| Evaluates something | Stores reusable configuration |
| Can exist inside other tuning | Can contain reusable tests |
| Has a gameplay purpose | Has a structural/reuse purpose |

In this example:

```text
Snippet
   |
   v
TestSetInstance
   |
   v
Trait Test
```

So the snippet is the reusable container, while the test is the condition being evaluated.

---

## Test Sets

A **test set** groups one or more tests together.

This sample contains a single trait test, but the structure itself is a test set.

Conceptually:

```text
Test Set
   |
   +--> Test A
   +--> Test B
   +--> Test C
```

The exact way multiple tests combine can depend on the structure being used, so always inspect the actual XML instead of assuming the logic.

> [!NOTE]
> This sample verifies a single-test `TestSetInstance`. More complex test-set behavior should be documented from additional samples.

---

## What Commonly Uses Tests?

Tests may appear in or be referenced by many systems, including:

- interactions
- loot
- snippets
- recipes
- buffs
- traits
- situations
- careers
- aspirations
- autonomy systems
- object behavior
- filters
- crafting systems

Their purpose remains the same even when the surrounding system changes:

> Check whether a condition is true before allowing behavior to continue.

---

## Common Things Tests Can Check

Different test types can evaluate different parts of the game state.

Examples include:

### Sim State

- traits
- buffs
- age
- species
- mood
- occult state

### Progression

- skill level
- statistic values
- aspiration progress
- career information

### Relationships

- relationship values
- relationship bits
- family relationships
- social context

### Objects

- object states
- tags
- ownership
- location
- availability

### World and Context

- venue
- zone
- time
- season
- household state
- participants

Not every test supports every kind of condition. The selected test type determines the available tunables.

---

## Participants Matter

Tests often need to know **who or what is being tested**.

In this example, the tuning name tells us the intended subject is:

```text
Actor
```

Conceptually:

```text
Actor
  |
  v
Trait Test
  |
  v
Ghost?
```

Other tests may target:

```text
Actor
Target Sim
Object
Household
Another Participant
```

> [!TIP]
> If a test seems logically correct but behaves unexpectedly, check which participant the test is evaluating.

---

## How to Read an Unknown Test

When you encounter a test, work through it in this order:

1. Identify the test container or resource.
2. Find the test type.
3. Determine which participant is being tested.
4. Read the whitelist, blacklist, threshold, or other condition.
5. Follow any referenced IDs.
6. Determine what happens if the test passes.
7. Determine what happens if it fails.

For this sample:

```text
1. TestSetInstance
       |
2. trait
       |
3. Actor
       |
4. whitelist_trait_types
       |
5. GHOST
```

That gives us the condition:

```text
Actor must qualify as a ghost.
```

---

## Common Mistakes

### Assuming a test changes something

A test usually evaluates state rather than changing it.

### Confusing the test container with the test itself

In this sample:

```text
TestSetInstance
```

is the reusable test-set resource.

The actual condition inside it is:

```text
trait
```

### Ignoring whitelists and blacklists

These often tell you exactly what values are allowed or rejected.

### Ignoring participants

A test may be correct but applied to the wrong Sim or object.

### Assuming all tests use IDs

Some fields may use enums or other values instead.

This sample uses:

```text
GHOST
```

rather than a numeric trait instance ID.

---

## FAQ

### Does a test make something happen?

Not usually by itself.

A test evaluates a condition. Another system decides what to do with the result.

---

### Is every test a snippet?

No.

This sample is a reusable test set stored as a snippet, but tests can also appear inside other tuning structures.

---

### Is `TestSetInstance` the same thing as a trait test?

No.

`TestSetInstance` is the container.

The actual test variant here is:

```text
trait
```

---

### What does `whitelist_trait_types` mean?

It defines which trait types are accepted by the test.

This sample accepts:

```text
GHOST
```

---

### Does this test check for one specific ghost trait?

Not in this sample.

It checks the trait type:

```text
GHOST
```

rather than referencing one specific trait ID.

---

### Can a test set contain more than one test?

Yes, structurally it can represent a set of tests. The exact combination logic should be read from the specific tuning structure being used.

---

### Why would a modder use a reusable test set?

Because the same condition may be needed in multiple places.

Instead of recreating the test repeatedly, several tunings can reference the same reusable snippet.

---

## If You Remember Only 3 Things

1. **Tests check conditions; they do not usually create the final effect.**
2. **The test type tells you what kind of condition is being evaluated.**
3. **Tests can be stored in reusable test-set snippets and referenced by other tuning.**

---

## Related Reference Pages

- [Snippet Tunings](../snippets/README.md)
- [Loot Actions](../loot/README.md)
- [Interactions](../interactions/README.md)
- [Traits](../traits/README.md)
- [Buffs](../buffs/README.md)
- [Statistics](../statistics/README.md)

---

## Related Course Material

- [Module 01 — Tuning Foundations](../../../modules/01-tuning-foundations/README.md)
- [Module 02 — Reading EA Tuning](../../../modules/02-reading-ea-tuning/README.md)
