---
doctype: Reference
module: 3dmeas
function: M3dmeasDefine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasDefine"
---

# M3dmeasDefine

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

> Add a path or template to, or delete a path or template from, a path or template 3D measurement context.

## Syntax

```c
AIL_INT64 M3dmeasDefine(
    AIL_ID             PathOrTemplateContext3dmeasId,  //out
    AIL_ID             Geometry3dgeoId,                //in
    AIL_ID             Matrix3dgeoId,                  //in
    AIL_INT            PathOrTemplateIndex,            //in
    AIL_INT64          Operation,                      //in
    AIL_INT            MarkerType,                     //in
    AIL_INT            TransitionType,                 //in
    AIL_INT            NumberMode,                     //in
    AIL_INT            LengthSource,                   //in
    AIL_INT            LengthMode,                     //in
    AIL_INT            NumberValue,                    //in
    const AIL_DOUBLE * ParamArrayPtr,                  //in
    AIL_INT64          ControlFlag                     //in
)
```

## Description

This function allows you to add or delete the path or template that is used to find markers. Note that a path can only be added to, or deleted from, a path 3D measurement context, and a template can only be added to, or deleted from, a template 3D measurement context.

Setting a path is useful when you want to find an object but you don't know its exact location. You can set a path that crosses through possible locations to detect height changes along the line. Setting a template is useful when you want to check whether an object is at an expected location. You can set a template where you expect a height change to occur, and verify if the object is present and has the shape you expect. You can use [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to fit a geometry to the found markers to compare it with the template that you defined.

You can only define one path per path 3D measurement context and one template per template 3D measurement context. When adding a path or template to the context, it is assigned an index of 0. Note that you cannot add a path or template to a profile 3D measurement context. A profile 3D measurement context has a default template that corresponds to the profiles supplied to [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) and cannot be defined nor deleted.

After defining the path or template, use [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) to find markers along the path or find the markers that can form the template.

## Parameters

### `PathOrTemplateContext3dmeasId` *(out, AIL_ID)*

