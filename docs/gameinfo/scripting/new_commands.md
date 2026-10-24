# Implementing new commands

When creating a new command, think about the arguments you need to execute what you want in the game. For example `SET_FAVOURITE_MODEL` needs two arguments: the char and the car model.

Then create a struct inheriting `SCR_CMD_HEADER` with the fields you will get from compiled script. Most of these fields are `u16` type, such as the script character index coming from `CHAR_DATA`.

Then we create the function responsible for the command execution in the game. There are some things to be aware of:

- The function must always return nothing (`void` return type) even if the command returns a boolean type on the script (such as in a IF/WHILE check statement).
- You MUST call `Next_503620(gBasePtr_6F8070)` at the very end of the function. Otherwise the script will stall forever and won't execute the next ones. (Exceptions: special commands which changes the flux of the script code, like `GOTO`)

For last you should add a switch case on `ScriptThread::ExecOpCode` or `ScriptThread::PreExecOpCode` functions that calls your new command function. If it's meant to be executed only on game start (i.e. before `LEVELSTART`), then link your new function on `ScriptThread::PreExecOpCode`. If it's meant to be executed during gameplay (i.e. after `LEVELSTART`) then implement it on `ScriptThread::ExecOpCode`. 

Here we show some examples of how to implement new commands.


### Command that stores information in the counters

This command stores the player current position (with `Fix16` precision). Similarly to `STORE_SCORE`, `STORE_LAST_CHAR_PUNCHED`, `GET_CAR_INFO_FROM_CRANE` etc it stores some information on the counters. For example this is how we must use this command in script:

```
CHAR_DATA my_char
COUNTER xpos
COUNTER ypos
COUNTER zpos

LEVELSTART

GET_CHAR_COORDS (my_char, xpos, ypos, zpos)
```

To create this new command `GET_CHAR_COORDS` first we create a struct like this:

```c
struct SCR_GET_CHAR_COORDS : SCR_CMD_HEADER
{
    u16 char_idx;
    u16 xpos_counter_idx;
    u16 ypos_counter_idx;
    u16 zpos_counter_idx;
};
```

Then the `GET_CHAR_COORDS` code must be something like this:

```c
void ScriptThread::SCRCMD_GET_CHAR_COORDS()
{
    SCR_GET_CHAR_COORDS* pCmd = (SCR_GET_CHAR_COORDS*)gBasePtr_6F8070;
    SCR_POINTER* pPtr = (SCR_POINTER*)gScriptManager_6F8060->GetBasePointer_512770(pCmd->char_idx);

    if (pPtr->field_8_char)
    {
        SCR_POINTER* pXpos = (SCR_POINTER*)gScriptManager_6F8060->GetBasePointer_512770(pCmd->xpos_counter_idx);
        SCR_POINTER* pYpos = (SCR_POINTER*)gScriptManager_6F8060->GetBasePointer_512770(pCmd->ypos_counter_idx);
        SCR_POINTER* pZpos = (SCR_POINTER*)gScriptManager_6F8060->GetBasePointer_512770(pCmd->zpos_counter_idx);

        pXpos->field_8_counter = pPtr->field_8_char->get_cam_x().mValue;
        pYpos->field_8_counter = pPtr->field_8_char->get_cam_y().mValue;
        pZpos->field_8_counter = pPtr->field_8_char->get_cam_z().mValue;
    }

    ScriptThread::Next_503620(gBasePtr_6F8070);
}
```

#### Explaining the function: 

First we cast `gBasePtr_6F8070` to `SCR_GET_CHAR_COORDS` pointer type, so we can use our new struct made for this new command. 

Then the use `GetBasePointer_512770` to get the character in-game pointer from its script index `char_idx`.

Before doing anything, we check if `field_8_char` it's not a null pointer, as it could crash the game should we were passing a non-created ped or a despawned one etc.

Then we use `GetBasePointer_512770` to get the `xpos`, `ypos` and `zpos` counter pointers. Finally we set their values by using `field_8_counter`.

Finally, we call `Next_503620(gBasePtr_6F8070)`.

#### Linking the new function to script

Now we want the script thread be able to find our new command function `SCRCMD_GET_CHAR_COORDS()`. First set an enum for our command in `SCRCMD`:

```c
enum SCRCMD
{
    // ...
    SCRCMD_GET_CHAR_COORDS; // = some number
}
```

Now, since `GET_CHAR_COORDS` is meant to be executed after `LEVELSTART`, we should link our function on `ScriptThread::ExecOpCode`:

```c
ScriptThread::ExecOpCode()
{
    // ...
    switch (pBasePtr->field_2_type)
    {
        /// .....
        case SCRCMD_GET_CHAR_COORDS:  // Here we link our command using its enum and its function
            ScriptThread::SCRCMD_GET_CHAR_COORDS();
            break;
    }
}
```

Then it's done. Our function is ready to be executed in game if the script requests it.

### Command that returns a boolean value

In this example we want to create a new command that must be used inside a `IF`, `WHILE` or `WHILE_EXEC` statement.

For this we must use the `ScriptThread` field called `field_8_cond_result`.

In this example we create a new command which checks if a given character is a player or not. It 

```c
struct SCR_IS_CHAR_PLAYER : SCR_CMD_HEADER
{
    u16 char_idx;
};
```

This is how the command should look like in the source code:

```c
void ScriptThread::SCRCMD_IS_CHAR_PLAYER()
{
    SCR_IS_CHAR_PLAYER* pCmd = (SCR_IS_CHAR_PLAYER*)gBasePtr_6F8070;
    SCR_POINTER* pPtr = (SCR_POINTER*)gScriptManager_6F8060->GetBasePointer_512770(pCmd->char_idx);

    if (pPtr->field_8_char && pPtr->field_8_char->is_player_41B0A0())
    {
        field_8_cond_result = true;
    }
    else
    {
        field_8_cond_result = false;
    }

    ScriptThread::Next_503620(gBasePtr_6F8070);
}
```

If the character exists and it is associated with a player, then set `field_8_cond_result` true and go on. Set it to false otherwise.

### Command that has a variable number of arguments

(WIP)