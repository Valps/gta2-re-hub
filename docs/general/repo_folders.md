# Repository folders

The root folder of the project has these folders:

```
3rdParty
Scripts
Source
cmake
reccmp
```

## 3rdParty

In 3rdParty folder you will find:

* `gta2_re_compile_tools` - Contains the compiler used on this project: MSVC6.
* `GTA2Hax` - Contains the reverse engineering of `DmaVideo.dll` and `d3ddll.dll`.

## /Scripts/

Here you will find useful scripts for the project. It will look like this:

```
/bin_comp/
/ida/

generate_function_decompme.py 
generate_context.py
gta2_data_setup.py
report_progress_to_discord.py
```

* `generate_function_decompme.py` : It reads a GTA2 function (from version 10.5 or 9.6f) and generates a `decomp.me` link with the target assembly. Very useful for creating scratches for functions.
* `generate_context.py` : It reads all headers from source code (respecting the #include dependency order) and copy to the clipboard. Useful for the `context` tab on decomp.me.
* `report_progress_to_discord.py` : It sends the matching progress to the discord. It shouldn't be used as it's automatically executed on github jobs after merging.

### /Scripts/bin_comp/

On this folder you will find Python scripts that get the assembly code from the game binary `.exe` and that verifies if each function marked as `MATCH_FUNC` is really matched by comparing post-processed assembly codes from both original binary and from built binary.

This folder contains the original binary of GTA2 on version 10.5 (Freeloader version). It will have the name `10.5.exe`.

On `bin_comp` folder you may also find the folder `diff`, which might contains files comparing assembly code for original and build binary. These files are automatically generated when a function marked as `MATCH_FUNC` fails to match with the original function.

### /Scripts/ida/

This folder contains Python scripts that can be used in IDA on tab `File -> Script file` or just pressing `Alt+F7`.

The most useful script is `import_match_status.py`, which updates the color of IDA functions based on the matching progress. You can see more about it [here](ida_scripts.md).

## /Source/

It contains all source code required to compile & build the following files:

* `decomp_main.exe` - Game main executable;
* `ExePatcher.exe` - Patcher: it patches functions marked with `MATCH_FUNC` into original executable. [See more](patching.md);
* `HookLoader.dll` - Hook loader for globals and functions into original executable;
* `gta2_dll_exports.dll` - Export dll;
* `gta2_dll_imports.dll` - Import dll;

It is on this folder (`Source`) that you will find all classes/structs used to build the game. This includes cars & peds behaviours, map functions, graphical rendering, network, frontend, police/ambulance/firetrucks behaviour etc.

## /cmake/

On this folder you will find the file `vc6.cmake` which contains the compiler flag/library/include setup for building.

## /reccmp/

This folder contains files related with the tool `reccmp` from https://github.com/isledecomp/reccmp . It's not fully functional yet as some actually matched functions are being reported with mismatches.
