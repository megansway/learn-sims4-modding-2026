# Module 03 — Your First Tuning Mod

This module walks through the basic workflow of making a small Sims 4 tuning mod.

The goal is not to build something complicated. The goal is to learn the process:

```text
Find the tuning
      |
      v
Understand the relevant field
      |
      v
Make one small change
      |
      v
Package the mod
      |
      v
Test it in-game
      |
      v
Fix anything that went wrong
```

Once you understand this loop, larger tuning mods become much easier to approach.

---

## What You Will Learn

By the end of this module, you should understand how to:

- choose a realistic modding goal (via your new understanding)
- find the relevant EA tuning
- identify the field you actually need to change
- avoid changing unrelated tuning
- make a small XML edit
- package the edited resource
- install the mod for testing
- test whether the change worked
- recognize when the wrong resource was edited
- revise the mod without starting over

---

# Start Small

For your first tuning mod, choose something with a clear and limited outcome.

Good beginner goals are things like:

- changing a numeric value
- changing a duration
- changing a cost
- changing a skill requirement
- changing an autonomy weight
- changing a mood weight
- adjusting a simple test threshold

Avoid starting with something that requires several new custom resources, complicated interactions, or Python scripting.

The best first mod teaches you the workflow without forcing you to learn five systems at once.

---

# Step 1 — Define the Exact Change

Before opening any tuning, write the goal in one sentence.

For example:

> Change the crafting cost of a specific recipe.

or:

> Lower the skill requirement for a specific interaction.

or:

> Make a buff last longer.

This sounds simple, but it matters.

A vague goal like:

> Make painting better.

is much harder to investigate than:

> Reduce the cost of Small Abstract Paintings from 50 Simoleons to 25.

Specific questions lead to specific tuning.

---

# Step 2 — Find the Starting Resource

Use the behavior you want to change to decide where to begin.

For example:

```text
Want to change a recipe cost?
        |
        v
Start with the recipe tuning
```

```text
Want to change a mood effect?
        |
        v
Start with the buff tuning
```

```text
Want to change whether an interaction is available?
        |
        v
Start with the interaction or its tests
```

```text
Want to change what happens after an interaction?
        |
        v
Look for loot or outcome tuning
```

If you are not sure what type of tuning you need, use the reference library.

---

# Step 3 — Read Before Editing

Once you find the resource, do not immediately change values.

First identify:

```text
Class
Type
Name
Instance ID
```

Then find the specific section related to your goal.

For example, if your goal is:

> Change the crafting cost.

then a field like:

```xml
<V t="flat_fee" n="crafting_cost">
    <T n="flat_fee">50</T>
</V>
```

is relevant.

Fields related to:

- masterpiece chance
- skill gain
- tags
- loot
- final-product quality

are probably not relevant to that particular change.

---

# Step 4 — Change One Thing

For a first mod, change as little as possible.

Example:

Original:

```xml
<V t="flat_fee" n="crafting_cost">
    <T n="flat_fee">50</T>
</V>
```

Edited:

```xml
<V t="flat_fee" n="crafting_cost">
    <T n="flat_fee">25</T>
</V>
```

That is a good beginner edit because:

- the goal is clear
- the value is easy to test
- the expected result is obvious
- there are few unrelated variables

> [!TIP]
> The smaller the edit, the easier it is to figure out what went wrong if the mod does not work.

---

# Step 5 — Keep the Original Resource Identity

When making a tuning override, the edited resource needs to retain the identity of the EA resource you are overriding.

That includes the resource information used by the game to identify it.

Conceptually:

```text
EA Resource
Instance ID: 123456

Your Override
Instance ID: 123456
```

The game can then use your version in place of the original.

> [!IMPORTANT]
> Do not randomly change the instance ID of an override. If the resource identity changes, the game may treat it as a different resource instead of replacing the original.

---

# Overrides vs New Resources

This distinction matters.

## Override

An override changes an existing EA resource.

Conceptually:

```text
EA Resource
    |
    v
Your Modified Version
```

The mod keeps the same resource identity so the game uses the altered version.

---

## New Resource

A new custom resource has its own unique identity.

Conceptually:

```text
EA Resource

Custom Resource
```

Both exist separately.

For your first tuning mod, an override is usually easier because you are changing something that already exists.

---

# Step 6 — Package the Resource

Once the tuning is edited, it needs to be placed into a `.package` file that The Sims 4 can load.

The exact steps depend on the tool you are using, but the basic idea is:

```text
Edited Tuning
      |
      v
.package File
      |
      v
Mods Folder
      |
      v
The Sims 4
```

The package is the container the game reads.

---

# Step 7 — Name the Mod Clearly

Use a filename that tells you what the package changes.

For example:

```text
MyName_LowerAbstractPaintingCost.package
```

is much more useful than:

```text
test1.package
```

Clear filenames make debugging much easier later.

A useful pattern is:

```text
CreatorName_Feature_Change.package
```

Examples:

```text
Creator_LowerPaintingCost.package
Creator_LongConfidentBuff.package
Creator_EasierSkillRequirement.package
```

---

# Step 8 — Put the Mod in the Mods Folder

Place the finished package in your Sims 4 Mods folder.

A simple structure might be:

```text
Mods/
 |
 +--> YourModName.package
```

For larger projects, folders can help keep things organized.

For example:

```text
Mods/
 |
 +--> YourName/
      |
      +--> YourModName.package
```

---

# Step 9 — Test One Expected Result

Before launching the game, write down what you expect to happen.

For example:

```text
Expected result:
Small Abstract Painting costs 25 Simoleons instead of 50.
```

Then test exactly that.

Do not test ten unrelated things at once.

You want a clear answer to:

> Did my change work?

---

# Step 10 — Compare Expected vs Actual

After testing, one of several things may happen.

