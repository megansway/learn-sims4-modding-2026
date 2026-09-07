# Module 07 — Debugging & Maintenance

Making a mod work once is only part of the job.

Mods can break because of tuning mistakes, conflicting overrides, missing references, game patches, changed resources, outdated assumptions, or problems elsewhere in the mod.

This module focuses on how to debug problems methodically and how to keep a mod maintainable over time.

A useful mental model is:

```text
Problem
   |
   v
Identify what changed
   |
   v
Narrow the cause
   |
   v
Test one hypothesis
   |
   v
Fix
   |
   v
Re-test
   |
   v
Maintain after future patches
```

The goal is not to eliminate every possible bug. The goal is to make debugging feel like a process instead of random guessing.

---

## What You Will Learn

By the end of this module, you should understand:

- how to approach a broken mod without changing everything at once
- how to separate mod problems from game problems
- how to test in a clean environment
- how mod conflicts happen
- what LastException files are useful for
- why tuning overrides can break after patches
- how to compare old and new EA tuning
- how to track compatibility
- how to maintain useful development notes
- how to decide whether a mod needs updating after a game patch

---

# Debugging Starts With a Question

Do not begin with:

> "My mod is broken."

That is too broad.

Try to describe the failure as specifically as possible.

For example:

> "The interaction no longer appears."

> "The interaction appears, but nothing happens when I click it."

> "The interaction works, but the target Sim gets the wrong buff."

> "The recipe works, but the cost did not change."

> "The mod worked before the latest patch and now it does not."

The more specific the symptom is, the easier it is to choose what to investigate.

---

# Expected vs Actual

Write down what you expected and what actually happened.

For example:

```text
Expected:
Interaction gives Actor a Confident buff.

Actual:
Interaction succeeds, but no buff appears.
```

That immediately narrows the problem.

The interaction itself probably runs.

So the next places to inspect may be:

```text
Outcome
Loot
Participant
Buff
```

rather than the entire interaction.

---

# Debug One Layer at a Time

Think of a mod as a chain.

For example:

```text
Interaction
    |
    v
Test
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

If the buff is missing, ask:

```text
Did the interaction run?
        |
        v
Did the test pass?
        |
        v
Did the expected outcome occur?
        |
        v
Did the loot run?
        |
        v
Was the buff added?
```

Find the first point where expected behavior stops.

That is usually where your investigation should focus.

---

# Do Not Change Five Things at Once

When a mod fails, it is tempting to start editing everything that might be related.

Avoid that.

If you change:

```text
Tests
Loot
Participants
Buff
Interaction
```

at the same time and the mod starts working, you still do not know what the actual problem was.

Instead:

```text
Hypothesis 1
   |
   v
Make one change
   |
   v
Test
```

Then repeat if necessary.

This is slower per edit but much faster overall.

---

# Test in a Clean Environment

Other mods can interfere with your results.

When debugging a strange problem, test with as little unrelated content installed as possible.

Conceptually:

```text
Mods Folder
 |
 +--> Your Test Mod
```

instead of:

```text
Mods Folder
 |
 +--> Hundreds of Unrelated Mods
```

If the problem disappears in a clean test environment, that strongly suggests a conflict or interaction with another mod.

---

# Keep a Dedicated Test Setup

A separate testing setup can save a lot of time.

For example:

```text
Test Environment
 |
 +--> Current Mod Build
 +--> Required Dependencies
 +--> Minimal Test Save
```

The goal is to reduce variables.

A test save with easy access to the relevant Sim, object, interaction, trait, or skill can make repeated testing much faster.

---

# Mod Conflicts

Two mods may conflict when they both override or alter the same resource.

Conceptually:

```text
EA Resource
   |
   +--> Mod A Override
   |
   +--> Mod B Override
```

Only one resulting version can effectively win for that resource.

This can produce behavior such as:

- one mod appearing not to work
- only part of a feature working
- unexpected values
- altered tests
- missing effects

---

# Shared Resources Matter

A conflict does not always mean two mods are trying to accomplish the same thing.

They may simply edit the same shared resource for different reasons.

For example:

```text
Interaction
 |
 +--> Mod A changes autonomy
 |
 +--> Mod B changes tests
