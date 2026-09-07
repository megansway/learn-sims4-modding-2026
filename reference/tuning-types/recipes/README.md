# Recipe Tunings

> **Status:** ✓ Documented  
> **Last verified:** 2026-09-07  
> **Game version:** 1.127.41.1030  
> **Pack requirements:** Varies by recipe

---

## In Plain English

A **recipe tuning** defines something the game can create, craft, prepare, or produce and the rules that control how that process works.

Despite the name, recipes are not limited to food. In The Sims 4, recipe tuning can also be used for other crafting-style systems, including painting.

A useful way to think about a recipe is:

> **The interaction handles what the Sim does. The recipe defines what is being made and how that creation process is configured.**

---

## Mental Model

```text
Sim starts crafting
        |
        v
Recipe Tuning
        |
        +--> Requirements
        +--> Crafting phases
        +--> Cost
        +--> Skill
        +--> Final product
        +--> Quality
        +--> Value
        +--> Loot
        +--> Resume behavior
```

The recipe often sits near the center of several connected systems.

---

## Verified Example

The sample used here is:

```text
recipe_Painting_Abstract_Small
```

This is a painting recipe for a **Small Abstract Painting**.

Its top-level tuning begins with:

```xml
<I c="PaintingRecipe"
   i="recipe"
   m="crafting.painting"
   n="recipe_Painting_Abstract_Small"
   s="26875">
```

This tells us several important things immediately.

---

## Reading the Top-Level Tuning

### `c="PaintingRecipe"`

```xml
c="PaintingRecipe"
```

This identifies the tuning class as:

```text
PaintingRecipe
```

That tells us this is not just a generic recipe resource. It is a recipe class specifically used by the painting system.

> [!IMPORTANT]
> Recipe tunings can have different classes. The fact that something is a recipe does not mean every recipe will have the same fields or structure.

---

### `i="recipe"`

```xml
i="recipe"
```

This tells us the tuning type is:

```text
recipe
```

So in this example:

```text
Tuning type:
recipe

Tuning class:
PaintingRecipe
```

---

### `m="crafting.painting"`

```xml
m="crafting.painting"
```

This identifies the Python module associated with the tuning:

```text
crafting.painting
```

That gives useful context about which game system owns or interprets this recipe.

---

### `n="recipe_Painting_Abstract_Small"`

```xml
n="recipe_Painting_Abstract_Small"
```

This is the tuning name.

The name already gives us several clues:

```text
recipe
Painting
Abstract
Small
```

That makes names very useful when searching extracted tuning.

---

### `s="26875"`

```xml
s="26875"
```

This is the instance ID for the recipe tuning.

Other resources can reference this recipe using that ID.

---

## What This Recipe Represents

Later in the tuning, we find:

```xml
<T n="name">0x3D29E9EA<!--String: "Small Abstract Painting"--></T>
```

So this recipe represents:

```text
Small Abstract Painting
```

It also references:

```xml
<T n="painting_style">26645<!--PaintingStyle: PaintingStyle_Abstract--></T>
```

which connects the recipe to the abstract painting style.

This already shows an important principle:

> A recipe is not just a finished object. It can also define the category, style, process, and rules associated with producing that object.

---

## Crafting Phases

This recipe contains:

```xml
<L n="_first_phases">
```

and:

```xml
<L n="_phases">
```

These define stages in the crafting process.

The first phase is:

```text
START_PHASE
```

and that phase points to:

```xml
<T n="super_affordance">
13358
<!-- CraftingPhaseCreateObjectInSlotSuperInteraction: easel_CreateCanvas -->
</T>
```

Then the recipe moves to:

```text
2-Staging
```

which uses:

```xml
<T n="super_affordance">
38889
<!-- canvas_PaintPainting_Staging_Small -->
</T>
```

Conceptually:

```text
START_PHASE
    |
    v
Create Canvas
    |
    v
2-Staging
    |
    v
Painting Interaction
```

This shows that a recipe can control a **multi-phase crafting process** rather than simply saying "create this object."

---

## How Recipe Tunings Connect to Interactions

Recipe phases can reference **super affordances**, which are interaction tunings.

In this sample:

```text
Recipe
   |
   +--> easel_CreateCanvas
   |
   +--> canvas_PaintPainting_Staging_Small
```

So the recipe and interaction system work together.

A useful distinction is:

