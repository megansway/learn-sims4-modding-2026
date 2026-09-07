# Traits

> **Status:** ✓ Up-to-date  
> **Last verified:** 2026-09-07  
> **Game version:** 1.127.41.1030  
> **Pack requirements:** Varies by trait

---

## In Plain English

A **trait** is a persistent characteristic or gameplay marker attached to a Sim.

Traits can represent personality, identity, progression, hidden states, rewards, roles, or other long-term gameplay information.

A useful way to think about traits is:

> **A trait tells the game something that is generally true about this Sim until that trait is changed or removed.**

---

## Mental Model

```text
Sim
 |
 +--> Trait
       |
       +--> Buffs
       +--> Tests
       +--> Conflicts
       +--> Special behavior
       +--> Replacements
       +--> UI information
```

Traits often influence many other systems rather than producing only one effect.

---

## Verified Example

The sample used here is:

```text
trait_Ambitious
```

Its top-level tuning begins with:

```xml
<I c="Trait"
   i="trait"
   m="traits.traits"
   n="trait_Ambitious"
   s="16823">
```

This identifies the resource as a `Trait` tuning with the tuning type `trait`. :contentReference[oaicite:15]{index=15}

---

## Reading the Top-Level Tuning

### `c="Trait"`

```xml
c="Trait"
```

This identifies the tuning class as:

```text
Trait
```

### `i="trait"`

```xml
i="trait"
```

This identifies the tuning type as:

```text
trait
```

### `m="traits.traits"`

```xml
m="traits.traits"
```

This identifies the associated Python module as:

```text
traits.traits
```

### `n="trait_Ambitious"`

This is the tuning name.

### `s="16823"`

This is the instance ID used to identify this trait resource. :contentReference[oaicite:16]{index=16}

---

## Trait Type

The sample contains:

```xml
<E n="trait_type">PERSONALITY</E>
```

So the Ambitious trait is categorized as:

```text
PERSONALITY
```

This shows that traits can have different trait types.

Conceptually:

```text
Trait
   |
   v
Trait Type
   |
   v
PERSONALITY
```

> [!IMPORTANT]
> Not every trait is a personality trait. Traits are used throughout the game for many different purposes.

---

## Ages

The trait contains:

```xml
<L n="ages">
    <E>ADULT</E>
    <E>ELDER</E>
    <E>YOUNGADULT</E>
</L>
```

So this trait is configured for:

```text
Young Adult
Adult
Elder
```

:contentReference[oaicite:17]{index=17}

This shows that traits can restrict which age groups may use them.

---

## Conflicting Traits

The sample contains:

```xml
<L n="conflicting_traits">
```

with:

```text
trait_Lazy
trait_Freegan
```

:contentReference[oaicite:18]{index=18}

Conceptually:

```text
Ambitious
   |
   +--> Conflicts with Lazy
   |
   +--> Conflicts with Freegan
```

This allows the game to prevent incompatible traits from existing together in contexts where the conflict is enforced.

---

## Trait Buffs

The sample contains:

```xml
<L n="buffs">
    <U>
        <T n="buff_type">
        12650
        <!-- Buff: Buff_Trait_Ambitious -->
        </T>
    </U>
</L>
```

This directly connects the Ambitious trait to:

```text
Buff_Trait_Ambitious
```

:contentReference[oaicite:19]{index=19}

Conceptually:

```text
Ambitious Trait
      |
      v
Buff_Trait_Ambitious
      |
      v
Ongoing Trait-Related Behavior
```

This is an important example of the relationship between traits and buffs.

---

## Buff Replacements

One of the largest sections in this trait is:

```xml
<L n="buff_replacements">
```

This maps normal buffs to Ambitious-specific replacement buffs.

For example:

```text
buff_Career_MovingUp
        |
        v
buff_Career_Promotion_Ambitious
```

and:

```text
buff_Career_Fired
        |
        v
buff_Career_Fired_Ambitious
```

:contentReference[oaicite:20]{index=20}

This means the trait can cause the Sim to receive a specialized version of a buff instead of the generic one.

Conceptually:

```text
Game wants to apply normal buff
            |
            v
Does Sim have Ambitious trait?
            |
           Yes
            |
            v
Use Ambitious-specific replacement buff
```

This is a powerful example of how traits can alter the behavior of other systems without replacing those systems entirely.

---

## Traits Can Modify Reactions

The replacement list includes buffs associated with:

- career promotion
- career failure
- fame rank changes
- swordsmanship outcomes
- other gameplay events

:contentReference[oaicite:21]{index=21}

That shows a trait can customize how a Sim reacts to gameplay outcomes.

The trait does not necessarily need to define every event itself.

Instead, it can intercept or replace the resulting buff.

---

## Trait Display Information

The sample contains:

```xml
<T n="display_name">
...
<!-- String: "Ambitious" -->
</T>
```

and:

```xml
<T n="trait_description">
...
<!-- String: "These Sims gain powerful Moodlets from career success, gain negative Moodlets from career failure, and may become upset if not promoted." -->
</T>
```

:contentReference[oaicite:22]{index=22}

The trait also has an icon:

```xml
<T n="icon">
...
</T>
```

This is the UI-facing portion of the trait.

Conceptually:

```text
Trait
 |
 +--> Name
 +--> Description
 +--> Icon
```

---

## Trait Tags

The sample contains:

```xml
<L n="tags">
    <E>TraitPersonality</E>
    <E>TraitGroup_Emotional</E>
</L>
```

So the Ambitious trait is tagged as both:

```text
TraitPersonality
TraitGroup_Emotional
```

:contentReference[oaicite:23]{index=23}

Tags allow other systems to categorize, filter, or recognize traits.

---

## CAS Integration

The trait contains:

```xml
<T n="cas_trait_asm_param">Ambitious</T>
```