```

If both replace the same interaction tuning, the changes may not combine automatically.

This is why override-based mods are more conflict-prone than changes that use separate custom resources.

---

# How to Investigate a Possible Conflict

Ask:

1. Does the mod work by itself?
2. Does it fail only when another mod is installed?
3. Do both mods modify the same tuning resource?
4. Is one mod overriding a shared snippet, interaction, loot, or other resource?
5. Can the changes be combined into one compatible version?

A conflict is not necessarily a bug in either mod.

Sometimes both mods work correctly on their own but cannot both replace the same resource independently.

---

# LastException Files

The Sims 4 can generate **LastException** files when an error occurs in the game's Python-driven simulation.

These files can provide clues about:

- which system failed
- which interaction or object was involved
- which Python module raised an exception
- whether a custom script mod appears in the error path
- what part of the game was running when the error occurred

A LastException is evidence, not always a complete diagnosis.

---

# What a LastException Does Not Mean

Seeing your mod mentioned in an error does not automatically prove:

> "My mod caused everything."

Likewise, not seeing your mod's name does not automatically prove it is unrelated.

Errors can propagate through connected systems.

Use the exception as a clue alongside:

- reproducible behavior
- clean testing
- tuning inspection
- conflict testing
- recent changes

---

# Tuning Errors vs Script Errors

A tuning mod and a Python script mod can fail differently.

A pure tuning problem may result in things like:

- missing interactions
- failed tuning loading
- incorrect behavior
- broken references
- invalid tests
- unexpected outcomes

A script mod may produce Python exceptions when code execution fails.

Some large mods contain both tuning and scripts, so the distinction may overlap.

---

# Broken References

Because Sims 4 tuning is highly reference-based, one incorrect reference can break behavior further down the chain.

For example:

```text
Interaction
    |
    v
Loot ID
    |
    v
Missing / Wrong Resource
```

or:

```text
Recipe
   |
   v
Final Product Definition
   |
   v
Wrong Object
```

When something suddenly stops at a particular stage, inspect the references leading into that stage.

---

# Invalid XML

Simple XML mistakes can also break tuning.

Common examples include:

- missing closing tags
- incorrect nesting
- accidentally deleting part of a structure
- malformed values
- editing the wrong field
- changing resource identity accidentally

This is one reason small edits are easier to debug than large rewrites.

---

# Compare Against the Original

When a tuning override breaks, compare your version to the current EA resource.

Ask:

```text
What did I change intentionally?

What changed unintentionally?

Did EA change this resource after a patch?
```

A simple side-by-side comparison can reveal problems quickly.

---

# Game Patches

The Sims 4 is updated regularly.

A patch may change:

- tuning structure
- interaction behavior
- IDs
- tests
- loot
- object definitions
- Python classes
- tunable fields
- pack systems
- underlying game logic

A mod that worked before the patch may stop working even if you did not change anything.

---

# Override Mods After a Patch

Overrides are especially important to re-check after patches because they replace EA resources.

Suppose EA changes:

```text
Interaction Version A
```

into:

```text
Interaction Version B
```

but your mod still contains an override based on Version A.

Then your package may effectively restore old tuning and accidentally remove EA's new changes.

Conceptually:

```text
New EA Tuning
      |
      X
Your Old Override
      |
      v
Old Behavior Restored
```

This is one of the most common maintenance risks with tuning overrides.

---

# Re-Extract Current Tuning

After a major patch, compare your mod against newly extracted current tuning.

Do not assume the previous EA resource is still identical.

A useful workflow is:

```text
Old EA Tuning
      |
      +--> Your Mod Changes

New EA Tuning
      |
      v
Compare
      |
      v
Reapply Only Your Intended Changes
```

This is much safer than blindly keeping the old full override.

---

# Think in Diffs

When maintaining an override, mentally separate:

```text
EA's tuning
```

from:

```text
Your intended change
```

For example:

```text
EA:
crafting_cost = 50

