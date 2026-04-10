---
doctype: Reference
module: 3dgra
function: M3dgraCopy
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraCopy"
---

# M3dgraCopy

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

> Copy an object into or out of a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraCopy(
    AIL_ID    SrcAilObjectId,  //in
    AIL_INT64 SrcLabel,        //in
    AIL_ID    DstAilObjectId,  //out
    AIL_INT64 DstLabel,        //in
    AIL_INT64 CopyType,        //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function copies an object into or out of a 3D graphics list.

You can copy a 3D graphic's geometry or transformation matrix, a point cloud graphic's LUT buffer, a 3D graphic or graphics list's bounding box, or a 3D graphics list's clipping box.

## Parameters

### `SrcAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the source object. Note that if the source object is not a 3D graphics list, the destination object must be.

### `SrcLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in [`SrcAilObjectId`](../../Reference/3dgra/M3dgraCopy.md), when the source object is a 3D graphics list.

### `DstAilObjectId` *(out, AIL_ID)*

Specifies identifier of the destination object. Note that if the destination object is not a 3D graphics list, the source object must be.

### `DstLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in [`DstAilObjectId`](../../Reference/3dgra/M3dgraCopy.md), when the destination object is a 3D graphics list.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type

---

### `M_BOUNDING_BOX`

Specifies to copy the bounding box of a 3D graphic (and, optionally, its descendants) into a 3D box geometry object, using the coordinate system of the 3D graphic's parent as a frame of reference.

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `M_LIST` | Specifies to use the entire contents of the 3D graphics list. This is equivalent to specifying the root node of the 3D graphics list using [`M_BOUNDING_BOX`](../../Reference/3dgra/M3dgraCopy.md) + [`M_RECURSIVE`](../../Reference/3dgra/M3dgraCopy.md). |
| `Value >= 0` | Specifies the label of the 3D graphic in the source 3D graphics list to use as the source object. The minimum bounding box for this 3D graphic is copied. |

---

### `M_CLIPPING_BOX`

Specifies to set the clipping box of the 3D graphics list, or copy a clipping box out of a 3D graphics list. The clipping box is used to clip 3D graphics with infinite size, such as infinite planes and cylinders, upon their creation.

#### `M_WHOLE_SCENE`

Specifies to set the destination 3D graphics list's entire bounding box as its clipping box.

| Value | Description |
| --- | --- |
| `M_LIST` | Specifies that the 3D graphics list itself is the destination object. |

#### `3D geometry object ID to use to define`

Specifies the identifier of the 3D box geometry object to use to set the clipping box of the 3D graphics list. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

| Value | Description |
| --- | --- |
| `M_LIST` | Specifies that the 3D graphics list itself is the destination object. |

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list from which to copy the clipping box. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `M_LIST` | Specifies that the 3D graphics list itself is the source object. |

---

### `M_COLOR_LUT`

Specifies to copy the LUT or to set the LUT associated with a point cloud graphic in a 3D graphics list.  Note that when setting a LUT, it is only used when the 3D graphics list is associated with a display and [`M_COLOR_USE_LUT`](../../Reference/3dgra/M3dgraControl.md) is set to [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md) (using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)). In this case, the values of the point cloud's component used for color are mapped to a color in the associated LUT. You can set the component used to color the point cloud graphic using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** The default LUT for a 3D graphics list is [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md); the LUT is only applied to point cloud graphics. To specify a color for other graphics, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

#### `M_NULL`

Specifies to remove the current LUT of a point cloud graphic in a 3D graphics list.

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to remove the default LUT of the 3D graphics list. This means that, if you subsequently add a point cloud graphic to the 3D graphics list and enable the default LUT, the graphic will be shown without a LUT. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_GRAYSCALE`

Specifies to use a predefined LUT buffer with a grayscale colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from black to white as the indices increase. Note that this colormap is effectively the same as no colormap ([`M_NULL`](../../Reference/3dgra/M3dgraCopy.md)), except that you can apply the [`M_FLIP`](../../Reference/3dgra/M3dgraCopy.md) combination value to invert the grayscale values.  *[Image: colormap_grayscale.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the grayscale colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_HOT`

