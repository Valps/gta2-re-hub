## Contribute to decompiling process

We are decompiling the version 10.5 of GTA2 (also known as Freeloader version). This is a matching decomp, so the main goal is to replicate the original binary as faithfully as possible.

On this project we use the following nomenclatures for GTA2:

* `standalone` version - It is the executable obtaining from building the source code.
* `patched` version - It is the original executable patched with matched functions.

For decompiling functions you may need:

* IDA 9.1
* An IDE with `clang` formatting or a formatter
* Python >= 3.7

### Get acquainted with the project

* [Markers](general/markers.md) - How we differ between stubbed functions and matched ones.
* [Repository folders](general/repo_folders.md) - Get acquainted with the project folders.
* [Variable types](general/types.md) - See what typedefs we use on this project.
* [Declaring globals](general/globals.md) - How we define globals on GTA2 and how they work.
* [Patching](general/patcher.md) - Patching our functions into original GTA2.

## Very important classes

Some classes are essential since they are used almost everywhere on GTA2:

* [Fix16](classes/fix16.md) - This is how the game avoids using floating numbers (except when really needed them).
* [Ang16](classes/ang16.md) - Another "Fix16" but for angles and rotations. They are fixed point but with less precision.

## Let's start!

There are mainly two ways to contribute for this project:

* [Matching functions](general/matching_funcs.md) - How to match new functions and include them into the repository.
* [Renaming functions or attributes](general/renaming.md) - There are many unknown attributes for many classes and way many functions without a proper name. This section also explains how to fiddle with the patched version to find what they do.
