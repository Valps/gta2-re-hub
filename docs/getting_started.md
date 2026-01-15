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

> [!IMPORTANT]
> To use `--run_patched`, you need to generate the patched exe yourself by running `ExePatcher.exe` within the `build_vc6` folder. The patcher expects the original GTA2 exe (called `10.5.exe`) to be inside the same directory.

After building it successfully, it will start a verification check to ensure that all functions marked with `MATCH_FUNC` indeed matches with the respective original binary functions.

The decomp build file will be on `/build_vc6/` named `decomp_main.exe`. You can copy it into game folder and run it.

### What version of GTA2 should I use/install?

Any version can be installed (for example v11.44 here: [https://gtamp.com/gta2/](https://gtamp.com/gta2/)). Since the main difference between GTA2 versions is on the executable, you should be able to use other versions rather than `10.5`, which is more difficult to find.

## How to contribute to the decompiling process?

See [Contribute to decompiling process](how_contribute.md).