Specifies to use a predefined LUT buffer with a hot colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from black, to red, to yellow, and then, to white along the RGB cube as the indices increase. This colormap can be useful for displaying infrared images.  *[Image: Hot_colormap.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the hot colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_HUE`

Specifies to use a predefined LUT buffer with a hue colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap is cyclic and transitions from red, to yellow, to green, to cyan, to blue, to magenta, and then, to red along the edge of the hue circle as the indices increase. This colormap is useful for displaying the hue component of an HSL image.  *[Image: Hue_colormap.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the hue colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_JET`

Specifies to use a predefined LUT buffer with a jet colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from dark blue, to blue, to cyan, to yellow, to red, and then, to dark red along the RGB cube as the indices increase. This colormap can be useful for displaying 3D elevation maps and thermal imaging.  *[Image: Jet_colormap.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the jet colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_SPECTRUM`

Specifies to use a predefined LUT buffer with a spectrum colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from red, to yellow, to green, to blue, and then, to violet along the RGB cube as the indices increase. This colormap is a representation of the visual spectrum according to wavelengths. It can also be useful for displaying 3D elevation maps and thermal imaging.  *[Image: spectrum_colormap.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the spectrum colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_TURBO`

Specifies to use a predefined LUT buffer with a turbo colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from dark blue, to blue, to cyan, to yellow, to red, and then, to dark red along the RGB cube as the indices increase. This colormap is similar to [`M_COLORMAP_JET`](../../Reference/3dgra/M3dgraCopy.md), but with smoother transitions.  *[Image: Turbo_colormap.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the turbo colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `M_COLORMAP_WHEEL`

Specifies to use a predefined LUT buffer with a colorwheel colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap is cyclic and transitions from blue, to magenta, to yellow, to green, and then, to blue again as the indices increase. This colormap is designed to be perceptually uniform (a given change in data is designed to be visually perceived as an equal change in color). This colormap can be useful for displaying 3D elevation maps.  *[Image: Wheel_colormap.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the colorwheel colormap. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list containing the point cloud graphic from which to copy the LUT. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to use the 3D graphics list's default LUT. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

#### `LUT buffer ID to use to define`

Specifies the identifier of the LUT buffer from which to copy the LUT. The LUT buffer must have been previously allocated using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md)with [`M_LUT`](../../Reference/buf/MbufAllocColor.md). Only 8-bit unsigned LUT buffers are supported.

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to set the 3D graphics list's default LUT to the specified source LUT buffer. |
| `Value >= 0` | Specifies the label of the point cloud graphic. Label 0 is the 3D graphics list's root node. |

---

### `M_COLOR_TEXTURE`

Specifies to copy or set the texture of a polygon graphic in a 3D graphics list.

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list containing the polygon graphic from which to copy the texture. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).  > **Note:** Note that using this copy type with a polygon graphic that does not have a texture will cause an error.

#### `Image buffer ID to use to define`

Specifies the identifier of the image buffer to use to set a polygon graphic's texture. The image buffer must be an 8-bit, unsigned, 1- or 3-band buffer. If the polygon graphic does not already have a texture, Aurora Imaging Library sets the texture mapping automatically. If the polygon graphic already has a texture, the image buffer must have the same size and number of bands as the texture being replaced. Otherwise, an error is generated.

---

### `M_GEOMETRY`

Specifies to copy a 3D graphic's geometry into a 3D geometry object, using its parent's coordinate system as a frame of reference. The 3D graphic must be of type box, cylinder, line, plane, or sphere; point geometries are not supported.

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list containing the 3D graphic. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).

---

### `M_GRAPHIC`

Specifies to copy a 3D graphic from a source 3D graphics list into a destination 3D graphics list. If the combination value [`M_CHILDREN_ONLY`](../../Reference/3dgra/M3dgraCopy.md) is not specified, [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) returns the label of the 3D graphic copied into the destination 3D graphics list.

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list containing the 3D graphic. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).

---

### `M_TRANSFORMATION_MATRIX`

Specifies to copy or set the transformation matrix of a 3D graphic.

#### `M_IDENTITY_MATRIX`

Specifies to reset the transformation matrix of the 3D graphic to the identity matrix.

#### `3D graphics list ID from which to copy`

Specifies the identifier of the 3D graphics list containing the 3D graphic from which to copy the transformation matrix. The 3D graphics list must either be associated with a 3D display, or must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). You can inquire the identifier of the 3D graphics list associated with a display using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md).

#### `Transformation matrix object ID to use to define`

Specifies the identifier of the transformation matrix object to use as a 3D graphic's transformation matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be of type [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md).

### Combination Constants — For setting a LUT recursively

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the LUT of a point cloud graphic in the destination 3D graphics list, and all of its point cloud graphic children in the 3D graphics list recursively.

| Value | Description |
| --- | --- |
| `M_RECURSIVE` | Sets the LUT of the point cloud graphic specified by the [`DstLabel`](../../Reference/3dgra/M3dgraCopy.md) parameter, and all of its point cloud graphic children in the 3D graphics list recursively. |

### Combination Constants — For specifying how to copy the 3D graphic

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify how to copy the 3D graphic.

| Value | Description |
| --- | --- |
| `M_CHILDREN_ONLY` | Specifies to copy the 3D graphic's descendants into the destination 3D graphics list, without copying the 3D graphic itself. The 3D graphic's children are directly copied into the destination 3D graphics list as children of the 3D graphic with label [`M_GRAPHIC`](../../Reference/3dgra/M3dgraCopy.md), and the rest of the descendants are copied recursively.  > **Note:** Note that in this case, [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) returns [`M_NO_LABEL`](../../Reference/3dgra/M3dgraCopy.md). |
| `M_RECURSIVE` | Specifies to copy the 3D graphic and all of its descendants in the 3D graphics list recursively. |

### Combination Constants — For specifying how to copy the 3D graphic's geometry

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify how to copy the geometry.

| Value | Description |
| --- | --- |
| `M_RELATIVE_TO_ROOT` | Specifies to copy the geometry using its relative position and orientation to the 3D graphics list's root node. |

### Combination Constants — For specifying how to copy the transformation matrix

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify how to copy the transformation matrix.

| Value | Description |
| --- | --- |
| `M_COMPOSE_WITH_CURRENT` | Specifies to compose the 3D graphic object's transformation matrix with the source transformation matrix. The [`DstAilObjectId`](../../Reference/3dgra/M3dgraCopy.md) parameter must be a 3D graphics list identifier. |
| `M_RELATIVE_TO_ROOT` | Specifies to copy or set the position and orientation of a 3D graphic relative to the 3D graphics list's root node. |

### Combination Constants — For specifying how to copy the bounding box

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify how to copy the bounding box.

| Value | Description |
| --- | --- |
| `M_RECURSIVE` | Specifies to copy the bounding box of the 3D graphic and all of its descendants in the 3D graphics list. |
| `M_RELATIVE_TO_ROOT` | Specifies to copy the bounding box of the 3D graphic relative to the 3D graphics list's root node. |

### Combination Constants — For specifying to flip the LUT

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the LUT should be flipped.

| Value | Description |
| --- | --- |
| `M_FLIP` | Specifies to flip the LUT's values (inverse LUT). |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the 3D graphic copied into the destination 3D graphics list. If the copy type does not return a new label in the destination 3D graphics list, this function returns `M_NO_LABEL`.
