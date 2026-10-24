# Commands

The script current command arguments are stored on the global `gBasePtr_6F8070`. They are naturally `u16` type, although this can change depending on what kind of argument is (for example a `Fix16` value, which naturally occupies 4 bytes).

Each script entity created by `CHAR_DATA`, `CAR_DATA`, `OBJ_DATA`, `CRANE_DATA`, `COUNTER` etc will have an associated index. To get the in-game pointer to that entity you need to call the function `gScriptManager_6F8060->GetBasePointer_512770`. It will give you a `SCR_POINTER` type. Here is its struct:

```c
struct SCR_POINTER : SCR_CMD_HEADER
{
    union
    {
        Ped* field_8_char;
        Car* field_8_car;
        Object* field_8_obj;
        Crane* field_8_crane;
        Light* field_8_light;
        Door* field_8_door;
        Crusher* field_8_crusher;
        s32 field_8_counter;
        u16 field_8_index;
        Generator* field_8_generator;
        Hud_Arrow* field_8_arrow;
    };
};
```

If the argument of your command is given by a `CHAR_DATA` entity, then you use the pointer given by `Ped* field_8_char`, and so on. Here is a list of them:

```
CHAR_DATA: field_8_char
CAR_DATA: field_8_car
OBJ_DATA: field_8_obj
CRANE_DATA: field_8_crane
LIGHT_DATA: field_8_light
COUNTER: field_8_counter
DOOR_DATA: field_8_door
ARROW_DATA: field_8_arrow
CRUSHER: field_8_crusher
GENERATOR: field_8_generator
```

The `u16 field_8_index` is used for identifying created pagers and bonuses for example.

(WIP)