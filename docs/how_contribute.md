## Contribute to decompiling process

We are decompiling the version 10.5 of GTA2 (also known as Freeloader version). This is a matching decomp, so the main goal is to replicate the original binary as faithfully as possible.

On this project we use the following nomenclatures for GTA2:

* `standalone` version - It is the executable obtaining from building the source code.
* `patched` version - It is the original executable patched with matched functions.
* `10.5` - The version of GTA2 we're decompiling.
* `9.6f` - The version of GTA2 released by Rockstar on Rockstar Classics in 2002-2003.
* `9.6` - Without the `f`, it's the GTA2 retail version released on `1999`.

For the decompiling process you may need:

* IDA 9.1
* An IDE with `clang` formatting or a text formatter
* Python >= 3.7

### Get acquainted with the project

* [Markers](general/markers.md) - How we differ between stubbed functions and matched ones.
* [Repository folders](general/repo_folders.md) - Get acquainted with the project folders.
* [Variable types](general/types.md) - See what typedefs we use on this project.
* [Declaring globals](general/globals.md) - How we define globals on GTA2 and how they work.
* [Patching](general/patching.md) - Patching our functions into original GTA2 executable.

## Very important classes

Some classes are essential since they are used almost everywhere on GTA2:

* [Fix16](classes/fix16.md) - This is how the game avoids using floating points (except when it really need them).
* [Ang16](classes/ang16.md) - Another "Fix16" but for angles and rotations. They are fixed point but with less precision.

## Some questions

### Do I need the game to contribute?

No, you just need the repo files and the prerequisites listed on [Getting Started](getting_started.md).

### What version of GTA2 should I use/install?

Any version can be installed (for example v11.44 here: [https://gtamp.com/gta2/](https://gtamp.com/gta2/)). Since the main difference between GTA2 versions is on the executable, you should be able to use other versions rather than `10.5`, which is more difficult to find.

## Let's start!

There are mainly two ways to contribute for this project:

* [Matching functions](general/matching_funcs.md) - How to match new functions and include them into the repository.
* [Renaming functions or attributes](general/renaming.md) - There are many unknown attributes for many classes and way many functions without a proper name. You can figure them out and rename according to what it does.