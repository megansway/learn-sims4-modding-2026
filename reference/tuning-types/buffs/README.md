# Buffs

> **Status:** ✓ Up-to-date  
> **Last verified:** 2026-09-07  
> **Game version:** 1.127.41.1030  
> **Pack requirements:** Varies by buff

---

## In Plain English

A **buff** is a gameplay state applied to a Sim.

Buffs can represent things the player sees, such as a moodlet, but they can also be hidden and used internally by the game to track conditions, trigger behavior, or influence other systems.

A useful way to think about buffs is:

> **A buff tells the game that a Sim is currently in a particular state or under a particular effect.**

---

## Mental Model

```text
Something happens
        |
        v
Buff is added
        |
        v
Sim enters a gameplay state
        |
        +--> Mood may change
        +--> Tests may detect it
        +--> Other systems may react
        +--> Buff may eventually expire
```

Not every buff does all of these things, but this is the general pattern.

---

## Verified Example

The sample used here is:

```text
Buff_Confident_BragAboutStartup
```

Its top-level tuning begins with:

```xml
<I c="Buff"
   i="buff"
   m="buffs.buff"
   n="Buff_Confident_BragAboutStartup"
   s="37296">
```

This identifies the resource as a `Buff` tuning with the tuning type `buff`. :contentReference[oaicite:0]{index=0}

---

## Reading the Top-Level Tuning

### `c="Buff"`

```xml
c="Buff"
```

This identifies the tuning class as:

```text
Buff
```

### `i="buff"`

```xml
i="buff"
```

This identifies the tuning type as:

```text
buff
```

### `m="buffs.buff"`

```xml
m="buffs.buff"
```

This identifies the associated Python module as:

```text
buffs.buff
```

### `n="Buff_Confident_BragAboutStartup"`

This is the tuning name.

Names often give useful clues about the intended purpose. In this case:

```text
Confident
BragAboutStartup
```

suggests a Confident buff associated with bragging about a startup.

### `s="37296"`

This is the instance ID of the buff.

Other tuning can reference this ID when it wants to apply, remove, test for, replace, or otherwise use this buff. :contentReference[oaicite:1]{index=1}

---

## Buff Name and Description

The sample contains:

```xml
<T n="buff_name">
0x27E97246
<!-- String: "Bragged about Startup" -->
</T>
```

and:

```xml
<T n="buff_description">
0x1722289F
<!-- String: "Only a truly talented entrepreneur can make such claims about their startup." -->
</T>
```

These references provide the text shown to the player.

Conceptually:

```text
Buff Tuning
    |
    +--> Display Name
    |
    +--> Description
```

This shows that a buff can contain both gameplay configuration and UI-facing information. :contentReference[oaicite:2]{index=2}

---

## Buff Icons

The sample contains:

```xml
<T n="icon">
...
</T>
```

which points to an icon resource.

That icon is part of how the buff is presented in the user interface.

Conceptually:

```text
Buff
 |
 +--> Name
 +--> Description
 +--> Icon
```

A visible moodlet usually needs enough UI data for the game to present it clearly to the player. :contentReference[oaicite:3]{index=3}

---

## Mood Type

This sample contains:

```xml
<T n="mood_type">
14634
<!-- Mood: Mood_Confident -->
</T>
```

This associates the buff with the:

```text
Confident
```

mood. :contentReference[oaicite:4]{index=4}

Conceptually:

```text
Buff
   |
   v
Mood Type
   |
   v
Confident
```

This is one of the most visible ways buffs affect gameplay.

---

## Mood Weight

The sample also contains:

```xml
<T n="mood_weight">1</T>
```

So this buff contributes a mood weight of:

```text
1
```

to the associated mood. :contentReference[oaicite:5]{index=5}

A useful simplified model is:

```text
Buff
   |
   +--> Mood Type = Confident
   |
   +--> Mood Weight = 1
```

Multiple mood-related buffs may contribute toward the Sim's overall emotional state.

---

## Temporary Buff Information

The sample contains:

```xml
<V t="enabled" n="_temporary_commodity_info">
```

Inside it is:

```xml
<T n="max_duration">240</T>
```

and a category:

```text
Confident_Buffs
```

This verifies that the buff has temporary-duration configuration and is categorized among Confident buffs. :contentReference[oaicite:6]{index=6}

Conceptually:

```text
Temporary Buff
      |
      +--> Category
      |
      +--> Maximum Duration
```

> [!NOTE]
> Duration behavior can vary by buff. Do not assume every buff uses the same temporary configuration.

---

## Audio on Add and Remove

The sample contains both:

```xml
audio_sting_on_add
```

and:

```xml
audio_sting_on_remove
```

These point to audio resources used when the buff is added or removed. :contentReference[oaicite:7]{index=7}

This shows that buffs can participate in presentation as well as gameplay state.

---

## What Does a Buff Actually Do?

A buff can serve several different purposes depending on how it is configured.

A buff may:

- contribute to a mood
- act as a temporary gameplay state
- be checked by tests
- be added or removed through loot
- influence autonomy
- unlock or block behavior
- act as a hidden marker
- participate in trait or interaction systems
- affect how other tuning behaves