| Interaction | Recipe |
|---|---|
| Defines what the Sim does | Defines what is being produced and its crafting rules |
| Can run animations and behavior | Can define phases, costs, skills, quality, result data |
| May be reused by recipes | May reference crafting interactions |

The recipe is therefore not the same thing as the interaction.

---

## Crafting Cost

This recipe contains:

```xml
<V t="flat_fee" n="crafting_cost">
    <T n="flat_fee">50</T>
</V>
```

So the crafting cost is configured as a flat fee of:

```text
50 Simoleons
```

This verifies that recipe tuning can define the cost associated with crafting the item.

---

## Retail Price

The recipe also contains:

```xml
<T n="_retail_price">60</T>
```

This is separate from the crafting cost.

So conceptually:

```text
Crafting Cost
    !=
Retail Price
```

The recipe can therefore track both the cost to make something and a separate value associated with selling or retail behavior.

---

## Skill Requirements

The sample contains:

```xml
<V t="enabled" n="skill_test">
```

Inside it:

```xml
<T n="skill">
16708
<!-- statistic_Skill_AdultMajor_Painting -->
</T>
```

and:

```xml
<T n="lower_bound">4</T>
```

This means the recipe requires at least:

```text
Painting Skill Level 4
```

Conceptually:

```text
Recipe
   |
   v
Skill Test
   |
   v
Painting Skill >= 4
```

This is one example of how recipe availability can be gated by skill.

---

## Skill Progression

The recipe also contains:

```xml
<U n="skill_loot_data">
```

which references the same painting skill:

```xml
<T n="stat">
16708
<!-- statistic_Skill_AdultMajor_Painting -->
</T>
```

This tells us the recipe also participates in skill-related progression or reward behavior during crafting.

So the recipe does not merely check skill level. It can also define how the crafting process interacts with skill advancement.

---

## Additional Tests

The tuning contains:

```xml
<L n="additional_tests">
```

This sample includes a `sim_info` test.

That shows recipe tuning can contain additional eligibility conditions beyond the main skill test.

Conceptually:

```text
Recipe availability
      |
      +--> Skill Test
      |
      +--> Additional Tests
```

> [!NOTE]
> The exact meaning of each test depends on the configured test type. Different recipes may use completely different conditions.

---

## Final Product

One of the most important sections is:

```xml
<U n="final_product">
```

This controls properties of the crafted result.

Inside it, we find:

```xml
<T n="definition">15922</T>
```

This connects the recipe to the object definition used for the finished item.

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

This is the part of the recipe where the created object becomes concrete.

---

## Tags Applied to the Finished Object

The final product contains:

```xml
<L n="apply_tags">
```

with tags such as:

```text
Genre_Painting_Abstract
BuyCatLD_WallDecoration
Func_CraftedObject_Generic
```

So the recipe can apply tags to the crafted object.

These tags may affect categorization, filtering, gameplay systems, or how other tuning recognizes the object.

---

## Conditional States

The recipe contains:

```xml
<L n="conditional_apply_states">
```

One entry applies:

```text
Marketable_HigherValue
```

if the Sim has:

```text
trait_Marketable
```

Conceptually:

```text
Does Sim have Marketable trait?
        |
       Yes
        |
        v
Apply Higher Value State
```

This is a great example of recipes using **tests to conditionally modify the final product**.

---

## Final Product Loot

Inside `final_product`, the recipe contains a `loot_list`.

That list includes several loot resources, such as:

```text
loot_Crafting_Recipe_QualityModifiers
loot_CollegeOrganization_SecretSociety_Inspired_Quality
loot_Fear_Failure_Trigger_ProgressiveFailure_CraftedObject
loot_Trait_Overachiever_PoorQuality
loot_Child_Confidence_Loss_PoorQuality
loot_Burnout_DecreaseAll_FromCrafting
loot_SelfDiscovery_Skill_HighQuality
loot_SelfDiscovery_Creative_HighQuality
```

This gives us a direct, verified example of:

```text
Recipe
   |
   v
Final Product
   |
   v
Loot
```

This is exactly why understanding loot is useful when reading recipes.

The recipe may define the finished object, while loot modifies or reacts to that result.

---

## Masterpieces

This recipe supports masterwork behavior:

```xml
<V t="enabled" n="masterworks">
```

The base chance is:

```xml
<T n="base_chance">0.1</T>
```

which is:

```text
10%
```

The recipe also requires:

```text
Painting Skill >= 7
```

