# Getting Started

We are decompiling the version 10.5 of GTA2 (also known as Freeloader version). This is a matching decomp, so the main goal is to replicate the original binary as faithfully as possible.

On this project we use the following nomenclatures for GTA2:

* `standalone` version - It is the executable obtaining from building the source code.
* `patched` version - It is the original executable patched with matched functions.
* `10.5` - The version of GTA2 we're decompiling.
* `9.6f` - The version of GTA2 released by Rockstar on Rockstar Classics in 2002-2003.
* `9.6` - Without the `f`, it's the GTA2 retail version released on `1999`.

## Building

### Prerequisites

* Python >= 3.7
* Wine (For Linux/Mac)
* `GTA2_ROOT` Environment variable pointing to your GTA2 installation

Clone the repository with the `--recursive` flag

```
git clone --recursive https://github.com/CriminalRETeam/gta2_re.git
```

Then run `gta2_data_setup.py`, which is located at `/Scripts/`.

### Windows 

```
pip install -r requirements.txt
python vc6_setup.py
python build.py
```

### Linux

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python3 vc6_setup.py
python3 build.py
```

## Optional arguments

Optionally, you can automatically run the built exe by passing one of the following arguments to `build.py`:

- `--run_standalone`
- `--run_patched`
- `--ignore_no_match`
- `--cores [number]` (maximum number of cpu cores to build with)
- `--reccmp` (for reccmp analysis)
- `--single_cpp [filename]` (useful for using objdiff)

After building it successfully, it will start a verification check to ensure that all functions marked with `MATCH_FUNC` indeed matches with the respective original binary functions.

The decomp build file will be on `/build_vc6/` named `decomp_main.exe`. You can copy it into game folder and run it.

### What version of GTA2 should I use/install?

We decompile functions of version 10.5, so for the decompiling process you will need the version 10.5 of the GTA2 executable. It's downloaded automatically after building, locating at `/Scripts/bin_comp/10.5.exe`.

On the other hand, if you want to test the build/executable, you need the game files. You will find many versions (like from R* Classics, GTAMP, Freeloader etc) but they all will work, since the game asset files (maps, script, audio etc) are basically the same. You can download the version v11.44 of the game (including only necessary game files) here: [https://gtamp.com/gta2/](https://gtamp.com/gta2/)

### Why is `decomp_main.exe` crashing after choosing a map level in menu?

In order to boot into a GTA2 map on standalone version, before entering the game you need to click at the big red button "`set boot2map debug opts`" located on the top of ImGui window. This initializate some essential variables and disables things that aren't working and causes crashes.

## How to contribute to the decompiling process?

See [Contribute to decompiling process](how_contribute.md).
