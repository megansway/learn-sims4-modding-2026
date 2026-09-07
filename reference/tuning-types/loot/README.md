# Loot Actions

> **Status:** ✓ Up-to-date   
> **Last verified:** 2026-09-07  
> **Game version:** 1.127.41.1030  
> **Pack requirements:** Varies by loot tuning

---

## In Plain English

A **loot action** tells the game to change something.

Loot is commonly used after an interaction, test, event, buff, trait trigger, situation, aspiration, or other gameplay system reaches a point where the game needs to apply an effect.

A useful way to remember it is:

> **Tests decide whether something should happen. Loot decides what happens afterward.**

---

## Mental Model

```text
Something happens
        |
        v
Conditions are checked
        |
        v
Loot runs
        |
        v
Game state changes
```

If a Sim gains a buff, loses money, gets a trait, changes relationship value, has a statistic modified, or triggers another gameplay effect, loot may be involved somewhere in that chain.

---

## What Does Loot Actually Do?

Loot can perform many kinds of gameplay changes.

Depending on the loot tuning and action types involved, it may:

- add or remove buffs
- add or remove traits
- modify statistics
- change commodities or motives
- adjust relationships
- give or remove Simoleons
- affect objects
- trigger notifications or other gameplay effects
- run additional loot
- modify hidden gameplay state

The exact behavior depends on the actions configured inside the loot.

> [!IMPORTANT]
> Loot is not one single effect type. It is a system for applying one or more gameplay effects.

---

## Where Will I See Loot?

Loot appears throughout Sims 4 tuning.

You may encounter it while working with:

- interactions
- buffs
- traits
- aspirations
- careers
- situations
- events
- snippets
- objects
- crafting systems
- relationship systems
- autonomy-related systems
- rewards

This is why loot is one of the most useful systems to understand early.

---

## How Loot Connects to Other Tuning

Loot often sits at the point where a gameplay event turns into an actual consequence.

For example:

```text
Interaction
    |
    v
Tests
    |
   Pass
    |
    v
Loot
    |
    +--> Buff
    +--> Statistic Change
    +--> Relationship Change
```

Or:

```text
Trait
   |
   v
Trigger
   |
   v
Loot
   |
   v
Add Buff
```

The thing that starts the behavior and the thing that applies the effect may be separate resources.

---

## What Commonly References Loot?

Loot may be referenced by systems such as:

- interactions
- snippets
- buffs
- traits
- situations
- situation goals
- aspirations
- careers
- events
- rewards
- object tuning
- other loot

The exact relationships vary by system.

---

## What Can Loot Reference?

Loot may point to or act on resources such as:

- buffs
- traits
- statistics
- commodities
- relationships
- objects
- Sims
- households
- participants
- other loot
- other gameplay resources

A loot action may not always directly contain the entire behavior. It may itself reference another tuning resource.

---

## Participants Matter

Many loot actions need to know **who or what should receive the effect**.

That means you may see participant-related fields that determine the target.

Conceptually:

```text
Loot runs
   |
   +--> Actor
   +--> Target Sim
   +--> Target Object
   +--> Household
   +--> Other participant
```

For example, the same loot action could behave very differently depending on whether it applies to:

- the Sim performing the interaction
- the Sim being interacted with
- an object
- every Sim in a household
- another participant resolved by the tuning

> [!NOTE]
> When loot seems to be affecting the "wrong" Sim or object, participant configuration is one of the first things to inspect.

---

## Tests Inside or Around Loot

Loot may be controlled by tests.

A simplified flow might look like:

```text
Run test
   |
   +--> Fail -> Do nothing
   |
   +--> Pass -> Run loot
```

Some loot structures may also contain conditional behavior internally.

The important distinction is still:

- tests evaluate conditions
- loot applies effects

---

## Loot Can Contain Multiple Actions

A single loot tuning may apply more than one effect.

Conceptually:

```text
Loot
 |
 +--> Add Buff
 |
 +--> Increase Statistic
 |
 +--> Give Money
 |
 +--> Run More Loot
```

That means one loot reference can sometimes be responsible for several gameplay changes at once.

This is useful when reverse-engineering behavior because the first effect you notice may not be the only thing the loot is doing.

---

## Chained Loot

Loot can sometimes trigger additional loot.

For example:

```text
Interaction
    |
    v
Loot A
    |
    +--> Add Buff
    |
    +--> Run Loot B
             |
             +--> Change Statistic
             +--> Modify Relationship
```

This can make behavior look more complicated than it first appears because the final effect may be several references away.

> [!TIP]
> If a loot tuning does not explain the full behavior you are seeing in-game, check whether it references another loot resource.

---

## What Does Loot Look Like in Tuning?

A simplified conceptual example might look like this:

```xml
<I c="ExampleLoot"
   i="action"
   m="example.module"
   n="example_loot_name"
   s="123456789">

    <L n="loot_actions">
        <V t="buff">
            <U n="buff">
                <T n="buff_type">987654321</T>
            </U>
        </V>
    </L>

</I>
```

This is only an illustrative example.

The important ideas are:

