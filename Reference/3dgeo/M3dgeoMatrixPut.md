---
doctype: Reference
module: 3dgeo
function: M3dgeoMatrixPut
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoMatrixPut"
---

# M3dgeoMatrixPut

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

> Put data from a user-supplied array into a transformation matrix object.

## Syntax

```c
void M3dgeoMatrixPut(
    AIL_ID             Matrix3dgeoId,  //out
    AIL_INT64          PutType,        //in
    const AIL_DOUBLE * UserArrayPtr    //in
)
```

## Description

This function copies an array of values into a specified transformation matrix object.

## Parameters

### `Matrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `PutType` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `UserArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the user array from which to copy the data into the transformation matrix object. Ensure that user array is large enough to contain the data to be copied to the destination object. Transformation matrices are 4x4 matrices, so the array must be of size 16.