## It Works

Great.

Now you know:

```text
Resource found correctly
        |
        v
Field identified correctly
        |
        v
Package loaded correctly
        |
        v
Change worked
```

---

## Nothing Changed

Possible causes include:

- wrong resource
- wrong field
- package not installed correctly
- mod not loading
- another resource controls the behavior
- another mod is overriding the same tuning
- the value is used differently than expected

Do not immediately assume the XML syntax is wrong.

---

## The Game Changed, But Not the Way You Expected

That usually means you found something relevant, but not the whole system.

For example:

```text
You changed Recipe A
        |
        v
Gameplay also depends on Recipe B
```

or:

```text
You changed the interaction
        |
        v
The actual effect lives in loot
```

This is where reference tracing becomes useful.

---

# Debugging by Asking Better Questions

If the mod does not work, narrow the problem down.

Instead of:

> Why is my mod broken?

ask:

> Did the game load my package?

then:

> Did I override the correct resource?

then:

> Is this field actually responsible for the behavior?

then:

> Does another resource override or modify the result?

Small questions are easier to solve.

---

# Keep a Change Log While Testing

For even a tiny mod, keep a note like:

```text
Goal:
Lower Small Abstract Painting cost.

Resource:
recipe_Painting_Abstract_Small

Original:
crafting_cost = 50

Changed:
crafting_cost = 25

Result:
Worked / Did not work
```

This prevents you from forgetting what you changed after several tests.

---

# Change One Variable at a Time

If you edit:

```text
Cost
Skill Requirement
Autonomy
Quality
Loot
```

all at once and something breaks, you have no idea which edit caused it.

Instead:

```text
Change Cost
    |
    v
Test

Change Skill Requirement
    |
    v
Test
```

This is slower in the moment but much faster when debugging.

---

# Backup the Original Tuning

Keep a clean copy of the original EA tuning somewhere outside your edited package.

For example:

```text
original/
    recipe_Painting_Abstract_Small.xml

edited/
    recipe_Painting_Abstract_Small.xml
```

That gives you an easy comparison point.

---

# Compare Before and After

When debugging, compare:

```text
Original
vs
Edited
```

Ask:

- What changed?
- Did anything change accidentally?
- Did indentation or structure get damaged?
- Did I remove a closing tag?
- Did I edit the correct value?
- Did I alter the resource identity?

The fewer changes you made, the easier this comparison becomes.

---

# Common Beginner Mistakes

## Editing Too Much at Once

Keep the first change small.

---

## Starting With a Huge Mod Idea

A complex mod may involve:

- interactions
- buffs
- traits
- loot
- snippets
- statistics
- strings
- objects
- Python

That is a lot to debug at once.

Learn the workflow first.

---

## Editing the Wrong Resource

A resource name may look correct but only be one part of the system.

Follow references when necessary.

---

## Forgetting What You Changed

Keep notes.

---

## Changing IDs Accidentally

Resource identity matters for overrides.

---

## Testing With Other Mods Installed

Other mods may change the same tuning.

If something behaves strangely, test in a clean environment when possible.

---

## Assuming the Mod Is Broken Because Nothing Changed

Sometimes the package is fine and the tuning simply was not responsible for the behavior you thought it was.

That is a research problem, not necessarily a packaging problem.

---

# A Simple First-Mod Exercise

Choose one EA tuning resource with a numeric value that produces an obvious result.

Good examples include:

```text
Crafting cost
Duration
Skill threshold
Mood weight
Autonomy weight
```

Then:

1. Write down the original value.
2. Change only that value.
3. Package the resource.
4. Install the mod.
5. Test the expected result.
6. Record what happened.
7. Restore or revise the value if needed.

The purpose is not to make an amazing mod.

The purpose is to complete the entire workflow once.

---

# Example Workflow

Suppose the original tuning contains:

```xml
<T n="mood_weight">1</T>
```

Your goal is:

> Increase the mood strength.

You change it to:

```xml
<T n="mood_weight">2</T>
```

Then:

```text
Edit
 |
 v
Package
 |
 v
Install
 |
 v
Launch Game
 |
 v
Trigger Buff
 |
 v
Observe Mood Strength
```

If the result matches your expectation, you have successfully completed a tuning override workflow.

---

# When to Follow References

You should follow references when the field you need is not in the starting resource.

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

If your goal is:

> Change the mood caused by the interaction.

the actual edit may belong in the buff, not the interaction.

Do not force the change into the first resource you find.

---

# When to Stop

Stop investigating when you understand enough to make the intended change safely.

You do not need to reverse-engineer the entire game system before changing one value.

A good stopping point is:

```text
I know:
- which resource controls the behavior
- which field controls the value
- what result I expect
```

That is enough to test.

---

# Modding Is Iterative

Your first attempt may not work.

That is normal.

The workflow is:

```text
Question
   |
   v
Hypothesis
   |
   v
Edit
   |
   v
Test
   |
   v
Observe
   |
   v
Revise
```

Each failed test should teach you something about the system.

---

# If You Remember Only 5 Things

1. **Start with one small, specific change.**
2. **Read the tuning before editing it.**
3. **Change one variable at a time.**
4. **Test one expected result at a time.**
5. **If the result is wrong, investigate the resource chain instead of randomly changing more values.**

---

# Practice Before Moving On

Complete one tiny tuning override.

Record:

```text
Goal:
Resource:
Original Value:
Edited Value:
Expected Result:
Actual Result:
```

If the result does not work, write down your next question before changing anything else.

That habit will save you a lot of time later.

---

## Reference Library

If you encounter an unfamiliar resource while making your first mod, use:

[The Tuning Type Reference](../../reference/tuning-types/README.md)

---

# Next Module

Continue to:

[Module 04 — Interactions](../04-interactions/README.md)
