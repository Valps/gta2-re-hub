# Patching into original exe

OBS: it's recomended to setup `GTA2_ROOT` path environment to the game installation root folder (which contains dlls and the executable itself).

To get the patched version, first you have to build the game by running `build.py` on root folder of the project.

After building it successfully, a new folder `build_vc6` will be created containing all built files. Then you put the original GTA2 executable (version 10.5) on this folder with the name `10.5.exe` and finally run `ExePatcher.exe`, which is in the same folder (`/build_vc6/`).

If it patched correctly, a new file called `10.5.new.exe` will appear on `/build_vc6/`. Then copy this executable into the game installation root folder and run it.

If you done all steps correctly, a debug console will start before the game boots in, printing HookLoader operations as well as some d3ddll and DmaVideo stuff. 

After the game started, you will see a ImGui window called `Debugger`. Here is where the magic of patching can be used to explore the game or even mess up with the game and having some fun. The patched version not only patches functions marked with `MATCH_FUNC` but it also implements ImGui in the game. Throught it you can see and change, in real time, values from the game and call game functions. Or even call functions that you have created.

The ImGui `Debugger` buttons, sliders and displays are implemented on `Source/ImGuiDebug.cpp` file.