for the base masterwork test.

So conceptually:

```text
Painting Skill >= 7?
        |
       Yes
        |
        v
Base Masterpiece Chance = 10%
```

---

## Masterpiece Chance Modifiers

The recipe includes several multiplier tests that change the masterpiece chance.

Examples include:

```text
Painting Master trait     -> x1.4
Inspired mood             -> x1.25
Bored mood                -> x0.75
Creative Visionary trait  -> x1.2
Perfectionist trait       -> x1.2
Certain inspired buffs    -> x1.4
```

This shows that recipe outcomes can be influenced by:

- traits
- moods
- buffs
- skills

Conceptually:

```text
Base Masterpiece Chance
        |
        +--> Skill
        +--> Trait
        +--> Mood
        +--> Buff
        |
        v
Final Chance
```

This is a very good example of multiple tuning systems meeting inside a recipe.

---

## Quality

The recipe defines:

```xml
<U n="quality_adjustment">
```

with:

```text
base_quality = -23
skill_adjustment = 9
```

This shows that quality can be calculated using recipe-specific values and skill influence.

Conceptually:

```text
Base Quality
      +
Skill Adjustment
      |
      v
Crafted Object Quality
```

The exact quality calculation is handled by the game system, but the recipe provides values that influence it.

---

## Quality and Simoleon Value

The sample also maps object quality states to value multipliers.

Examples include:

```text
Quality_Normal
Quality_Outstanding
Quality_Poor
Marketable_HigherValue
```

These states affect the object's value range.

This connects:

```text
Quality
   |
   v
Object State
   |
   v
Simoleon Value
```

So the value of the crafted object can depend on the quality state produced by the crafting process.

---

## Skill-Based Value Scaling

The recipe contains:

```xml
<V t="enabled" n="simoleon_value_skill_curve">
```

This maps Painting skill levels to value multipliers.

The sample includes points such as:

```text
Skill 1  -> 1.83
Skill 4  -> 2.00
Skill 7  -> 3.72
Skill 10 -> 9.78
```

This shows that the final product's value can scale with the creator's skill.

Conceptually:

```text
Painting Skill
      |
      v
Value Curve
      |
      v
Finished Object Value
```

---

## Buffs and Recipe Weight

The recipe contains:

```xml
<L n="buff_weight_multipliers">
```

with:

```text
Buff_Trait_Lazy -> 5
```

This shows that buffs or trait-related buffs can influence recipe weighting.

This is especially relevant when the game is deciding between possible behaviors or autonomous choices.

> [!NOTE]
> The exact autonomy behavior should be understood in context with the surrounding crafting and autonomy systems, but the sample clearly shows a buff-based recipe weight multiplier.

---

## Autonomy

The recipe contains:

```xml
<T n="autonomy_weight">1</T>
```

This gives the recipe an autonomy weight.

So recipe tuning can participate in autonomy decisions rather than existing only for player-selected crafting.

---

## Recipe Categories

The sample contains:

```xml
<V t="enabled" n="base_recipe_category">
```

which points to:

```text
paintingCategory_Abstract
```

This helps organize the recipe into a menu or crafting category.

Conceptually:

```text
Painting Menu
     |
     v
Abstract Category
     |
     v
Small Abstract Painting
```

---

## Base Recipe

The recipe also references:

```xml
<V t="enabled" n="base_recipe">
```

which points to:

```text
recipe_Painting_Abstract
```

This shows that one recipe can derive or organize itself around another base recipe.

Conceptually:

```text
Base Abstract Painting Recipe
             |
             v
Small Abstract Painting Recipe
```

This is another example of recipe tuning being reference-based.

---

## Resume Interaction

The sample contains:

```xml
<V t="enabled" n="resume_affordance">
```

which points to:

```text
crafting_resume_Easel
```

This tells the game what interaction should be used when a Sim resumes an unfinished crafting process.

Conceptually:

```text
Unfinished Painting
       |
       v
Resume Affordance
       |
       v
crafting_resume_Easel
```

This is especially useful for multi-stage crafting systems.

---

## Recipe Difficulty

The recipe contains:

```xml
<E n="recipe_difficulty">1</E>
```

This gives the recipe a configured difficulty value.

Different crafting systems may use this field differently, but it is another example of recipe-specific metadata.

---

## Recipe Tags

The recipe contains:

```xml
<L n="recipe_tags">
```

with:

