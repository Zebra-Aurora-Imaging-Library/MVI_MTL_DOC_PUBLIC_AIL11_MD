---
doctype: Reference
module: met
function: MmetSetRegion
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetSetRegion"
---

# MmetSetRegion

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

> Set the metrology region that delimits the area from which to establish the feature.

## Syntax

```c
void MmetSetRegion(
    AIL_ID     ContextId,                   //out
    AIL_INT    FeatureLabelOrIndex,         //in
    AIL_INT    ReferenceFrameLabelOrIndex,  //in
    AIL_INT64  Geometry,                    //in
    AIL_DOUBLE Param1,                      //in
    AIL_DOUBLE Param2,                      //in
    AIL_DOUBLE Param3,                      //in
    AIL_DOUBLE Param4,                      //in
    AIL_DOUBLE Param5,                      //in
    AIL_DOUBLE Param6                       //in
)
```

## Description

This function sets a geometrically-defined ROI, referred to as a metrology region, for a feature within the template of the metrology context. For a physically measured feature, the metrology region delimits the area in the target image from which to establish the edgels of the feature to validate and measure. For a constructed feature, the metrology region delimits the area in the feature itself, which is used as the base feature to build the constructed feature.

When you call this function, you can define the metrology region in one of the following ways.

- Explicitly specify the shape and size of the region. This is referred to as an explicitly-defined metrology region.
- Set the region's shape and size using a graphic in a 2D graphics list. This is referred to as a 2D graphics list metrology region.
- Define the metrology region based on other features by passing a derived metrology region object to this function. This is referred to as a derived metrology region.

When using a 2D graphics list metrology region ([`M_FROM_GRAPHIC_LIST`](../../Reference/met/MmetSetRegion.md)), the 2D graphics list must contain only one graphic and it must have the same geometric-shape as specified with the [`Geometry`](../../Reference/met/MmetSetRegion.md) parameter of this function. The advantage of defining a 2D graphics list metrology region is that you can set it with respect to the relative coordinate system as well as set it interactively.

When using a derived metrology region ([`M_FROM_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetSetRegion.md)), you must allocate a derived metrology region object using [`MmetAlloc`](../../Reference/met/MmetAlloc.md) with [`M_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetAlloc.md), and then set it up using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_REGION_...`](../../Reference/met/MmetControl.md). Once you specify the geometry of the derived metrology region using the [`M_REGION_GEOMETRY`](../../Reference/met/MmetControl.md) control type, you can set most of the geometry's attributes either explicitly or derive them based on a feature. Use the corresponding [`M_REGION_..._TYPE`](../../Reference/met/MmetControl.md) control type to specify which. For example, instead of specifying explicit X- and Y-coordinates to set the position of a rectangular metrology region, you can derive its position according to the position of a point feature.

The metrology region that you choose must be compatible with the geometry and operation of the feature you are adding, with [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md). For example, to add a physically measured circle feature that is built using a best fit operation, you can use an [`M_RING`](../../Reference/met/MmetSetRegion.md) metrology region.

The metrology region is set according to its feature's reference frame ([`ReferenceFrameLabelOrIndex`](../../Reference/met/MmetSetRegion.md)). The global frame is the default. If the target image is not calibrated, the global frame's default origin (0,0) is aligned with the center of the top-left corner pixel of the target image. If the target image is calibrated, the global frame's default origin is aligned with the origin of the relative (world) coordinate system. To change the reference frame to a local frame, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_REFERENCE_FRAME`](../../Reference/met/MmetControl.md). Local frames are coordinate systems located anywhere within the metrology template and defined as metrology features.

A valid feature must not only fall within the metrology region, but its edgels' gradient angle must also fall along the region's orientation. Various control types can be used to set a valid relationship between the metrology region's orientation, and the edgels' gradient angle. For more information, see [Gradient angle](../../UserGuide/C21_Metrology/S06_Eliminating_unwanted_edgels.md).