- the tuning defines loot behavior
- it contains one or more actions
- those actions may reference other tuning resources
- the action type determines what kind of effect occurs

---

## Important Things to Look For

When reading loot, ask:

1. What triggers this loot?
2. What actions does it contain?
3. Who or what is the target?
4. Does it have tests or conditions?
5. What resources does it reference?
6. Does it trigger more loot?
7. Are multiple effects happening?
8. What tuning references this loot?

These questions help you trace the full gameplay effect.

---

## Following a Loot Reference

Imagine you are trying to figure out why an interaction gives a Sim a buff.

You find:

```text
Interaction
    |
    v
Loot ID: 123456789
```

You open that loot and find:

```text
Loot
    |
    v
Buff ID: 987654321
```

Then:

```text
Buff
    |
    v
Mood / State Change
```

So the full chain is:

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
Gameplay Effect
```

The interaction may never directly define the buff itself.

---

## Loot vs Tests

These two concepts work together constantly.

| Loot | Tests |
|---|---|
| Applies gameplay effects | Checks conditions |
| Changes game state | Determines whether something is valid |
| "What happens?" | "Should this happen?" |
| May run after tests pass | May prevent loot from running |

Example:

```text
Does Sim have Skill Level 5?
        |
       Yes
        |
        v
Run Loot
        |
        v
Give Reward Buff
```

The skill check is the test.

Giving the buff is the loot.

---

## Loot vs Snippets

Loot and snippets are also easy to confuse because snippets may contain or reference loot.

| Loot | Snippet |
|---|---|
| Applies gameplay effects | Stores reusable tuning data |
| Usually changes game state | Can contain many kinds of configuration |
| May be referenced by snippets | May reference loot |
| Focused on consequences | Broad reusable tuning category |

A snippet may organize or reuse loot, but it is not automatically loot itself.

---

## Common Modding Uses

Modders may edit or create loot when they want to:

- add a buff after an interaction
- remove an unwanted effect
- change a reward
- modify relationship gains or losses
- alter statistics
- give or remove money
- add or remove traits
- trigger additional gameplay effects
- change the outcome of an interaction or event

Loot is often the correct place to look when the modding goal is:

> "When this happens, I want something different to happen afterward."

---

## Common Mistakes

### Looking only at the interaction

The effect may be defined in loot rather than directly inside the interaction.

### Ignoring participants

The loot may be correct, but it may target a different Sim or object than you expect.

### Assuming one loot equals one effect

A single loot tuning may contain multiple actions.

### Stopping after the first loot reference

Loot can sometimes trigger more loot, so the effect chain may continue.

### Changing shared loot without checking where else it is used

If multiple systems reference the same loot, modifying it may affect all of them.

> [!WARNING]
> Shared loot can have wider effects than expected. Always check what references the loot before assuming a change is isolated.

---

## FAQ

### Does loot run by itself?

Usually, something else triggers it. That could be an interaction, trait, buff, situation, event, snippet, or another gameplay system.

### Is loot only used for rewards?

No. Loot can apply positive, negative, neutral, visible, or hidden gameplay changes.

### Can loot remove things as well as add them?

Yes. Depending on the action type, loot may add, remove, increase, decrease, or otherwise modify gameplay state.

### Can one loot apply several effects?

Yes. A loot tuning may contain multiple actions.

### Can loot trigger other loot?

Yes, depending on the tuning structure involved.

### Why can't I find the buff inside the interaction?

Because the interaction may reference loot, and the loot may be what applies the buff.

### Why is the loot affecting the wrong Sim?

Check participant configuration. The action may be targeting the actor, target, household, object, or another participant.

### Is loot always visible to the player?

No. Loot can modify hidden statistics, traits, flags, or other internal state.

---

## Advanced Notes

Loot is best understood as part of a larger **action-and-consequence system** rather than as one specific resource with one fixed behavior.

Different action types can perform very different operations, and some systems may expose loot through reusable structures, snippets, outcome tuning, or other wrappers.

Because of that, deeper documentation may eventually separate specific loot action families into their own reference pages.

For example, future pages could cover areas such as:

- buff operations
- trait operations
- statistic operations
- relationship operations
- money operations
- object operations
- participant targeting
- chained loot
- conditional loot

That would make it easier to move from:

> "What is loot?"

to:

> "What exactly does this particular loot action type do?"

---

## If You Remember Only 3 Things

1. **Loot applies gameplay effects and changes game state.**
2. **Tests decide whether something should happen; loot decides what happens afterward.**
3. **If the effect you are looking for is missing from the resource you opened, follow the loot references.**

---

## Related Reference Pages

- [Snippet Tunings](../snippets/README.md)
- [Tests](../tests/README.md)
- [Buffs](../buffs/README.md)
- [Traits](../traits/README.md)
- [Statistics](../statistics/README.md)
- [Interactions](../interactions/README.md)

---

## Related Course Material

- [Module 01 — Tuning Foundations](../../../modules/01-tuning-foundations/README.md)
- [Module 02 — Reading EA Tuning](../../../modules/02-reading-ea-tuning/README.md)
