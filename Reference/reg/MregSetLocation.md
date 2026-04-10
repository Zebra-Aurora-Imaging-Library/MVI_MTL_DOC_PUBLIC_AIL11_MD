---
doctype: Reference
module: reg
function: MregSetLocation
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregSetLocation"
---

# MregSetLocation

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

> Sets the rough location of the specified registration element's image with respect to another registration element's image or the global pixel coordinate system.

## Syntax

```c
void MregSetLocation(
    AIL_ID     ContextId,   //out
    AIL_INT    Index,       //in
    AIL_INT    Target,      //in
    AIL_INT64  ParamType,   //in
    AIL_DOUBLE Param1,      //in
    AIL_DOUBLE Param2,      //in
    AIL_DOUBLE Param3,      //in
    AIL_DOUBLE Param4,      //in
    AIL_INT64  ControlFlag  //in
)
```

## Description

This function sets the rough location of the specified registration element's image with respect to another registration element's image or the global pixel coordinate system. The [`MregCalculate`](../../Reference/reg/MregCalculate.md) function uses this location to find the transformations that optimally position the images in the global pixel coordinate system.

You can explicitly specify the rough location or you can copy it from a registration element of another registration context or a result element of a registration result buffer. When copying the rough location, you can use the same reference index as the element from which you are copying the rough location. Alternatively, you can specify a different reference index.

The more precise the information, the faster the registration calculation will be. If you know the exact position of the image, you can bypass the optimization step of the calculation by either disabling the [`MregControl`](../../Reference/reg/MregControl.md)   [`M_OPTIMIZE_LOCATION`](../../Reference/reg/MregControl.md) control type or replacing the image in the array by **M_NULL** (see [Skipping the optimization step](../../UserGuide/C12_Correlation_stitching_registration/S07_Customizing_your_correlation_stitching_registration_settings.md)). In this case, the transformation that you specify using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) becomes the optimal transformation. Upon calling [`MregCalculate`](../../Reference/reg/MregCalculate.md), this transformation will be converted so that it maps the image into the global pixel coordinate system without first converting it into its reference image's pixel coordinate system.

Some restrictions apply when selecting the image's reference. The settings cannot be circularly defined. If you take a specific image and look at its reference, and then at its reference image's reference, and continue through the series of images linked in this manner, the first image must never appear as another image's reference. Furthermore, there should only be one image with its reference set to the global pixel coordinate system.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the registration context of the registration element to affect. The registration context must have been previously allocated on the required system using [`MregAlloc`](../../Reference/reg/MregAlloc.md).

### `Index` *(in, AIL_INT)*

Specifies the index of the registration element. This parameter can be set to one of the following:

*For specifying the index*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that all of the registration elements will have their location set. |
| `0 <= Value < Number of source elements` | Specifies the index of the registration element. |

### `Target` *(in, AIL_INT)*

Specifies the reference of the registration element's image. The coordinate system associated with this reference is used as the reference coordinate system.

*For specifying the reference*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COPY` | Uses the same index as the reference of the element from which positional information is being copied. This setting is only available if the [`ParamType`](../../Reference/reg/MregSetLocation.md) parameter is set to either [`M_COPY_REG_CONTEXT`](../../Reference/reg/MregSetLocation.md) or [`M_COPY_REG_RESULT`](../../Reference/reg/MregSetLocation.md). |
| `M_NEXT` | Specifies the reference to be the image associated with the registration element whose index follows the specified registration element's index. When the registration element's image is the last one in the input sequence, the reference is set to [`M_REGISTRATION_GLOBAL`](../../Reference/reg/MregSetLocation.md), the global pixel coordinate system. |
| `M_PREVIOUS` *(default)* | Specifies the reference to be the image associated with the registration element whose index precedes the specified registration element's index. When the registration element has an index 0, the reference is set to [`M_REGISTRATION_GLOBAL`](../../Reference/reg/MregSetLocation.md), the global pixel coordinate system. |
| `M_REGISTRATION_GLOBAL` | Specifies that the reference is the global pixel coordinate system. |
| `M_UNCHANGED` | Specifies that only the rough locations are changed; the same reference is used. |
| `0 <= Value < Number of source elements` | Specifies the index of the registration element of the reference image. |

### `ParamType` *(in, AIL_INT64)*

Specifies the type of transformation to use to set the location of the registration element's image.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition depends on the value of [`ParamType`](../../Reference/reg/MregSetLocation.md).

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition depends on the value of [`ParamType`](../../Reference/reg/MregSetLocation.md).

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition depends on the value of [`ParamType`](../../Reference/reg/MregSetLocation.md).

### `Param4` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the transformation type

---

### `M_COPY_REG_CONTEXT`

Specifies that the rough locations are copied from another registration context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default index. The default index is the same as the one specified for the [`Index`](../../Reference/reg/MregSetLocation.md) parameter.  If the [`Index`](../../Reference/reg/MregSetLocation.md) parameter is set to [`M_ALL`](../../Reference/reg/MregSetLocation.md), the function copies each rough location from the source context into the corresponding index in the destination context. In this case, the number of elements in the source registration context passed to [`Param1`](../../Reference/reg/MregSetLocation.md) must be greater than or equal to the number of elements in the destination registration context. Any extra elements in the source context will not be copied. |
| `0 <= Value < Number of source elements` | Specifies the registration element's index. |

---

### `M_COPY_REG_RESULT`

Specifies that the transformation between two registration result elements is copied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default index. The default index is the same as the one specified for the [`Index`](../../Reference/reg/MregSetLocation.md) parameter. |
| `0 <= Value < Number of source elements` | Specifies the registration result element's index. |
| `M_DEFAULT` | Specifies that the target registration result element's index is the same as the one that is specified in the [`Target`](../../Reference/reg/MregSetLocation.md) parameter. |
| `M_REGISTRATION_GLOBAL` | Specifies that the target registration result element's reference is the global pixel coordinate system. |
| `Value >= 0` | Specifies the target registration result element's index. |

---

### `M_POSITION_XY`

Specifies the coordinates of the origin of the registration element's image in the coordinate system of its reference. Note that when the reference is an image, the specified coordinates need not lie within the reference image.

---

### `M_POSITION_XY_ANGLE`

Specifies the coordinates of the origin and the angle of the registration element's image in the coordinate system of its reference. Note that when the reference is an image, the specified coordinates need not lie within the reference image.

---

### `M_WARP_4_CORNER`

Specifies the transformation that transforms an arbitrary quadrilateral in the current image's pixel coordinate system into a rectangle in its reference coordinate system. You specify the transformation by supplying 4 points in the current image and 2 points in the reference image.

---

### `M_WARP_4_CORNER_REVERSE`

Specifies the transformation that transforms a rectangle in the current image's pixel coordinate system into an arbitrary quadrilateral in its reference coordinate system. You specify the transformation by supplying 2 points in the current image and 4 points in the reference image.

---

### `M_WARP_POLYNOMIAL`

Specifies the transformation matrix that transforms points in the reference coordinate system into points in the current image's pixel coordinate system.