Your change:
crafting_cost = 25
```

Your real mod concept is:

```text
Change crafting_cost from EA value to 25
```

not:

```text
Keep this entire old XML file forever
```

That mindset makes patch updates much easier.

---

# Track What Your Mod Actually Changes

Keep a small development note for every mod.

For example:

```text
Mod:
Lower Painting Costs

Resources Modified:
recipe_Painting_Abstract_Small

Fields Changed:
crafting_cost

Original Value:
50

Custom Value:
25

Reason:
Lower crafting price
```

After a patch, this tells you exactly what needs to be checked.

---

# Track Resource IDs

For tuning overrides, keep:

```text
Resource Name
Instance ID
Resource Type
```

For example:

```text
recipe_Painting_Abstract_Small
26875
recipe
```

This makes it easier to locate the current version after a patch.

---

# Compatibility Information

For released mods, track the game version you last tested against.

For example:

```text
Last Tested:
Game Version 1.x.x.x
```

This does not guarantee future compatibility.

It tells users:

> "This is the version I actually checked."

That is much more useful than simply saying:

> "Should work."

---

# "Compatible" vs "Needs Update"

A game patch does not automatically mean every mod is broken.

After a patch, a mod may be:

```text
Still Compatible
```

```text
Needs Re-testing
```

```text
Needs Update
```

These are different states.

Do not declare a mod broken simply because a patch happened.

Test it.

---

# A Good Patch-Day Workflow

When the game updates:

```text
1. Record the new game version.
2. Read patch notes for relevant systems.
3. Re-extract current tuning.
4. Identify resources your mod overrides.
5. Compare old EA tuning to new EA tuning.
6. Reapply your intended changes if necessary.
7. Test in-game.
8. Update compatibility notes.
```

You do not need to inspect the entire game.

Focus on the systems your mod actually touches.

---

# Patch Notes Are Clues

If a patch says:

> Updated romantic interactions.

and your mod changes romantic interactions, investigate.

If the patch says:

> Fixed visual issue with a specific CAS item.

and your mod only changes painting recipe costs, that may be much less relevant.

Patch notes help prioritize what to inspect.

They do not replace checking the actual tuning.

---

# When EA Adds New Fields

Suppose your old override contains:

```text
A
B
C
```

After a patch, EA changes the resource to:

```text
A
B
C
D
```

If your old override replaces the entire resource, field `D` may disappear from the effective game tuning.

That can break new functionality even though your original modification only cared about `B`.

This is why overrides deserve careful maintenance.

---

# When EA Changes Existing Values

EA may also adjust a value that your mod intentionally changes.

For example:

```text
Old EA:
50

Your Mod:
25

New EA:
40
```

Now you need to decide:

> Do I still want my value to be 25?

Maybe yes.

Maybe your intended change was actually:

> Reduce the EA price by half.

If so, perhaps your new value should be 20.

Maintenance sometimes requires revisiting the design intent, not just preserving old numbers.

---

# Keep Versions of Your Own Mod

Do not overwrite your only copy while updating.

A simple structure might be:

```text
mod/
 |
 +--> releases/
 |     |
 |     +--> v1.0
 |     +--> v1.1
 |
 +--> development/
```

Git can make this much easier because every committed version can be preserved in history.

---

# Why GitHub Helps

Your repository can track:

- documentation changes
- mod source files
- compatibility notes
- bug fixes
- version history
- release notes

Instead of remembering:

> "What did I change three patches ago?"

you can inspect the history.

That is one of the biggest advantages of learning version control alongside modding.

---

# Use Meaningful Commits

Instead of:

```text
changes
```

use:

```text
Update painting recipe for game patch
```

or:

```text
Fix buff participant targeting
```

or:

```text
Adjust skill requirement
```

A commit message should help future-you understand why the change happened.

---

# Keep Development and Releases Separate

Your working version may contain unfinished changes.

Your public release should represent a tested state.

Conceptually:

```text
Development
    |
    v
Testing
    |
    v