The verified sample specifically shows a visible Confident buff with a name, description, icon, mood type, mood weight, duration-related settings, and add/remove audio. :contentReference[oaicite:8]{index=8}

---

## Visible vs Hidden Buffs

Not every buff needs to be shown to the player.

Some buffs are primarily used as internal state markers.

Conceptually:

```text
Visible Buff
    |
    +--> Moodlet
    +--> Name
    +--> Description
    +--> Icon

Hidden Buff
    |
    +--> Tracks State
    +--> Used by Tests
    +--> Triggers Other Behavior
```

The specific sample here is clearly player-facing because it defines visible UI text and an icon. :contentReference[oaicite:9]{index=9}

---

## How Buffs Connect to Other Tuning

Buffs are highly connected.

A simplified chain might look like:

```text
Interaction
    |
    v
Loot
    |
    v
Buff
    |
    +--> Mood
    +--> Tests
    +--> Autonomy
    +--> Other Gameplay
```

Another common pattern is:

```text
Trait
   |
   v
Buff
   |
   v
Gameplay Effect
```

Your Ambitious trait sample gives a concrete example of a trait directly referencing buffs, which we will cover on the Trait page. :contentReference[oaicite:10]{index=10}

---

## What Commonly References Buffs?

Buffs may be referenced by systems such as:

- loot
- traits
- interactions
- tests
- situations
- aspirations
- careers
- autonomy systems
- recipes
- objectives
- other buffs
- object behavior

The specific relationship depends on the system.

---

## What Can Buffs Reference?

Depending on the buff class and configuration, buffs may connect to:

- moods
- commodities
- statistics
- interactions
- loot
- visual/audio resources
- UI strings
- other gameplay systems

The verified example directly shows references to a mood, strings, icon data, audio data, and temporary-buff configuration. :contentReference[oaicite:11]{index=11}

---

## Buffs vs Traits

Buffs and traits can both influence gameplay, but they generally serve different roles.

| Buff | Trait |
|---|---|
| Represents a gameplay state or effect | Represents a persistent characteristic or marker |
| Often temporary | Often longer-term or persistent |
| Can influence mood | Can modify broader behavior |
| Frequently added and removed | Usually remains until deliberately changed |
| Can be referenced by traits | Can provide or replace buffs |

A trait may cause a Sim to receive specific buffs, while the buff represents the temporary state that results.

---

## Buffs vs Loot

These are also easy to confuse.

| Buff | Loot |
|---|---|
| A state/effect applied to a Sim | An action that changes gameplay state |
| Can exist on the Sim | Runs and performs something |
| May be added or removed | May add or remove buffs |
| Can be detected by tests | Can be triggered after tests |

A useful chain is:

```text
Loot
 |
 v
Add Buff
 |
 v
Sim now has Buff
```

---

## Common Modding Uses

Buff tuning is important when you want to change:

- mood effects
- mood strength
- buff duration
- visible text
- icons
- temporary states
- hidden states
- how other systems detect a Sim's state
- which effects appear after interactions

If the question is:

> "Why does this Sim currently have this effect?"

the buff is often one of the most important resources to inspect.

---

## Common Mistakes

### Assuming every buff is a moodlet

Some buffs are hidden and never appear in the UI.

### Assuming the buff caused itself

Another resource usually adds the buff.

Look backward through references to find what triggered it.

### Ignoring the mood type

If the buff affects emotion, the `mood_type` is critical.

### Confusing mood weight with duration

These are different concepts.

Mood weight contributes to emotional strength.

Duration controls how long the buff remains.

### Assuming every buff has the same fields

Different buffs can have very different configurations.

---

## FAQ

### Is a buff the same thing as a mood?

No.

A buff may contribute to a mood, but the buff and the mood are separate resources.

### Can a Sim have multiple buffs?

Yes.

The game can track many buffs on a Sim at the same time.

### Are all buffs visible?

No.

Some are hidden and exist only for gameplay logic.

### Can loot add a buff?

Yes.

Loot commonly performs buff operations.

### Can traits give Sims buffs?

Yes.

The Ambitious trait sample directly references a buff associated with the trait. :contentReference[oaicite:12]{index=12}

### Can a buff expire?

Yes.

This sample contains temporary-buff duration configuration. :contentReference[oaicite:13]{index=13}

### Can buffs affect mood strength?

Yes.

This sample has:

```text
mood_type = Confident
mood_weight = 1
```

:contentReference[oaicite:14]{index=14}

---

## If You Remember Only 3 Things

1. **A buff represents a gameplay state or effect on a Sim.**
2. **Buffs can be visible moodlets or hidden internal states.**
3. **If you want to know why a buff exists, follow the tuning that added or references it.**

---

## Related Reference Pages

- [Traits](../traits/README.md)
- [Loot Actions](../loot/README.md)
- [Tests](../tests/README.md)
- [Statistics](../statistics/README.md)
- [Commodities](../commodities/README.md)
- [Interactions](../interactions/README.md)

---

## Related Course Material

- [Module 01 — Tuning Foundations](../../../modules/01-tuning-foundations/README.md)
- [Module 05 — Buffs, Traits & Statistics](../../../modules/05-buffs-traits-statistics/README.md)