Specifies the path or template 3D measurement context to which to add, or from which to delete, the path or template. The context must have been allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIND_MARKER_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md) or [`M_FIND_MARKER_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md).

### `Geometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object to use to define the path or template. The 3D geometry object must have been allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a line.

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix object to apply to the path or template.

### `PathOrTemplateIndex` *(in, AIL_INT)*

Specifies the index of the template or path in the 3D measurement context.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

*For specifying the type of operation*

| Value | Description |
| --- | --- |
| `M_ADD` | Specifies to add a path or template to the specified 3D measurement context. |
| `M_DELETE` | Specifies to delete the specified path or template from the specified 3D measurement context. |

### `MarkerType` *(in, AIL_INT)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `TransitionType` *(in, AIL_INT)*

Specifies the type of transition to find.

### `NumberMode` *(in, AIL_INT)*

Specifies the mode with which to determine the number of profiles that will be used to find the markers of a template.

### `LengthSource` *(in, AIL_INT)*

Specifies the source to use to define the length of the profiles.

### `LengthMode` *(in, AIL_INT)*

Specifies the mode with which to interpret the specified length ([`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md)).

### `NumberValue` *(in, AIL_INT)*

Specifies the number of profiles.

### `ParamArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the lengths of the profiles. If this information is not required, set this parameter to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For adding a path or template to the 3D measurement context

To add a path or template to the 3D measurement context, the [`PathOrTemplateContext3dmeasId`](../../Reference/3dmeas/M3dmeasDefine.md), [`Geometry3dgeoId`](../../Reference/3dmeas/M3dmeasDefine.md), [`Matrix3dgeoId`](../../Reference/3dmeas/M3dmeasDefine.md), [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasDefine.md), [`TransitionType`](../../Reference/3dmeas/M3dmeasDefine.md), [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md), [`LengthSource`](../../Reference/3dmeas/M3dmeasDefine.md), [`LengthMode`](../../Reference/3dmeas/M3dmeasDefine.md), [`NumberValue`](../../Reference/3dmeas/M3dmeasDefine.md), and [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md) parameters can be set to the following values. The [`Operation`](../../Reference/3dmeas/M3dmeasDefine.md) parameter must be set to [`M_ADD`](../../Reference/3dmeas/M3dmeasDefine.md).  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasDefine.md). Note, if unused, set [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md) to [`M_NULL`](../../Reference/3dmeas/M3dmeasDefine.md).

---

### `Path 3D measurement context identifier in which to define a path`

Specifies to add a path to a path 3D measurement context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE` *(default)* | Specifies to search for edge transitions. |
| `M_EDGE_OR_INVALID` | Specifies to search for edge transitions and invalid transitions. |
| `M_INVALID` | Specifies to search for invalid transitions. |

---

### `Template 3D measurement context identifier in which to define a template`

Specifies to add a template to a template 3D measurement context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE` *(default)* | Specifies to search for edge transitions. |
| `M_EDGE_OR_INVALID` | Specifies to search for edge transitions and invalid transitions. |
| `M_INVALID` | Specifies to search for invalid transitions. |
| `M_DEFAULT` |  |
| `M_SPACING` | Specifies that the number of profiles is based on the spacing between the profiles. The profiles are generated such that they are evenly spaced and span the entire template. Note that you must set the required spacing using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_TEMPLATE_PROFILE_SPACING_MODE`](../../Reference/3dmeas/M3dmeasControl.md). |
| `M_USER_DEFINED` *(default)* | Specifies that the number of profiles is explicitly set using the [`NumberValue`](../../Reference/3dmeas/M3dmeasDefine.md) parameter. |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies that the length of each profile is set individually. You must set the length of each profile using [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md).  Note that this value is only available when [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) is set to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasDefine.md). |
| `M_TEMPLATE` *(default)* | Specifies that the length of each profile is determined by a single value set for the template. You must set the length of the profiles using [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md). |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the length value directly, without applying a multiplying factor. For example, if you set the length value to 2.0, the profile length will be 2.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the length value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the length value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the length value by the pixel size in X. For example, if you set the length value to 2.0 and the pixel size in X is 0.5 mm, the profile length will be 1.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the length value by the pixel size in Y. For example, if you set the length value to 2.0 and the pixel size in Y is 0.6 mm, the profile length will be 1.2 mm. |
| `M_DEFAULT` | Specifies the default value.  If [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasDefine.md), specifies that the number value is not required.  If [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) is set to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasDefine.md), the default value is 10. |
| `Value > 0` | Specifies the number of profiles.  Note that this value is only available when [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) is set to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasDefine.md). |

### For deleting a path or template from the 3D measurement context

To delete a path or template from the 3D measurement context, the [`PathOrTemplateContext3dmeasId`](../../Reference/3dmeas/M3dmeasDefine.md), [`Geometry3dgeoId`](../../Reference/3dmeas/M3dmeasDefine.md), [`Matrix3dgeoId`](../../Reference/3dmeas/M3dmeasDefine.md), and [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasDefine.md) parameters can be set to the following values. The [`Operation`](../../Reference/3dmeas/M3dmeasDefine.md) parameter must be set to [`M_DELETE`](../../Reference/3dmeas/M3dmeasDefine.md).  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasDefine.md). Note, set [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md) to [`M_NULL`](../../Reference/3dmeas/M3dmeasDefine.md).

---

### `Path 3D measurement context identifier in which to remove a path`

Specifies to delete a path from a path 3D measurement context.

| Value | Description |
| --- | --- |
| `M_PATH_INDEX` | Specifies the path to delete from the path 3D measurement context. |

---

### `Template 3D measurement context identifier in which to remove a template`

Specifies to delete a template from a template 3D measurement context.

| Value | Description |
| --- | --- |
| `M_TEMPLATE_INDEX` | Specifies the template to delete from the template 3D measurement context. |

## Return Value

**Type:** `AIL_INT64`

Reserved for future expansion.
