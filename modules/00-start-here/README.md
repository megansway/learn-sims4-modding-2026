# Module 00 — Start Here

Welcome to the course.

This module is for complete beginners. You do not need prior programming experience to start learning Sims 4 modding.

The goal here is to understand the basic vocabulary, tools, and file types you will see throughout the rest of the repository.

---

## What is a Sims 4 mod?

A mod is a file or group of files that changes, adds, or extends how The Sims 4 behaves.

Mods can do things like:

- change existing gameplay
- add new interactions
- add custom traits or buffs
- add recipes or crafting content
- change autonomy
- alter tuning values
- add new systems through Python scripting
- replace or extend existing game resources

Some mods are very small and change only one value.

Others contain many interconnected files and systems.

---

## Package Mods vs Script Mods

You will commonly see two major categories of Sims 4 mods.

### Package Mods

Package mods usually contain game resources such as:

- tuning
- XML-related resources
- object data
- textures
- strings
- icons
- meshes
- other game assets

They usually use the `.package` file extension.

A tuning mod that changes an existing interaction is commonly distributed as a package file.

### Script Mods

Script mods contain Python code.

They usually use the `.ts4script` file extension.

Script mods are used when a creator needs behavior that cannot be achieved with tuning alone.

For example:

```text
Tuning
    |
    v
Configure existing game systems

Python
    |
    v
Create or control more advanced custom behavior
