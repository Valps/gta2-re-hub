# Globals

The original game stores global variables in static addresses as well as global classes objects. In order to hook them into the patched version properly, we need to assign the original address for each global. 

However, on the other hand we need to initialize many of these globals in order to get the standalone version working as it should. In this sense we are currently using macros to define these globals.

On IDA globals often appear as `dword_XXXXXX`, `word_XXXXXX`, `byte_XXXXXX` or even `struct_XXXXXX`. Many globals are already renamed, such as `gSprite_6F61E8` or `gViewCamera_676978`.

## Globals declaration

* `DEFINE_GLOBAL` - Simple global declaration;
* `DEFINE_GLOBAL_INIT` - Global declaration with a initial value;
* `DEFINE_GLOBAL_ARRAY` - Simple global declaration but for arrays;
* `DEFINE_GLOBAL_ARRAY_INIT` - Global array declaration with their elements set up;

### DEFINE_GLOBAL

The simplest way to define a global is using `DEFINE_GLOBAL(var_type, var_name, address);`. You can use it for objects (such as `s32`, `Fix16`, `Ang16` etc) and for pointers (such as `MapRenderer*`) but never arrays. Some examples are shown below:

```
DEFINE_GLOBAL(Fix16, dword_6F628C, 0x6F628C);
DEFINE_GLOBAL(MapRenderer*, gpMapRenderer_6F66E4, 0x6F66E4);
```

> [!IMPORTANT]
> When defining a new global, ensure that:
>
> * (1) The address registered is indeed the original global address;
> * (2) It is not already defined elsewhere on the repository.
>
> If you don't give the correct address for a global, then the patched version will hook values/pointers from a wrong address causing bugs on the patched game.
>
> If the same global is defined multiple times (multiple globals containing the same original address), the standalone will crash on start. 
>
>Luckly we have a script that checks for duplicated globals whenever you build the game. When a duplicate is found, it will give you the duplicate names and their respective files where they were defined.

### DEFINE_GLOBAL_INIT

This macro is similar to `DEFINE_GLOBAL` but you set a initial value for it. This initial value only affects the standalone version.

It's used as `DEFINE_GLOBAL_INIT(var_type, var_name, initial_value, address);`. Some examples are shown below:

```
DEFINE_GLOBAL_INIT(s8, byte_620F20, -1, 0x620F20);
DEFINE_GLOBAL_INIT(Fix16, dword_6FD9E4, Fix16(0), 0x6FD9E4);
```

### DEFINE_GLOBAL_ARRAY

It is equivalent to `DEFINE_GLOBAL` but for arrays. 

```
DEFINE_GLOBAL_ARRAY(array_var_type, array_name, array_length, address);
```

One example of its usage is

```
DEFINE_GLOBAL_ARRAY(wchar_t, gGangNameWide_67E030, 4, 0x67E030);
```

which is equivalent to `wchar_t gGangNameWide_67E030[4];`.

### DEFINE_GLOBAL_ARRAY_INIT

For last, you should use `DEFINE_GLOBAL_ARRAY_INIT` for declaring global initialized arrays. It's used on this way:

```
DEFINE_GLOBAL_ARRAY_INIT(array_var_type, array_name, array_length, address, values[...]);
```

One example of it is shown below:

```
DEFINE_GLOBAL_ARRAY_INIT(char*, 
                         error_table_61A6D4, 
                         13, 
                         0x61A6D4, 
                         "Invalid integer",
                         "Number too big",
                         "Write error",
                         "EOF in comment",
                         "Token too big",
                         "Input file not found",
                         "Can't open output file",
                         "Invalid long integer",
                         "Invalid hex integer",
                         "Invalid hex long integer",
                         "Invalid f32",
                         "Output too big",
                         "Unknown error");
```

## How to use "extern" on globals

Some globals are used in different cpp files. Once we can't defined multiple globals, then we need to reference them in some way. The macro `EXTERN_GLOBAL` does exactly this. Its usage is very simple:

```
EXTERN_GLOBAL(var_type, var_name);
```

Example:

```
EXTERN_GLOBAL(Sprite*, gSprite_6F61E8);
```

For global arrays, we use `EXTERN_GLOBAL_ARRAY`:

```
EXTERN_GLOBAL_ARRAY(array_type, array_name, array_size);
```
