# Matching functions

Setup your IDE or text formatter to use `.clang-format`, which is on repo root folder. This makes the code more cleaner and consistent.

## Default procedure

* Open IDA database of GTA2 v10.5 (you can get it from our discord server)
* Pick a non-black colored function (non-matched function)
* Copy the function name you want to match.
* Create a decompme scratch using `generate_function_decompme.py`. For example, for the function `Ped::sub_45EE00` go to `Scripts` folder and run

```
python generate_function_decompme.py Ped::sub_45EE00
```

* If done correctly, a new browser tab will open on `decomp.me`. Then copy the IDA decompiled code into the decompme tab `Source code`.
* Now you need to get the context code, containing the needed classes and globals. You can generate it by running `generate_context.py` also on `Scripts` folder, or go to https://decomp.me/preset/152 and copy the context code from other GTA2 scratch. Put it on `Context` tab on your decompme scratch.

* Match the function. 

> In some cases some global names won't match with the target asm, but as long as they have the same 
> address, we count it as matched. Example: [https://decomp.me/scratch/Bc1c7](https://decomp.me/scratch/Bc1c7)

* Copy the matched function into the repo. Don't forget to format 

* If needed, make adjustments on the code (such as changing variable types on classes or functions) in order to fit the function without errors.

* Do not forget to replace `STUB_FUNC` (or `WIP_FUNC`) by `MATCH_FUNC` on your function.

* Build the game. If it was successful, then waits for the verification. If all functions with `MATCH_FUNC` matched, then the message `Build finished and verified successfully!` will be shown.

* Finally, repeat the process with another non-matched function or make a PR.

## Before moving it to repo... Get some advices

After getting a match on decomp.me, try to clean-up the code if possible. This includes:

* Remove unnecessary local variables. In some cases they are needed to match, so there is no problem.
* Put local variables declarations inside the scopes in which they are used (if possible). In other words, avoid leaving lots of locals defined on the beginning of the function if possible.
* Try to rewrite `do... while` loops as `for` loops. In many cases it is possible and it makes the code more readable. If it breaks the match and you cannot get it matching with `for` loops, do not worry and leave it as `do... while`.
* Remove all `this->` when referencing a class attribute. Do not worry if you forget doing it or you have much trouble with local renaming conflicts. Removing them makes the code a bit cleaner.