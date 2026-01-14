# Fix16

Floats/doubles are rarely used in GTA2. Instead, in most cases it uses a fixed point class which we called `Fix16`. It consists by a single 32 bits signed attribute called `mValue`.

The precision used in GTA2 for fixed points is `16384 = 0x4000`. So a Fix16 with `mValue = 16384` means that it is equivalent to `1.0` in float.

## Convertions

Very often the game converts `Fix16` into `u8`, `s32` or `f32`.

One example is when the game converts map coordinates (which are `Fix16`) into "block coordinates", which are integer numbers (like `(x,y,z) = (145, 20, 3)`).

### Converting Fix16 into integer type

Since `16384 = 2^14 = 1 << 14`, one of most used conversions is `Fix16 -> s32`:
```
inline s32 ToInt() const
{
    return mValue >> 14;
}
```

or `Fix16 -> u8`:

```
inline u8 ToUInt8() const
{
    return mValue >> 14;
}
```

One example of its usage is on the following snippet:
```
block_4DFE10 = Map_0x370::get_block_4DFE10(
                   this,
                   a2 >> 14,
                   (a3 - dword_6F6110) >> 14,
                   a4 >> 14);
```

This is how it appears on IDA. In almost all cases it shows up as `>> 14`. The actual code is

```
block_4DFE10 = Map_0x370::get_block_4DFE10(
                   a2.ToInt(),
                   (a3 - dword_6F6110).ToInt(),
                   a4.ToInt());
```

The reason that the `ToInt()` method doesn't appear on IDA is because many of `Fix16` methods are inlined on version 10.5 of GTA2.

### Converting integer type into Fix16

The other very used conversion is the opposite. Instead of `>> 14`, we find `<< 14`. It is contained in one of the constructors of Fix16:

```
Fix16(s32 value)
{
    mValue = value << 14;
}
```

It appears on IDA like this:

```
v19.mValue = v6 << 14;
```

Note that we rarely use `mValue` directly. In this case, the actual code should be

```
Fix16 v19(v6);  //  or Fix16 v19 = Fix16(v6)
```

### Float convertion

In some cases, floats are necessary in the game (such as dealing with graphical render vertices). In these cases `Fix16` can be converted into floats using this method:

```
float ToFloat() const
{
    return (mValue / 16384.0f);
}
```

Since `1/16384 = 0.000061035156`, this method appears on IDA on this way:

```
gTileVerts_6F65A8[1].field_8_z = (float)dword_6F6318 * 0.000061035156
```

Actually it is

```
gTileVerts_6F65A8[1].field_8_z = dword_6F6318.ToFloat()
```

## Fixed point Math

Sum and subtraction between Fix16's are the ordinary operations between integers. So `Fix16(1) + Fix16(2) = Fix16(3)` and `Fix16(3) - Fix16(1) = Fix16(2)`.

However, multiplication and division between Fix16 are little different. For multiplication, after multiplying their respective `mValue`s, we need to divide the result by the precision, 16384:

```
Fix16 operator*(const Fix16& in) const
{
    s32 value = (s32)((mValue * (__int64)in.mValue) >> 14);
    return Fix16(value, 0);
}
```

For division operation, we multiply the numerator by the precision and then divide it by the denominator's `mValue`:

```
Fix16 operator/(const Fix16& in)
{
    s32 value = (s32)(((__int64)mValue << 14) / in.mValue);
    return Fix16(value, 0);
}
```

These subleties are justified by fixed point math. For example, since when multiplying two `mValue`s we're carrying two `16384` factors, the result must be normalized dividing it by `16384`, so the result is a Fix16.

> [!IMPORTANT]
If you sum (or subtract) a Fix16 with a integer type, the integer is converted to Fix16 before carrying the operation. So `Fix16(5) + 2` is actually `Fix16(5) + Fix16(2)`.

For multiplication between Fix16 and a integer, the operation is the usual one. For example `Fix16(4) * 2` is `Fix16(8)`.

## Useful methods

Besides conversion, we have other useful methods:

### Absolute value

```
Fix16 var(2);
Fix16 var_abs = Fix16::Abs(var);
```

### Square root

One can operate square root of Fix16 using the method below:

```
Fix16 var(2);
Fix16 var_sqrt = Fix16::SquareRoot(var);
```

## Other Fix16 constructors

If one wants to initialize a `Fix16` with a raw value for `mValue` (i.e. without multiplying it by `16384`), then they need to put another number (usually zero) as a second argument. The constructor method needed is

```
Fix16(s32 value, u8) : mValue(value)
{
}
```

So, in this way, `Fix16 var = Fix16(0x4000, 0)` means that `mValue` assumes `0x4000` instead of `0x4000 * 16384`.

