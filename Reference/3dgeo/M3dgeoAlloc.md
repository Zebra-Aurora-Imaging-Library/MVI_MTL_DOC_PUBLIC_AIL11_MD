---
doctype: Reference
module: 3dgeo
function: M3dgeoAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoAlloc"
---

# M3dgeoAlloc

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

> Allocate a 3D geometry object or transformation matrix object.

## Syntax

```c
AIL_ID M3dgeoAlloc(
    AIL_ID    SysId,                      //in
    AIL_INT64 ObjectType,                 //in
    AIL_INT64 ControlFlag,                //in
    AIL_ID *  GeometryOrMatrix3dgeoIdPtr  //out
)
```

## Description

This function allocates a 3D geometry object or transformation matrix object on the specified system.

Note that 3D geometry objects or transformation matrix objects allocated with [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) can be saved using either [`M3dgeoStream`](../../Reference/3dgeo/M3dgeoStream.md) or [`M3dgeoSave`](../../Reference/3dgeo/M3dgeoSave.md) and restored using [`M3dgeoRestore`](../../Reference/3dgeo/M3dgeoRestore.md).

When the 3D geometry object or transformation matrix object is no longer required, release it using[`M3dgeoFree`](../../Reference/3dgeo/M3dgeoFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dgeo/M3dgeoAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dgeo/M3dgeoAlloc.md) was specified, the smart identifier manages the 3D geometry object or transformation matrix object's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D geometry object or transformation matrix object.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ObjectType` *(in, AIL_INT64)*

Specifies to allocate a 3D geometry object or transformation matrix object. This parameter must be set to one of the following:

*For specifying the object type*

| Value | Description |
| --- | --- |
| `M_GEOMETRY` | Allocates a 3D geometry object on the specified system. |
| `M_TRANSFORMATION_MATRIX` | Allocates a transformation matrix object on the specified system. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `GeometryOrMatrix3dgeoIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D geometry object or transformation matrix object identifier or specifies the data type that the function should use to return the 3D geometry object or transformation matrix object identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D geometry object or transformation matrix object; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D geometry object or transformation matrix object; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DGEO_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D geometry object or transformation matrix object (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D geometry object identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D geometry object identifier.

If allocation fails, [`M_NULL`](../../Reference/3dgeo/M3dgeoAlloc.md) is written as the identifier. |
| `Address in which to write the transformation matrix object identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated transformation matrix object identifier.

If allocation fails, [`M_NULL`](../../Reference/3dgeo/M3dgeoAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D geometry object or transformation matrix object identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DGEO_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dgeo/M3dgeoAlloc.md) was specified).