> **Note:** Note that you cannot specify a geometrically-defined region for a feature with a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md); you can only use metrology regions established with this function.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the identifier of the metrology context containing the template in which to set the metrology region. The metrology context must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `FeatureLabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the feature that will be established from the area delimited by the metrology region. Set this parameter to one of the following values.

*For specifying a feature*

| Value | Description |
| --- | --- |
| `M_FEATURE_INDEX` | Specifies the index of an existing individual feature. |
| `M_FEATURE_LABEL` | Specifies the label value of an existing individual feature. |

### `ReferenceFrameLabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the reference frame feature of the feature for which you are specifying a metrology region. To change the reference frame, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_REFERENCE_FRAME`](../../Reference/met/MmetControl.md).

*For specifying the reference frame*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_GLOBAL_FRAME`](../../Reference/met/MmetSetRegion.md). |
| `M_FEATURE_INDEX` | Specifies the index value of an existing reference frame feature. |
| `M_FEATURE_LABEL` | Specifies the label value of an existing reference frame feature. |
| `M_GLOBAL_FRAME` | Specifies the global frame as the reference frame. |

### `Geometry` *(in, AIL_INT64)*

Specifies the geometric shape of the metrology region.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the metrology region to set. Its definition is dependent on the geometry chosen.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the metrology region to set. Its definition is dependent on the geometry chosen.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the metrology region to set. Its definition is dependent on the geometry chosen.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the metrology region to set. Its definition is dependent on the geometry chosen.

### `Param5` *(in, AIL_DOUBLE)*

Specifies an attribute of the metrology region to set. Its definition is dependent on the geometry chosen.

### `Param6` *(in, AIL_DOUBLE)*

Specifies an attribute of the metrology region to set. Its definition is dependent on the geometry chosen.

## Parameter Associations

### For setting an explicitly-defined or 2D graphics list metrology region

The following can be specified for setting an explicitly-defined metrology region. To specify a 2D graphics list metrology region, you must add [`M_FROM_GRAPHIC_LIST`](../../Reference/met/MmetSetRegion.md) to the supported geometry ([`M_ARC`](../../Reference/met/MmetSetRegion.md), [`M_RECTANGLE`](../../Reference/met/MmetSetRegion.md), and [`M_SEGMENT`](../../Reference/met/MmetSetRegion.md)).  Unused parameters should be set to [`M_NULL`](../../Reference/met/MmetSetRegion.md).

---

### `M_DEFAULT`

Same as [`M_INFINITE`](../../Reference/met/MmetSetRegion.md).

---

### `M_ARC`

Specifies that the feature will be established from an arc metrology region.  *[Image: MetRoiArc.png]*  [`M_ARC`](../../Reference/met/MmetSetRegion.md) can be used to establish point ([`M_POINT`](../../Reference/met/MmetAddFeature.md)) features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's Y-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the radius, in pixel or world units. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_INFINITE`

Specifies that the feature will be established from an infinite metrology region. An infinite metrology region is an X- Y-plane with no boundaries. Use [`Param3`](../../Reference/met/MmetSetRegion.md) to specify the angle that defines the orientation of this region. Note that [`M_INFINITE`](../../Reference/met/MmetSetRegion.md) is similar to [`M_RECTANGLE`](../../Reference/met/MmetSetRegion.md), however a rectangular metrology region has a width and a height, which are used to bind the metrology region.  [`M_INFINITE`](../../Reference/met/MmetSetRegion.md) can be used to establish any feature and is the default metrology region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's Y-coordinate, in pixel or world units. |
| `M_DEFAULT` | Specifies the default value; the default value is 0.0°. This indicates that the orientation of the infinite metrology region is the same as the orientation of the reference frame. |
| `M_RADIAL` | Specifies that the infinite metrology region has a radial orientation, from 0.0° to 360.0°, based on the specified origin.  *[Image: InfiniteRadialRegion1.png]*  In such a metrology region, edgels can only be considered part of the feature if their gradient angle conforms to the direction of a radii pointing out from the region's origin. [`M_RADIAL`](../../Reference/met/MmetSetRegion.md) is usually used to define a metrology region for a physically measured circular feature whose radius is unknown. Typically the origin of the metrology region is placed at the feature's center, resulting in valid edgels only being possible if they encircle the origin. |
| `0.0 <= Value <= 360.0` | Specifies the angle, in degrees, relative to the reference frame. This angle represents the infinite region's orientation. |

---

### `M_RECTANGLE`

