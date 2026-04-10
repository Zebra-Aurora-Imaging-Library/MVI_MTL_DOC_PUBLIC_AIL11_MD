---
doctype: Reference
module: 3dgra
function: M3dgraRemove
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraRemove"
---

# M3dgraRemove

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Remove a 3D graphic from a 3D graphics list.

## Syntax

```c
void M3dgraRemove(
    AIL_ID    List3dgraId,  //out
    AIL_INT64 Label,        //in
    AIL_INT64 ControlFlag   //in
)
```

## Description

This function removes a 3D graphic from a 3D graphics list. Removing a 3D graphic from a 3D graphics list will also remove all of its children from the 3D graphics list.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to remove the 3D graphic or node. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `Label` *(in, AIL_INT64)*

Specifies the label of the 3D graphic to remove.

*For specifying the label of the 3D graphic or node*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to clear the 3D graphics list, leaving the root node as the only element in the 3D graphics list. |
| `Value > 0` | Specifies the label of the 3D graphic to remove. The root node cannot be removed. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