and:

```xml
<V t="ui_trait_category_tag" n="ui_category" />
```

This shows that the trait has configuration connected to its Create-a-Sim presentation and categorization. :contentReference[oaicite:24]{index=24}

---

## Whim Set

The trait contains:

```xml
<V n="whim_set" t="enabled">
    <T n="enabled">
    34778
    <!-- ObjectivelessWhimSet: whimset_HasTraitAmbitious -->
    </T>
</V>
```

This connects the trait to:

```text
whimset_HasTraitAmbitious
```

:contentReference[oaicite:25]{index=25}

Conceptually:

```text
Ambitious Trait
      |
      v
Ambitious Whim Set
```

This shows that traits can participate in broader behavioral or goal-selection systems.

---

## How Traits Connect to Other Tuning

The Ambitious sample gives us a particularly useful connection map:

```text
Trait
 |
 +--> Buff
 |
 +--> Buff Replacements
 |
 +--> Conflicting Traits
 |
 +--> Ages
 |
 +--> Tags
 |
 +--> UI Strings
 |
 +--> Icon
 |
 +--> Whim Set
```

:contentReference[oaicite:26]{index=26}

Traits often sit at the center of many systems because other gameplay needs to know:

> "What kind of Sim is this?"

---

## Traits and Tests

Traits are commonly checked by tests.

Conceptually:

```text
Interaction
    |
    v
Trait Test
    |
    v
Does Sim have Ambitious?
    |
   Yes
    |
    v
Allow Special Behavior
```

This is one of the most common ways traits influence gameplay.

The trait itself exists on the Sim.

A test checks for it.

---

## Traits and Buffs

The verified sample gives us two different trait-to-buff relationships.

### Trait-Owned Buff

```text
Ambitious
   |
   v
Buff_Trait_Ambitious
```

### Buff Replacement

```text
Normal Career Buff
        |
        v
Ambitious-Specific Career Buff
```

:contentReference[oaicite:27]{index=27}

This demonstrates that traits can affect buffs in more than one way.

---

## Traits vs Buffs

| Trait | Buff |
|---|---|
| Usually represents a persistent characteristic | Usually represents a current state or effect |
| Can exist long-term | Often temporary |
| Can alter many systems | Often expresses one current condition |
| Can provide or replace buffs | Can be caused by traits |
| Commonly checked by tests | Commonly checked by tests |

A simplified mental model is:

```text
Trait
 = "What is generally true about this Sim?"

Buff
 = "What is happening to this Sim right now?"
```

There are exceptions, especially with hidden system traits and buffs, but this distinction is useful when starting out.

---

## Trait Conflicts

Conflicting traits are worth paying attention to when creating or modifying traits.

The Ambitious trait conflicts with:

```text
Lazy
Freegan
```

:contentReference[oaicite:28]{index=28}

If you create a custom trait or modify trait relationships, conflict tuning can determine whether certain combinations are allowed.

---

## Trait Eligibility

The Ambitious trait demonstrates that eligibility can be constrained by:

- age
- trait type
- conflicting traits
- species-related configuration
- other trait-specific conditions

Not every trait will use the same restrictions.

---

## Common Modding Uses

Trait tuning is important when you want to change:

- personality traits
- trait eligibility
- age restrictions
- conflicts
- trait-provided buffs
- trait-specific reactions
- UI names and descriptions
- icons
- tags
- related whim or behavior systems

If the question is:

> "Why does this Sim behave differently because they have this characteristic?"

the trait is often the correct place to start.

---

## Common Mistakes

### Assuming every trait is a CAS personality trait

Traits are used for many systems beyond CAS personality selection.

### Assuming the trait contains all of its behavior

It may reference buffs, whim sets, tests, interactions, or other resources.

### Ignoring buff replacements

Traits can change gameplay by replacing generic buffs with trait-specific versions.

### Ignoring conflicts

A trait may be incompatible with other traits.

### Confusing a trait with the buff it provides

The trait and its associated buff are separate tuning resources.

---

## FAQ

### Is a trait permanent?

Traits are generally more persistent than buffs, but they can still be added or removed by gameplay or mods.

### Can a trait give a Sim a buff?

Yes.

The Ambitious trait directly references `Buff_Trait_Ambitious`. :contentReference[oaicite:29]{index=29}

### Can traits replace buffs?

Yes.

The Ambitious trait contains a substantial `buff_replacements` map. :contentReference[oaicite:30]{index=30}

### Can traits conflict with one another?

Yes.

The Ambitious sample conflicts with Lazy and Freegan. :contentReference[oaicite:31]{index=31}

### Can traits be limited by age?

Yes.

This sample is configured for Young Adult, Adult, and Elder Sims. :contentReference[oaicite:32]{index=32}

### Can traits be checked by tests?

Yes.

Trait tests are a common way for other systems to determine whether a Sim has a particular trait or trait type.

### Are all traits visible?

No.

The game also uses hidden traits and system-specific markers.

---

## If You Remember Only 3 Things

1. **Traits represent persistent characteristics or gameplay markers on a Sim.**
2. **Traits often change behavior indirectly by connecting to buffs, tests, conflicts, and other systems.**
3. **When studying a trait, inspect both what the trait contains and what other tuning checks for that trait.**

---

## Related Reference Pages

- [Buffs](../buffs/README.md)
- [Tests](../tests/README.md)
- [Loot Actions](../loot/README.md)
- [Interactions](../interactions/README.md)
- [Statistics](../statistics/README.md)
- [Commodities](../commodities/README.md)

---

## Related Course Material

- [Module 01 — Tuning Foundations](../../../modules/01-tuning-foundations/README.md)
- [Module 05 — Buffs, Traits & Statistics](../../../modules/05-buffs-traits-statistics/README.md)