Specifies that the feature will be established from a rectangular metrology region.  *[Image: MetRoiRectangle.png]*  Note that [`M_RECTANGLE`](../../Reference/met/MmetSetRegion.md) is similar to [`M_INFINITE`](../../Reference/met/MmetSetRegion.md), however an infinite metrology region does not have a width and a height (boundaries).  [`M_RECTANGLE`](../../Reference/met/MmetSetRegion.md) can be used to establish segment ([`M_SEGMENT`](../../Reference/met/MmetAddFeature.md)) or edgel ([`M_EDGEL`](../../Reference/met/MmetAddFeature.md)) features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's Y-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the rectangle's width, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the rectangle's height, in pixel or world units. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_RING`

Specifies that the feature will be established from a ring-shaped metrology region.  *[Image: MetRoiRing.png]*  [`M_RING`](../../Reference/met/MmetSetRegion.md) can be used to establish circle ([`M_CIRCLE`](../../Reference/met/MmetAddFeature.md)) or edgel ([`M_EDGEL`](../../Reference/met/MmetAddFeature.md)) features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's Y-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start radius, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end radius, in pixel or world units. |

---

### `M_RING_SECTOR`

Specifies that the feature will be established from a ring-sector metrology region.  *[Image: metroiringsector.png]*  [`M_RING_SECTOR`](../../Reference/met/MmetSetRegion.md) can be used to establish radial segments ([`M_SEGMENT`](../../Reference/met/MmetAddFeature.md)), arcs ([`M_ARC`](../../Reference/met/MmetAddFeature.md)), or edgel ([`M_EDGEL`](../../Reference/met/MmetAddFeature.md)) features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the origin's Y-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start radius, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end radius, in pixel or world units. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_SEGMENT`

Specifies that the feature will be established from a linear segment metrology region.  *[Image: MetRoiSegment.png]*  [`M_SEGMENT`](../../Reference/met/MmetAddFeature.md) can be used to establish point ([`M_POINT`](../../Reference/met/MmetAddFeature.md)) features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start point's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start point's Y-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end point's X-coordinate, in pixel or world units. |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end point's Y-coordinate, in pixel or world units. |

### Combination Constants — For setting a 2D graphics list metrology region

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set a 2D graphics list metrology region.

Unused parameters should be set to [`M_NULL`](../../Reference/met/MmetSetRegion.md).

#### `M_FROM_GRAPHIC_LIST`

Specifies a 2D graphics list metrology region. Interactive graphics are allowed. Metrology ignores a graphic's fill color, if specified.  When using [`M_FROM_GRAPHIC_LIST`](../../Reference/met/MmetSetRegion.md) with [`M_ARC`](../../Reference/met/MmetSetRegion.md), the 2D graphics list must contain only one arc, created using [`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md), or [`MgraArcFill`](../../Reference/gra/MgraArcFill.md). The arc must be circular; that is, its X- and Y-radius must be the same.  When using [`M_FROM_GRAPHIC_LIST`](../../Reference/met/MmetSetRegion.md) with [`M_RECTANGLE`](../../Reference/met/MmetSetRegion.md), the 2D graphics list must contain only one rectangle, created using [`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), or [`MgraRectFill`](../../Reference/gra/MgraRectFill.md).  When using [`M_FROM_GRAPHIC_LIST`](../../Reference/met/MmetSetRegion.md) with [`M_SEGMENT`](../../Reference/met/MmetSetRegion.md), the 2D graphics list must contain only one line, created using [`MgraLine`](../../Reference/gra/MgraLine.md).

### For setting a derived metrology region

The following can be specified for setting a derived metrology region.  Unused parameters should be set to [`M_NULL`](../../Reference/met/MmetSetRegion.md).

---

### `M_FROM_DERIVED_GEOMETRY_REGION`

Specifies a derived metrology region. To set up this metrology region, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_REGION_...`](../../Reference/met/MmetControl.md).  It is only recommended to use a derived metrology region when you need at least one other feature to set the metrology region. However, you can use a derived metrology region to set the metrology region using features, explicit values, or a combination of the two. Once you define this type of metrology region, the region information (not the derived metrology region object itself) is considered part of the metrology context with which it is used; for example, that information will be saved along with the context if you call [`MmetSave`](../../Reference/met/MmetSave.md).  You can use [`M_FROM_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetSetRegion.md) with any geometry.
