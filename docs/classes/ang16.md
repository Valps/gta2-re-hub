# Ang16

Similar to Fix16, it is composed by a single attribute named `rValue`. It has a signed 16 bits type, or `s16`. The angle of peds, cars, sprites and even the rotation of figures (like the player health hearts) have Ang16 type. 

It has precision 4. This means that each angle degree is multiplied by 4. For example `rValue = 360` is actually `90°`. Its orientation is in general anti-clockwise. The maximum value is then `1439`, which is equivalent to `359.75°`.

Peds, cars, objects and sprites in general have this reference for `rValue`:

```
0 : South
360 : East
720 : North
1080 : West
```

## Normalization

Since `rValue` must be between `0` and `1439`, whenever we sum or subtract Ang16's we renormalize the angle. For this we use the method called `Normalize`:

```
void Normalize()
{
    for (; rValue < 0; rValue += 1440)
    {
        ;
    }
    for (; rValue >= 1440; rValue -= 1440)
    {
        ;
    }
}
```

Like Fix16 these operations are inlined on version 10.5 of GTA2. Therefore, on IDA Ang16 renormalizations often shows up in this way:

```
v1 = -dword_6787A0;
if ( (__int16)dword_6787A0 > 0 )
{
    LOWORD(v1) = 1440 * ((1439 - (__int16)-(__int16)dword_6787A0) / 0x5A0u) 
        - dword_6787A0;
}
if ( (__int16)v1 >= 1440 )
{
    LOWORD(v1) = (__int16)v1 % 0x5A0u;
}
this->field_130 = v1;
```

Whenever we see `0x5A0u` and `1440`, we suspect that an Ang16 renormalization is happening there.

## Main constructors

Unlike Fix16, `Ang16` constructors don't multiply the input value by the precision 4. The simplest constructor is

```
Ang16(s32 value) : rValue(value)
{
}
```

which assigns a value for `rValue`. 

On the other hand, you can put a second argument to make it normalize `rValue`, useful if you are initializing an Ang16 with a sum or subtraction of numbers:

```
Ang16(s16 value, u8 null2) : rValue(value)
{
    Normalize();
}
```

## Ang16 Math

You can add and subtract Ang16's like Fix16. But be aware that after each operation the angle will be renormalized. Note the second argument on constructors on the following overloads:

```
Ang16 operator+(const Ang16& rhs)
{
    return Ang16(rValue + rhs.rValue, 0);
}

Ang16 operator-(const Ang16& other)
{
    return Ang16(rValue - other.rValue, 0);
}
```

For example, if `var1, var2` and `var3` are Ang16's, then `var1 = var2 + var3` is actually

```
var1.rValue = var2.rValue + var3.rValue;
var1.Normalize().
```
