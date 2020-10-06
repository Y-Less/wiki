---
id: OnPlayerEditAttachedObject
title: OnPlayerEditAttachedObject
description: This callback is called when a player ends attached object edition mode.
tags: ["player"]
---

:::warning

Callback นี้ถูกเพิ่มใน SA-MP 0.3e และจะไม่ทำงานในเวอร์ชั่นก่อนหน้านี้!

:::

## คำอธิบาย

This callback is called when a player ends attached object edition mode.

| Name           | Description                                                   |
| -------------- | ------------------------------------------------------------- |
| playerid       | The ID of the player that ended edition mode                  |
| response       | 0 if they cancelled (ESC) or 1 if they clicked the save icon. |
| modelid        | The model of the attached object that was edited              |
| boneid         | The bone of the attached object that was edited               |
| Float:fOffsetX | The X offset for the attached object that was edited          |
| Float:fOffsetY | The Y offset for the attached object that was edited          |
| Float:fOffsetZ | The Z offset for the attached object that was edited          |
| Float:fRotX    | The X rotation for the attached object that was edited        |
| Float:fRotY    | The Y rotation for the attached object that was edited        |
| Float:fRotZ    | The Z rotation for the attached object that was edited        |
| Float:fScaleX  | The X scale for the attached object that was edited           |
| Float:fScaleY  | The Y scale for the attached object that was edited           |
| Float:fScaleZ  | The Z scale for the attached object that was edited           |

## ส่งคืน

1 - Will prevent other scripts from receiving this callback.

0 - Indicates that this callback will be passed to the next script.

มันถูกเรียกในฟิลเตอร์สคริปต์ก่อนเสมอ

## ตัวอย่าง

```c
enum attached_object_data
{
    Float:ao_x,
    Float:ao_y,
    Float:ao_z,
    Float:ao_rx,
    Float:ao_ry,
    Float:ao_rz,
    Float:ao_sx,
    Float:ao_sy,
    Float:ao_sz
}

new ao[MAX_PLAYERS][MAX_PLAYER_ATTACHED_OBJECTS][attached_object_data];

// The data should be stored in the above array when attached objects are attached.

public OnPlayerEditAttachedObject(playerid, response, index, modelid, boneid, Float:fOffsetX, Float:fOffsetY, Float:fOffsetZ, Float:fRotX, Float:fRotY, Float:fRotZ, Float:fScaleX, Float:fScaleY, Float:fScaleZ)
{
    if(response)
    {
        SendClientMessage(playerid, COLOR_GREEN, "Attached object edition saved.");

        ao[playerid][index][ao_x] = fOffsetX;
        ao[playerid][index][ao_y] = fOffsetY;
        ao[playerid][index][ao_z] = fOffsetZ;
        ao[playerid][index][ao_rx] = fRotX;
        ao[playerid][index][ao_ry] = fRotY;
        ao[playerid][index][ao_rz] = fRotZ;
        ao[playerid][index][ao_sx] = fScaleX;
        ao[playerid][index][ao_sy] = fScaleY;
        ao[playerid][index][ao_sz] = fScaleZ;
    }
    else
    {
        SendClientMessage(playerid, COLOR_RED, "Attached object edition not saved.");

        new i = index;
        SetPlayerAttachedObject(playerid, index, modelid, boneid, ao[playerid][i][ao_x], ao[playerid][i][ao_y], ao[playerid][i][ao_z], ao[playerid][i][ao_rx], ao[playerid][i][ao_ry], ao[playerid][i][ao_rz], ao[playerid][i][ao_sx], ao[playerid][i][ao_sy], ao[playerid][i][ao_sz]);
    }
    return 1;
}
```

## บันทึก

:::warning

Editions should be discarded if response was '0' (cancelled). This must be done by storing the offsets etc. in an array BEFORE using EditAttachedObject.

:::

## ฟังก์ชั่นที่เกี่ยวข้องกัน

- [EditAttachedObject](../../scripting/functions/EditAttachedObject.md): Edit an attached object.
- [SetPlayerAttachedObject](../../scripting/functions/SetPlayerAttachedObject.md): Attach an object to a player