```text
Recipe_Plopsy_Browser
```

Tags can connect recipes to other game systems that search, filter, categorize, or recognize recipes.

---

## How Recipe Tunings Connect to Other Tuning

This sample gives us a particularly rich connection map:

```text
Recipe
 |
 +--> Crafting Interactions
 |
 +--> Skill
 |
 +--> Tests
 |
 +--> Base Recipe
 |
 +--> Pie Menu Category
 |
 +--> Buffs
 |
 +--> Traits
 |
 +--> Moods
 |
 +--> Final Product Definition
 |
 +--> Object States
 |
 +--> Loot
 |
 +--> Resume Interaction
 |
 +--> Tags
```

This is why recipes are such useful tuning resources to study.

They often sit in the middle of many systems at once.

---

## A More Complete Mental Model

```text
Player / Autonomy
       |
       v
Recipe Available?
       |
       +--> Skill Test
       +--> Additional Tests
       |
       v
Crafting Begins
       |
       v
Crafting Phases
       |
       +--> Super Interactions
       |
       v
Final Product
       |
       +--> Definition
       +--> Tags
       +--> States
       +--> Quality
       +--> Value
       +--> Loot
       |
       v
Finished Crafted Object
```

---

## Common Modding Uses

Recipe tuning is a major place to look when you want to change:

- crafting costs
- skill requirements
- recipe availability
- crafting phases
- result objects
- quality behavior
- masterpiece chances
- final object value
- recipe categories
- autonomy weighting
- crafting-related loot
- resume behavior
- tags
- conditional states

If the modding question is:

> "What determines how this craftable thing works?"

the recipe is often one of the first resources worth examining.

---

## Common Mistakes

### Assuming recipes are only for food

They are not.

This verified sample is a painting recipe.

### Assuming the recipe is the interaction

The recipe references crafting interactions. They are separate resources.

### Looking only for the result object

The recipe may also control cost, skill, phases, quality, value, loot, tags, and other behavior.

### Ignoring the final product section

A large amount of important behavior may live inside:

```text
final_product
```

### Ignoring tests

Recipe availability and result behavior can depend on skills, traits, buffs, moods, and other conditions.

### Ignoring loot

The finished product may trigger several additional gameplay effects through loot references.

---

## FAQ

### Are recipes only used for cooking?

No.

This sample proves that painting also uses recipe tuning.

### Is the recipe the same thing as the crafted object?

No.

The recipe defines the process and rules.

The final product section references the object definition used for the result.

### Is the recipe the same thing as the crafting interaction?

No.

The recipe can reference super interactions used during crafting.

### Can recipes have multiple phases?

Yes.

This sample contains a start phase and a staging phase.

### Can recipes require skills?

Yes.

This recipe requires Painting skill level 4.

### Can recipes influence quality?

Yes.

This sample includes quality adjustment, masterpiece logic, and skill-based effects.

### Can traits affect recipe results?

Yes.

This sample uses traits in conditional state application and masterpiece multipliers.

### Can moods affect crafting results?

Yes.

Inspired and Bored moods modify masterpiece chance in this sample.

### Can buffs affect recipe behavior?

Yes.

This recipe contains buff-based weight multipliers and buff tests for masterpiece modifiers.

### Can recipes run loot?

Yes.

This recipe includes a `loot_list` inside the final product configuration.

### Can recipes control value?

Yes.

This sample includes retail price, quality-based value modifiers, and a skill-based value curve.

---

## If You Remember Only 3 Things

1. **A recipe defines what is being crafted and the rules around producing it.**
2. **Recipes can connect to interactions, skills, tests, traits, buffs, loot, object definitions, quality, value, and more.**
3. **Do not read a recipe as one isolated file—follow its references to understand the full crafting system.**

---

## Related Reference Pages

- [Interactions](../interactions/README.md)
- [Loot Actions](../loot/README.md)
- [Tests](../tests/README.md)
- [Buffs](../buffs/README.md)
- [Traits](../traits/README.md)
- [Statistics](../statistics/README.md)
- [Commodities](../commodities/README.md)

---

## Related Course Material

- [Module 01 — Tuning Foundations](../../../modules/01-tuning-foundations/README.md)
- [Module 02 — Reading EA Tuning](../../../modules/02-reading-ea-tuning/README.md)
- [Module 06 — Recipes & Crafting](../../../modules/06-recipes-and-crafting/README.md)
