---
doctype: Reference
module: func
function: MfuncAllocId
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncAllocId"
---

# MfuncAllocId

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

> Associate an Aurora Imaging Library identifier with a user-defined object.

## Syntax

```c
AIL_ID MfuncAllocId(
    AIL_ID    ContextFuncId,  //in
    AIL_INT64 AilObjectType,  //in
    void *    UserObjectPtr   //in
)
```

## Description

This function allows you to associate an Aurora Imaging Library identifier with a user-defined object (such as, a structure or an array). You can associate an Aurora Imaging Library identifier with a user object so that it is recognized by Aurora Imaging Library and treated as a standard Aurora Imaging Library object during tracing or error handling. Once the object is associated with an identifier, the object is known as a user-defined Aurora Imaging Library object.

The parameters passed to [`MfuncAllocId`](../../Reference/func/MfuncAllocId.md) do not allow you to specify whether the user-defined object is allocated on the Host or on a system with an on-board processor. For information on creating user-defined objects on systems with on-board processors and associating these object with Aurora Imaging Library identifiers, see [Associating an Aurora Imaging Library identifier with a user-defined object](../../UserGuide/C68_The_AIL_function_development_module/S08_Associating_an_identifier_with_a_userdefined_object.md).

You must also specify an Aurora Imaging Library type for your object ([`AilObjectType`](../../Reference/func/MfuncAllocId.md) parameter). Aurora Imaging Library allows up to 32 different user-defined Aurora Imaging Library object types.

Use the Aurora Imaging Library identifier to refer to the user-defined Aurora Imaging Library object. To refer to the actual data grouped into this user-defined object, use the pointer to the object ([`UserObjectPtr`](../../Reference/func/MfuncAllocId.md) parameter).

To inquire about the type of this object, or the pointer to it, use the [`MfuncInquire`](../../Reference/func/MfuncInquire.md) function.

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies an Aurora Imaging Library function identifier. This parameter can be set to the following values:

*For an Aurora Imaging Library function identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default Aurora Imaging Library function identifier. Use this value to create user-defined Aurora Imaging Library objects outside of a user-defined Aurora Imaging Library function. |
| `Function identifier` | Specifies the identifier of a user-defined Aurora Imaging Library function. |

### `AilObjectType` *(in, AIL_INT64)*

Specifies the Aurora Imaging Library type of the user-defined object to be associated with an Aurora Imaging Library identifier. To specify the type, you must specify one of the groups listed in the table below and combine it with an offset. The two object groups allow you to distinguish between custom created objects, especially when the created objects are to be used in different Aurora Imaging Library modules.

*For specifying the Aurora Imaging Library type of user-defined object*

| Value | Description |
| --- | --- |
| `M_USER_OBJECT_1` | Specifies that the user object falls under the first group of user object types. |
| `M_USER_OBJECT_2` | Specifies that the user object falls under the second group of user object types. |

*For distinguishing between the different object types*

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset within the selected object type group. The value must have only one of its 16 least significant bits set. |

### `UserObjectPtr` *(in, *void)*

Specifies the address of the user-defined object that is to be associated with an Aurora Imaging Library identifier. Note that user-defined objects stored in the memory of an on-board processor are not accessible from Host, and vice versa.

## Return Value

**Type:** `AIL_ID`

The returned value is the Aurora Imaging Library identifier associated with the object if the allocation is successful. If allocation fails, `M_NULL` is returned.
