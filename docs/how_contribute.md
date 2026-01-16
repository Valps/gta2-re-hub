## Contribute to decompiling process

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

We decompile functions of version 10.5, so for the decompiling process you will need the version 10.5 of the GTA2 executable. It's downloaded automatically after building, locating at `/Scripts/bin_comp/10.5.exe`.

On the other hand, if you want to test the build/executable, you need the game files. You will find many versions (like from R* Classics, GTAMP, Freeloader etc) but they all will work, since the game asset files (maps, script, audio etc) are basically the same. You can download the version v11.44 of the game (including only necessary game files) here: [https://gtamp.com/gta2/](https://gtamp.com/gta2/)

The version 9.6f has different addresses from 10.5. However, we still look into 9.6f because some 10.5 functions only match using 9.6f inlined functions as version 9.6f has inlines disabled and thus we can see these functions calls.

### Why is the version 9.6f so important for decompiling 10.5?

The version 9.6f has different addresses from 10.5. However, we still look into 9.6f because some 10.5 functions only match using 9.6f inlined functions as version 9.6f has inlines disabled and thus we can see these functions calls.

## Let's start!

There are mainly two ways to contribute for this project:

* [Matching functions](general/matching_funcs.md) - How to match new functions and include them into the repository.
* [Renaming functions or attributes](general/renaming.md) - There are many unknown attributes for many classes and way many functions without a proper name. You can figure them out and rename according to what it does.