Stable Release
```

Do not assume every saved edit is release-ready.

---

# Reproduce the Bug

Before fixing something, make sure you can reproduce it.

A good bug description includes:

```text
Starting State:
What you did:
What you expected:
What happened:
Does it happen every time?
```

For example:

```text
Starting State:
Young Adult Sim with Painting level 5.

Action:
Select Small Abstract Painting.

Expected:
Recipe costs 25 Simoleons.

Actual:
Recipe costs 50 Simoleons.

Reproducible:
Yes.
```

That is much easier to debug than:

> "Painting mod doesn't work."

---

# Reduce the Test Case

If a complex mod breaks, isolate the smallest failing part.

For example:

```text
Full Mod
 |
 +--> Trait
 +--> Interaction
 +--> Loot
 +--> Buff
 +--> Statistic
```

If the problem is the buff not being applied, test:

```text
Interaction
   |
   v
Loot
   |
   v
Buff
```

without worrying about unrelated features.

This is called reducing the problem.

---

# Ask What Changed

If something used to work, ask:

```text
What changed since the last working version?
```

Possible answers include:

- your tuning
- another installed mod
- the game version
- a dependency
- the save state
- a resource reference
- a tool or packaging step

That gives you a short list of likely causes.

---

# Common Debugging Mistakes

## Randomly Changing Values

Make a hypothesis first.

---

## Testing With Too Many Mods Installed

Reduce variables.

---

## Assuming Every Error Is a Conflict

Your own tuning may simply be incorrect.

---

## Assuming Every Error Is Your Mod

The base game or another mod may be involved.

---

## Ignoring Shared Overrides

The same resource may be used by multiple systems.

---

## Keeping Old EA Tuning Forever

Re-check overrides after updates.

---

## Updating Without Testing

A successful package build does not prove gameplay behavior is correct.

---

## Forgetting the Game Version

Always know what version you tested.

---

# A Useful Debugging Checklist

When something breaks, ask:

```text
1. What exactly is wrong?
2. What did I expect?
3. Can I reproduce it?
4. Does it happen with only my mod installed?
5. What is the first point in the tuning chain that behaves incorrectly?
6. What resources are involved?
7. Did any IDs or references change?
8. Did the game recently update?
9. Does another mod override the same resource?
10. What is the smallest change I can test next?
```

---

# A Useful Maintenance Checklist

After a patch:

```text
1. Record the game version.
2. Check whether relevant game systems changed.
3. Re-extract current EA tuning.
4. Compare overridden resources.
5. Preserve new EA changes.
6. Reapply your intended modifications.
7. Test the mod in-game.
8. Test for conflicts if necessary.
9. Update documentation.
10. Release only after testing.
```

---

# If You Remember Only 5 Things

1. **Debug the first point where expected behavior stops.**
2. **Change one thing at a time and test again.**
3. **Always consider conflicts when multiple mods modify the same resource.**
4. **Re-check tuning overrides after game patches because EA may have changed the original resource.**
5. **Keep notes about exactly what your mod changes, which resources it touches, and which game version you tested.**

---

# Final Practice

Take one small mod you have already made and create a maintenance record:

```text
Mod Name:

Game Version Tested:

Resources Modified:

Instance IDs:

Fields Changed:

Expected Behavior:

Known Conflicts:

Last Test Result:
```

Then imagine the game patched tomorrow.

Could you quickly tell which EA resources need to be checked?

If yes, your mod is already much easier to maintain.

---

## Reference Library

For deeper explanations of specific tuning resources you encounter while debugging, use:

[The Tuning Type Reference](../../reference/tuning-types/README.md)

---

# Where to Go From Here

You have now completed the core course.

You should have a working foundation for:

- reading tuning
- following references
- identifying conditions and effects
- making simple tuning edits
- understanding interactions
- understanding Sim-state systems
- investigating crafting
- debugging and maintaining mods

From here, you can either start building mods or use the reference library to study specific systems in more depth.

The reference library is designed to grow alongside your experience.

---

# Course Complete

Return to:

[Course Map](../../COURSE-MAP.md)

or explore:

[Tuning Type Reference](../../reference/tuning-types/README.md)
