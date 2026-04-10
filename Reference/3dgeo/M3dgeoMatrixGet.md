---
doctype: Reference
module: 3dgeo
function: M3dgeoMatrixGet
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoMatrixGet"
---

# M3dgeoMatrixGet

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

> Get data from a transformation matrix object and place it in a user-supplied array.

## Syntax

```c
void M3dgeoMatrixGet(
    AIL_ID       Matrix3dgeoId,  //in
    AIL_INT64    GetType,        //in
    AIL_DOUBLE * UserArrayPtr    //out
)
```

## Description

This function copies the values from a transformation matrix object and places them in an array.

## Parameters

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the source transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `GetType` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `UserArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the user array in which to copy the data from the transformation matrix. Ensure that the user array is large enough to receive the data to be copied from the source object. Transformation matrices are 4x4 matrices, so the array must be of size 16.
