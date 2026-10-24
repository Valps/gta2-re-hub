# Markers

On GTA2 source code you will find 3 types of function markers: `MATCH_FUNC`, `STUB_FUNC` and `WIP_FUNC`. They are always placed one line before function's body and they always carry 1 argument, which is the function address.

## MATCH_FUNC

Functions with this marker will have their assembly compared with the respective original function. If the verification fails, files containing the original and the built assembly code will be created on `/Scripts/bin_comp/diff/` ([See more](diff.md)).

Also, independently if it matches or not, these functions will be patched on original executable on executing `ExePatcher.exe`. So you can test implemented non-matched functions or even try to change a matched function to see what happens on the game. In this case, it's recommended to use `--ignore_no_match` when running `build.py`.

One example of its usage is
```
MATCH_FUNC(0x4BF2F0);
s8 GangPool_CA8::FindGangByCarModel_4BF2F0(s32 car_model)
{
    for (u8 i = 0; i < 10; i++)
    {
        if (GangByIdx_4BF1C0(i)->field_13C_gang_car_model == car_model)
        {
            return i;
        }
    }
    return -1;
}
```

## STUB_FUNC

Stubbed functions are ones that are on the repository (on `Source` folder) but not matched. They look like this:

```
STUB_FUNC(0x4f3c00)
void MapRenderer::draw_left_4F3C00(u16* arg0, s32* pVertIdx, s32 a2, Fix16_Point* a5)
{
    NOT_IMPLEMENTED;
}
```

## WIP_FUNC

Work-in-progress functions. For building and patching purposes, it's the same as `STUB_FUNC` but it has a different color on IDA. Also used for statistics when reporting progress to discord.

One example of its usage is this function which renders 1 of the 4 diagonal wall slopes. Although the assembly is not fully matched, it has pretty much the same behaviour as the original function.

```
WIP_FUNC(0x4ec450)
void MapRenderer::sub_4EC450(u16& left_word)
{
    WIP_IMPLEMENTED;
    Ang16 rotation;
    sub_46BDF0(gXCoord_6F63AC + stru_6F6484.y, gYCoord_6F63B8, &gTileVerts_6F65A8[0]);
    sub_46BD40(gXCoord_6F63AC + stru_6F6484.y, gYCoord_6F63B8, &gTileVerts_6F65A8[1]);

    rotation = Fix16::atan2_fixed_405320(Fix16(gTileVerts_6F65A8[1].x - gTileVerts_6F65A8[0].x),
                                         Fix16(gTileVerts_6F65A8[1].y - gTileVerts_6F65A8[0].y));

    if (rotation < word_6F6414 || rotation > word_6F6420) // optimization
    {
        sub_46BD40(gXCoord_6F63AC, gYCoord_6F63B8 + stru_6F6484.y, &gTileVerts_6F65A8[2]);
        sub_46BDF0(gXCoord_6F63AC, gYCoord_6F63B8 + stru_6F6484.y, &gTileVerts_6F65A8[3]);

        dword_6F6560 = dword_620FE4[left_word >> 13];
        u16 texture_idx = gGtx_0x106C_703DD4->GetTile_5AA870(left_word & 1023);
        if (texture_idx)
        {
            pgbh_DrawTile(dword_6F6560 | gLightingDrawFlag_7068F4,
                          gSharp_pare_0x15D8_705064->field_0_textures1[texture_idx],
                          gTileVerts_6F65A8,
                          field_10);
            ++field_2F00_drawn_tile_count;
        }
    }
}
```
