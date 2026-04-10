---
doctype: Reference
module: 3dgra
function: M3dgraInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraInquire"
---

# M3dgraInquire

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

> Inquire information about a 3D graphic in a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraInquire(
    AIL_ID    List3dgraId,  //in
    AIL_INT64 Label,        //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires information about a setting of the specified 3D graphic in a 3D graphics list.

## Parameters

### `List3dgraId` *(in, AIL_ID)*

Specifies the identifier of the 3D graphics list containing the 3D graphic about which to inquire information. You can inquire the settings of a 3D graphic in the 3D graphics list, as well as the default settings of the 3D graphics list itself. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `Label` *(in, AIL_INT64)*

Specifies the label of the 3D graphic to inquire. You can also inquire the default settings of the 3D graphics list itself.

*For specifying the 3D graphic label*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to inquire the default settings of the 3D graphics that are added to the 3D graphics list. These settings will not be applied to the 3D graphics that are already in the 3D graphics list. |
| `M_LIST` | Specifies to inquire the 3D graphics list itself. |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For inquiring about the 3D graphics list

The following inquire types allow you to inquire about the 3D graphics list when [`Label`](../../Reference/3dgra/M3dgraInquire.md) is [`M_LIST`](../../Reference/3dgra/M3dgraInquire.md). These inquire types inquire settings that affect all graphics in the graphics list.

---

### `Graphics list identifier with Label set to M_LIST`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md).

#### `M_EDITABLE_APPEARANCE`

Inquires whether the interactively editable graphic is displayed such that it appears as a solid surface, wireframe, or points. This is the appearance applied to a graphic when [`M_EDITABLE`](../../Reference/3dgra/M3dgraInquire.md) is [`M_ENABLE`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_COLOR`

Inquires the default color of the points and lines of the interactively editable graphics added to the 3D graphics list. This is the color applied to a graphic when [`M_EDITABLE`](../../Reference/3dgra/M3dgraInquire.md) is [`M_ENABLE`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_OPACITY`

Inquires the default opacity of the interactively editable graphics added to the 3D graphics list. This is the opacity applied to a graphic when [`M_EDITABLE`](../../Reference/3dgra/M3dgraInquire.md) is [`M_ENABLE`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EMPTY`

Inquires whether the 3D graphics list contains no displayable 3D graphics. Invisible 3D graphics include nodes, 3D graphics with [`M_VISIBLE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_FALSE`](../../Reference/3dgra/M3dgraControl.md), and containers with no valid points.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the 3D graphics list contains displayable 3D graphics. |
| `M_TRUE` | Specifies that the 3D graphics list does not contain any displayable 3D graphics. |

### For inquiring about the default settings of 3D graphics added to the 3D graphics list

The following inquire types allow you to inquire about the default settings of a 3D graphics list when [`Label`](../../Reference/3dgra/M3dgraInquire.md) is [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraInquire.md).

---

### `Graphics list identifier with Label set to M_DEFAULT_SETTINGS`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraInquire.md) specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the 3D graphics added to the 3D graphics list are displayed, by default, as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_BACKGROUND_COLOR`

Inquires the default background color for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_BACKGROUND_MODE`

Inquires the default background mode for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the default color of the points and lines of the 3D graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_AXIS_X`

Inquires the default color of the X-axis of axis graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_AXIS_X`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_AXIS_Y`

Inquires the default color of the Y-axis of axis graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_AXIS_Y`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_AXIS_Z`

Inquires the default color of the Z-axis of axis graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_AXIS_Z`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_COMPONENT`

Inquires the default component used to color the points in the point cloud graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_COMPONENT`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_NULL` | Specifies that the color of the points in the point cloud graphic is set to a single color using [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_COLOR_COMPONENT_BAND`

Inquires the default band used to color the point cloud graphic when the point cloud graphic's color is set using [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_COMPONENT_BAND`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS`

Inquires the default limits of the values in the component used to color the point cloud graphics added to the 3D graphics list when the point cloud graphics are colored using [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) by default.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS_MAX`

Inquires the default maximum color value for point cloud graphics added to the 3D graphics list when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraInquire.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS_MAX`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS_MIN`

Inquires the default minimum color value for point cloud graphics added to the 3D graphics list when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraInquire.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS_MIN`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LUT_ID`

Inquires the identifier of the default LUT buffer associated with the 3D graphics list. This is the LUT that is applied by default to point cloud graphics added to the list. If the default LUT is predefined (default [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md)), Aurora Imaging Library returns the type of predefined colormap to which the LUT is set ([`M_COLORMAP_...`](../../Reference/3dgra/M3dgraCopy.md)). If the default LUT is user-defined, the identifier of an internal copy of the user-defined LUT is returned.  For a user-defined LUT for which an internal copy exists, you must not free the associated buffer. You can, however, modify the buffer so that newly added point cloud graphics receive the modified LUT. Note that modifying the 3D graphics list's default LUT buffer will not change the LUT for point cloud graphics that are already in the 3D graphics list; each point cloud graphic gets its own copy of the LUT associated with the 3D graphics list when it is created.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the LUT buffer is set to [`M_NULL`](../../Reference/3dgra/M3dgraCopy.md). |
| `LUT buffer identifier` | Specifies the Aurora Imaging Library identifier of the current LUT buffer. |

#### `M_COLOR_LUT_SIZE`

Inquires the size of the point cloud graphic's default LUT buffer.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the size of the point cloud graphic's LUT. |

#### `M_COLOR_LUT_SIZE_BAND`

Inquires the number of bands of the point cloud graphic's default LUT buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies that the point cloud graphic's LUT has one band. |
| `3` | Specifies that the point cloud graphic's LUT has three bands. |

#### `M_COLOR_USE_LUT`

Inquires whether the point cloud graphics added to the 3D graphics list are colored, by default, using a LUT to map values from the component specified by [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) to a color.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_USE_LUT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_USE_TEXTURE`

Inquires whether the polygon graphics added to the 3D graphics list are filled with a texture by default. If the polygon graphic has no texture buffer, [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md) is used regardless of this setting.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_USE_TEXTURE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_FALSE` | Specifies to fill the polygon with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |

#### `M_DISPLAY_BASES`

Inquires whether the cylinder graphic's circular bases are displayed or not by default when a cylinder graphic is added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_DISPLAY_BASES`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FILL_COLOR`

Inquires the default color of the solid surfaces of the 3D graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FIXED_FONT_SIZE`

Inquires the default font size in pixels for text graphics with fixed scaling ([`M_FIXED_SCALE`](../../Reference/3dgra/M3dgraText.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the height of the font in pixels. |

#### `M_FONT`

Inquires the default font for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_FONT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT_AUTO_SELECT`

Inquires the default automatic font selection behavior for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_FONT_AUTO_SELECT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT_SIZE`

Inquires the default font size for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_FONT_SIZE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_GRAPHIC_RESOLUTION`

Inquires the default mesh resolution of 3D graphics added to the 3D graphics list. This applies to arc, filled arc, axis, cylinder, and sphere graphics.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_RESOLUTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_KEYING_COLOR`

Inquires the default pixel value to display as transparent for polygon graphics added to the 3D graphics list.  This setting is only used if the polygon is filled with a texture buffer. Any portion of the polygon graphic that would be displayed with this color is instead made transparent, regardless of the polygon graphic's [`M_OPACITY`](../../Reference/3dgra/M3dgraInquire.md)setting. This can be used to cut out parts of a polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_KEYING_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the default opacity of the 3D graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the 3D graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the default shading of the 3D graphics added to the 3D graphics list. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.  > **Note:** This setting does not apply to text, node, line, arc, filled arc, grid, or dots graphics. You can inquire the default shading mode for text graphics using [`M_TEXT_SHADING`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_ALIGN_HORIZONTAL`

Inquires the default horizontal alignment of the text in the text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_ALIGN_HORIZONTAL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_ALIGN_VERTICAL`

Inquires the default vertical alignment of the text in the text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_ALIGN_VERTICAL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_BORDER`

Inquires the default borders around text graphics added to the 3D graphics list.  Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific border.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_BORDER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_DIRECTION`

Inquires the default direction to draw text the text that is displayed in a text graphic that is added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_DIRECTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_SHADING`

Inquires the default shading of the text graphics added to the 3D graphics list. Shaded text graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the default thickness of the points/lines of the 3D graphics added to the 3D graphics list in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD`

Inquires whether to use different levels of detail (LoDs) based on the display view. This improves performance when the display selects a lower LoD.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_LEVELS`

Inquires how many LoDs to generate if [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraInquire.md)is set to [`M_ENABLE`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_LEVELS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`

Inquires the maximum factor by which to resample the point cloud when regenerating the LoDs.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_SAMPLE_MODE`

Inquires what type of sub-sampling to perform when generating LoDs.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_SAMPLE_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the default visibility of the 3D graphics added to the 3D graphics list. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

### For inquiring general information about a 3D graphic or 3D graphic label

The following inquire types allow you to inquire general information about a 3D graphic or 3D graphic label.

---

### `Graphics list identifier with Label set to any 3D graphic label`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a 3D graphic label specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_CHILDREN`

Inquires the labels of the 3D graphic's immediate children.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of the 3D graphic's child. |

#### `M_GRAPHIC_TYPE`

Inquires the type of 3D graphic.

| Value | Description |
| --- | --- |
| `M_GRAPHIC_TYPE_ARC` | Specifies an arc graphic. |
| `M_GRAPHIC_TYPE_ARC_FILL` | Specifies a filled arc graphic. |
| `M_GRAPHIC_TYPE_AXIS` | Specifies an axis graphic. |
| `M_GRAPHIC_TYPE_BOX` | Specifies a box graphic. |
| `M_GRAPHIC_TYPE_CYLINDER` | Specifies a cylinder graphic. |
| `M_GRAPHIC_TYPE_DOTS` | Specifies dots graphic. |
| `M_GRAPHIC_TYPE_GRID` | Specifies a grid graphic. |
| `M_GRAPHIC_TYPE_LINE` | Specifies a line graphic. |
| `M_GRAPHIC_TYPE_LINES` | Specifies a lines graphic. |
| `M_GRAPHIC_TYPE_NODE` | Specifies a node graphic. |
| `M_GRAPHIC_TYPE_PLANE` | Specifies a plane graphic. |
| `M_GRAPHIC_TYPE_POINT_CLOUD` | Specifies a point cloud graphic. |
| `M_GRAPHIC_TYPE_POLYGON` | Specifies a polygon graphic. |
| `M_GRAPHIC_TYPE_RECT` | Specifies a rectangle graphic. |
| `M_GRAPHIC_TYPE_SPHERE` | Specifies a sphere graphic. |
| `M_GRAPHIC_TYPE_TEXT` | Specifies text graphic. |

#### `M_LABEL_EXISTS`

Inquires whether the specified label exists in the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the label does not exist in the 3D graphics list. |
| `M_TRUE` | Specifies that the label exists in the 3D graphics list. |

#### `M_NUMBER_OF_CHILDREN`

Inquires the 3D graphic's number of immediate children. Use with [`M_RECURSIVE`](../../Reference/3dgra/M3dgraInquire.md) to get the 3D graphic's total number of descendants.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of immediate children. |

#### `M_PARENT`

Inquires the label of the 3D graphic's parent.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the 3D graphic does not have a parent. This is only returned when [`Label`](../../Reference/3dgra/M3dgraInquire.md) is the root node. |
| `Value >= 0` | Specifies the label of the 3D graphic's parent. |

### Combination Constants — For inquiring about all descendants of a 3D graphic

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the labels or the number of all descendants of a 3D graphic in a 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RECURSIVE` | Specifies to get the labels or the number of all descendants of a 3D graphic in a 3D graphics list. |

### For inquiring the settings of a 3D graphic

The following inquire types allow you to inquire about 3D graphics settings.  > **Note:** Note that all inquires on the position or rotation of a 3D graphic are returned relative to the 3D graphic's parent's coordinate system. To inquire positions and rotations relative to the root node, use [`M_RELATIVE_TO_ROOT`](../../Reference/3dgra/M3dgraInquire.md).

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_ARC`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and an arc graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_ANGLE`

Inquires the arc graphic's angle.

| Value | Description |
| --- | --- |
| *(see [`M_ANGLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_APPEARANCE`

Inquires whether the arc graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CENTER_X`

Inquires the X-coordinate of the arc graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the arc graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the arc graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_END_POINT_X`

Inquires the X-coordinate of the arc graphic's end point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the arc graphic's end point, relative to its parent's coordinate system. |

#### `M_END_POINT_Y`

Inquires the Y-coordinate of the arc graphic's end point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_END_POINT_Z`

Inquires the Z-coordinate of the arc graphic's end point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_GRAPHIC_RESOLUTION`

Inquires the resolution of the arc graphic's mesh.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_RESOLUTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the arc graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_NORMAL_X`

Inquires the X-component of the arc graphic's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the arc graphic's normal unit vector, relative to its parent's coordinate system. |

#### `M_NORMAL_Y`

Inquires the Y-component of the arc graphic's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the arc graphic's normal unit vector, relative to its parent's coordinate system. |

#### `M_NORMAL_Z`

Inquires the Z-component of the arc graphic's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the arc graphic's normal unit vector, relative to its parent's coordinate system. |

#### `M_OPACITY`

Inquires the opacity of the arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the arc graphic's position. The position is the arc graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the arc's center, its X-axis is the vector directed from the arc's center to the arc's start point, and its Z-axis is the arc's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the arc graphic's position. The position is the arc graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the arc's center, its X-axis is the vector directed from the arc's center to the arc's start point, and its Z-axis is the arc's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the arc graphic's position. The position is the arc graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the arc's center, its X-axis is the vector directed from the arc's center to the arc's start point, and its Z-axis is the arc's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RADIUS`

Inquires the arc graphic's radius.

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the arc graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_START_POINT_X`

Inquires the X-coordinate of the arc graphic's start point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_START_POINT_Y`

Inquires the Y-coordinate of the arc graphic's start point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_START_POINT_Z`

Inquires the Z-coordinate of the arc graphic's start point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the arc graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_ARC_FILL`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and an arc graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_ANGLE`

Inquires the filled arc graphic's angle.

| Value | Description |
| --- | --- |
| *(see [`M_ANGLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_APPEARANCE`

Inquires whether the filled arc graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CENTER_X`

Inquires the X-coordinate of the filled arc graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the filled arc graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the filled arc graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the filled arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_END_POINT_X`

Inquires the X-coordinate of the filled arc graphic's end point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the filled arc graphic's end point, relative to its parent's coordinate system. |

#### `M_END_POINT_Y`

Inquires the Y-coordinate of the filled arc graphic's end point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_END_POINT_Z`

Inquires the Z-coordinate of the filled arc graphic's end point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the filled arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_GRAPHIC_RESOLUTION`

Inquires the resolution of the filled arc graphic's mesh.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_RESOLUTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the filled arc graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_NORMAL_X`

Inquires the X-component of the filled arc graphic's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the filled arc graphic's normal unit vector, relative to its parent's coordinate system. |

#### `M_NORMAL_Y`

Inquires the Y-component of the filled arc graphic's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the filled arc graphic's normal unit vector, relative to its parent's coordinate system. |

#### `M_NORMAL_Z`

Inquires the Z-component of the filled arc graphic's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the filled arc graphic's normal unit vector, relative to its parent's coordinate system. |

#### `M_OPACITY`

Inquires the opacity of the filled arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the filled arc graphic's position. The position is the filled arc graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the filled arc's center, its X-axis is the vector directed from the filled arc's center to the filled arc's start point, and its Z-axis is the filled arc's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the filled arc graphic's position. The position is the filled arc graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the filled arc's center, its X-axis is the vector directed from the filled arc's center to the filled arc's start point, and its Z-axis is the filled arc's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the filled arc graphic's position. The position is the filled arc graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the filled arc's center, its X-axis is the vector directed from the filled arc's center to the filled arc's start point, and its Z-axis is the arc's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RADIUS`

Inquires the filled arc graphic's radius.

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the filled arc graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the filled arc graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_START_POINT_X`

Inquires the X-coordinate of the filled arc graphic's start point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_START_POINT_Y`

Inquires the Y-coordinate of the filled arc graphic's start point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_START_POINT_Z`

Inquires the Z-coordinate of the filled arc graphic's start point.

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the filled arc graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the filled arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_AXIS`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and an axis graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the axis graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the axis graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_PER_AXIS` | Specifies that the 3 axes are not set to the same color. To inquire the color of each axis, use [`M_COLOR_AXIS...`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_AXIS_X`

Inquires the color of the axis graphic's X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_AXIS_X`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_AXIS_Y`

Inquires the color of the axis graphic's Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_AXIS_Y`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_AXIS_Z`

Inquires the color of the axis graphic's Z-axis.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_AXIS_Z`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_GRAPHIC_RESOLUTION`

Inquires the resolution of the axis graphic's mesh.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_RESOLUTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the axis graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_LENGTH`

Inquires the length of the axis graphic's axes.

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the axis graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the axis graphic's position. The position is the axis graphic's coordinate system relative to the parent's coordinate system. The origin is the origin of the axes, and its orientation is the orientation of the axes.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the axis graphic's position. The position is the axis graphic's coordinate system relative to the parent's coordinate system. The origin is the origin of the axes, and its orientation is the orientation of the axes.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the axis graphic's position. The position is the axis graphic's coordinate system relative to the parent's coordinate system. The origin is the origin of the axes, and its orientation is the orientation of the axes.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the axis graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the axis graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the axis graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the axis graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_BOX`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a box graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the specified box graphic's corner relative to its parent's coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the specified box graphic's corner relative to its parent's coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the specified box graphic's corner relative to its parent's coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates of the corners of the box graphic's specified face, relative to its parent's coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the corners of the box graphic's specified face, relative to its parent's coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the corners of the box graphic's specified face, relative to its parent's coordinate system. |

#### `M_APPEARANCE`

Inquires whether the box graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_AXIS_ALIGNED`

Inquires whether the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_ALIGNED`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_X`

Inquires the X-coordinate of the box graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the box graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the box graphic's center.

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the box graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_CORNER_X_ALL`

Inquires the X-coordinate of each of the box graphic's 8 corners, in their index order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-component the box graphic's corner relative to the box graphic's parent. |

#### `M_CORNER_Y_ALL`

Inquires the Y-coordinate of each of the box graphic's 8 corners, in their index order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-component the box graphic's corner relative to the box graphic's parent. |

#### `M_CORNER_Z_ALL`

Inquires the Z-coordinate of each of the box graphic's 8 corners, in their index order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-component the box graphic's corner relative to the box graphic's parent. |

#### `M_EDITABLE`

Inquires if the box is editable interactively.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_ROTATION`

Inquires the size of the rotation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_ROTATION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_SCALE`

Inquires the size of the scaling handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_SCALE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_TRANSLATION`

Inquires the size of the translation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_TRANSLATION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FACE_n_CORNERS_X`

Inquires the X-coordinates of each corner of box face _n_, in their index order, where _n_ is the box face index.

| Value | Description |
| --- | --- |
| *(see [`M_FACE_n_CORNERS_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_FACE_n_CORNERS_Y`

Inquires the Y-coordinates of each corner of box face _n_, in their index order, where _n_ is the box face index.

| Value | Description |
| --- | --- |
| *(see [`M_FACE_n_CORNERS_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_FACE_n_CORNERS_Z`

Inquires the Z-coordinates of each corner of box face _n_, in their index order, where _n_ is the box face index.

| Value | Description |
| --- | --- |
| *(see [`M_FACE_n_CORNERS_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the box graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the box graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the box graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the box graphic's position. The position is the box graphic's coordinate system relative to the parent's coordinate system. The origin is the box graphic's center, and its coordinate system's orientation is the box graphic's orientation.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the box graphic's position relative to its parent's coordinate system. |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the box graphic's position. The position is the box graphic's coordinate system relative to the parent's coordinate system. The origin is the box graphic's center, and its coordinate system's orientation is the box graphic's orientation.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the box graphic's position relative to its parent's coordinate system. |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the box graphic's position. The position is the box graphic's coordinate system relative to the parent's coordinate system. The origin is the box graphic's center, and its coordinate system's orientation is the box graphic's orientation.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the box graphic's position relative to its parent's coordinate system. |

#### `M_RENDER_LAYER`

Inquires which layer the box graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE`

Inquires whether the box graphic is rotatable interactively. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_X`

Inquires whether the box graphic is rotatable interactively around the X-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_Y`

Inquires whether the box graphic is rotatable interactively around the Y-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_Z`

Inquires whether the box graphic is rotatable interactively around the Z-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE`

Inquires whether the box graphic is scalable interactively. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_X`

Inquires whether the box graphic is scalable interactively in the X-direction. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_X0`

Inquires whether the box graphic is scalable interactively in the X-direction by the individual handle, initially corresponding to the minimum X-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_X0`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_X1`

Inquires whether the box graphic is scalable interactively in the X-direction by the individual handle, initially corresponding to the maximum X-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_X1`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_Y`

Inquires whether the box graphic is scalable interactively in the Y-direction. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_Y0`

Inquires whether the box graphic is scalable interactively in the Y-direction by the individual handle, initially corresponding to the minimum Y-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_Y0`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_Y1`

Inquires whether the box graphic is scalable interactively in the Y-direction by the individual handle, initially corresponding to the maximum Y-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_Y1`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_Z`

Inquires whether the box graphic is scalable interactively in the Z-direction. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_Z0`

Inquires whether the box graphic is scalable interactively in the Z-direction by the individual handle, initially corresponding to the minimum Z-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_Z0`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SCALABLE_Z1`

Inquires whether the box graphic is scalable interactively in the Z-direction by the individual handle, initially corresponding to the maximum Z-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_SCALABLE_Z1`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the box graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_X`

Inquires the box graphic's length along its coordinate system's X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_Y`

Inquires the box graphic's length along its coordinate system's Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_Z`

Inquires the box graphic's length along its coordinate system's Z-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the box graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE`

Inquires whether the box graphic is translatable interactively. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE_X`

Inquires whether the box graphic is translatable interactively in the X-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE_Y`

Inquires whether the box graphic is translatable interactively in the Y-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE_Z`

Inquires whether the box graphic is translatable interactively in the Z-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_UNROTATED_MAX_X`

Inquires the box graphic's maximum X-coordinate, ignoring the box's rotation. The coordinate is returned as if the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_UNROTATED_MAX_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_UNROTATED_MAX_Y`

Inquires the box graphic's maximum Y-coordinate, ignoring the box's rotation. The coordinate is returned as if the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_UNROTATED_MAX_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_UNROTATED_MAX_Z`

Inquires the box graphic's maximum Z-coordinate, ignoring the box's rotation. The coordinate is returned as if the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_UNROTATED_MAX_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_UNROTATED_MIN_X`

Inquires the box graphic's minimum X-coordinate, ignoring the box's rotation. The coordinate is returned as if the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_UNROTATED_MIN_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_UNROTATED_MIN_Y`

Inquires the box graphic's minimum Y-coordinate, ignoring the box's rotation. The coordinate is returned as if the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_UNROTATED_MIN_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_UNROTATED_MIN_Z`

Inquires the box graphic's minimum Z-coordinate, ignoring the box's rotation. The coordinate is returned as if the box graphic is axis-aligned relative to its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_UNROTATED_MIN_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the box graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_CYLINDER`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a cylinder graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the cylinder graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_AXIS_X`

Inquires the X-component of the cylinder graphic's central axis unit vector. This vector does not reflect the cylinder graphic's length, regardless of whether a vector was used to define the cylinder graphic's length.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_AXIS_Y`

Inquires the Y-component of the cylinder graphic's central axis unit vector. This vector does not reflect the cylinder graphic's length, regardless of whether a vector was used to define the cylinder graphic's length.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_AXIS_Z`

Inquires the Z-component of the cylinder graphic's central axis unit vector. This vector does not reflect the cylinder graphic's length, regardless of whether a vector was used to define the cylinder graphic's length.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_X`

Inquires the X-coordinate of the center point on the cylinder graphic's central axis. This inquire type is only available if the cylinder graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraCylinder.md).

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the center point on the cylinder graphic's central axis. This inquire type is only available if the cylinder graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraCylinder.md).

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the center point on the cylinder graphic's central axis. This inquire type is only available if the cylinder graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraCylinder.md).

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CLIPPING_MODE`

Inquires the way in which the infinite cylinder graphic is clipped with respect to the clipping box.

| Value | Description |
| --- | --- |
| *(see [`M_CLIPPING_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_DISPLAY_BASES`

Inquires whether the cylinder graphic's circular bases are displayed or not.

| Value | Description |
| --- | --- |
| *(see [`M_DISPLAY_BASES`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_END_POINT_X`

Inquires the X-coordinate of the cylinder graphic's end point, positioned at the center of the cylinder graphic's second circular base. This inquire type is only available if the cylinder graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraCylinder.md).

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_END_POINT_Y`

Inquires the Y-coordinate of the cylinder graphic's end point, positioned at the center of the cylinder graphic's second circular base. This inquire type is only available if the cylinder graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraCylinder.md).

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_END_POINT_Z`

Inquires the Z-coordinate of the cylinder graphic's end point, positioned at the center of the cylinder graphic's second circular base. This inquire type is only available if the cylinder graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraCylinder.md).

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_GRAPHIC_RESOLUTION`

Inquires the resolution of the cylinder graphic's mesh.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_RESOLUTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the cylinder graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_LENGTH`

Inquires the cylinder graphic's length. Note that for infinite cylinders, the value returned is truncated to the limits of the clipping box of the 3D graphics list (as it existed when the cylinder graphic was added).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the cylinder graphic's position. The position is the cylinder graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the cylinder graphic's start point, and its Z-axis is the cylinder graphic's central axis.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the cylinder graphic's position. The position is the cylinder graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the cylinder graphic's start point, and its Z-axis is the cylinder graphic's central axis.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the cylinder graphic's position. The position is the cylinder graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the cylinder graphic's start point, and its Z-axis is the cylinder graphic's central axis.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RADIUS`

Inquires the cylinder graphic's radius.

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the cylinder graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the cylinder graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_START_POINT_X`

Inquires the X-coordinate of the cylinder graphic's start point on the cylinder's central axis.  When the cylinder graphic's length is infinite, this returns the X-coordinate of the first point used to define the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_START_POINT_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_START_POINT_Y`

Inquires the Y-coordinate of the cylinder graphic's start point on the cylinder's central axis.  When the cylinder graphic's length is infinite, this returns the X-coordinate of the first point used to define the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_START_POINT_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_START_POINT_Z`

Inquires the Z-coordinate of the cylinder graphic's start point on the cylinder's central axis.  When the cylinder graphic's length is infinite, this returns the Z-coordinate of the first point used to define the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_START_POINT_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the cylinder graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_DOTS`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a dots graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_COLOR`

Inquires the color of the points in the dots graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_PER_POINT` | Specifies that the color of the dots are individually set using [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md) with a color array. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the dots graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the dots graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POINTS_X`

Inquires the X-coordinate of each point in the dots graphic.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of a point in the dots graphic, relative to its parent's coordinate system. |

#### `M_POINTS_Y`

Inquires the Y-coordinate of each point in the dots graphic.

| Value | Description |
| --- | --- |
| *(see [`M_POINTS_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POINTS_Z`

Inquires the Z-coordinate of each point in the dots graphic.

| Value | Description |
| --- | --- |
| *(see [`M_POINTS_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the dots graphic's position. The position is the dots graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the position and orientation of its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the dots graphic's position. The position is the dots graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the position and orientation of its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the dots graphic's position. The position is the dots graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the position and orientation of its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the dots graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the dots graphic's points in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the dots graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_GRID`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a grid graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the grid graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the grid graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_EDITABLE`

Inquires if the grid is editable interactively.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_ROTATION`

Inquires the size of the rotation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_ROTATION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_TRANSLATION`

Inquires the size of the translation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_TRANSLATION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the grid graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the grid graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_NUMBER_OF_TILES_X`

Inquires the number of cells along the grid graphic's X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_TILES_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_NUMBER_OF_TILES_Y`

Inquires the number of cells along the grid graphic's Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_TILES_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the grid graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the grid graphic's position. The position is the grid graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the grid's center, and its orientation is the grid's orientation.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the grid graphic's position. The position is the grid graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the grid's center, and its orientation is the grid's orientation.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the grid graphic's position. The position is the grid graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the grid's center, and its orientation is the grid's orientation.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the grid graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE`

Inquires whether the grid graphic is rotatable interactively. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_X`

Inquires whether the grid graphic is rotatable interactively around the X-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_Y`

Inquires whether the grid graphic is rotatable interactively around the Y-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_Z`

Inquires whether the grid graphic is rotatable interactively around the Z-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_X`

Inquires the grid graphic's length along its coordinate system's X-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the grid graphic, in world units, along the grid's coordinate system's X-axis. |

#### `M_SIZE_Y`

Inquires the grid graphic's length along its coordinate system's Y-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the grid graphic, in world units, along the grid's coordinate system's Y-axis. |

#### `M_SPACING_X`

Inquires the spacing between the lines along the grid graphic's X-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SPACING_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SPACING_Y`

Inquires the spacing between the lines along the grid graphic's Y-axis.

| Value | Description |
| --- | --- |
| *(see [`M_SPACING_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the grid graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE`

Inquires whether the grid graphic is translatable interactively. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE_Z`

Inquires whether the grid graphic is translatable interactively in the Z-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the grid graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_LINE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a line graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the line graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_AXIS_X`

Inquires the X-component of the line graphic's direction unit vector. This vector does not reflect the line graphic's length, regardless of whether a vector was used to define the line graphic's length.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_AXIS_Y`

Inquires the Y-component of the line graphic's direction unit vector. This vector does not reflect the line graphic's length, regardless of whether a vector was used to define the line graphic's length.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_AXIS_Z`

Inquires the Z-component of the line graphic's direction unit vector. This vector does not reflect the line graphic's length, regardless of whether a vector was used to define the line graphic's length.

| Value | Description |
| --- | --- |
| *(see [`M_AXIS_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_X`

Inquires the X-coordinate of the line graphic's center point. This inquire type is only available if the line graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraLine.md).

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the line graphic's center point. This inquire type is only available if the line graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraLine.md).

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the line graphic's center point. This inquire type is only available if the line graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraLine.md).

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CLIPPING_MODE`

Inquires the way in which the infinite line graphic is clipped with respect to the clipping box.

| Value | Description |
| --- | --- |
| *(see [`M_CLIPPING_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_END_POINT_X`

Inquires the X-coordinate of the line graphic's end point. This inquire type is only available if the line graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraLine.md).

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_END_POINT_Y`

Inquires the Y-coordinate of the line graphic's end point. This inquire type is only available if the line graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraLine.md).

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_END_POINT_Z`

Inquires the Z-coordinate of the line graphic's end point. This inquire type is only available if the line graphic's length is not [`M_INFINITE`](../../Reference/3dgra/M3dgraLine.md).

| Value | Description |
| --- | --- |
| *(see [`M_END_POINT_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the line graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_LENGTH`

Inquires the line graphic's length. Note that for infinite lines, the value returned is truncated to the limits of the clipping box of the 3D graphics list (as it existed when the line graphic was added).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the line graphic's position. The position is the line graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the line graphic's start point, and its Z-axis shares the same direction as the line graphic's direction.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the line graphic's position. The position is the line graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the line graphic's start point, and its Z-axis shares the same direction as the line graphic's direction.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the line graphic's position. The position is the line graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the line graphic's start point, and its Z-axis shares the same direction as the line graphic's direction.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the line graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_START_POINT_X`

Inquires the X-coordinate of the line graphic's start point.  When the line graphic's length is infinite, this returns the X-coordinate of the first point used to define the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_START_POINT_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_START_POINT_Y`

Inquires the Y-coordinate of the line graphic's start point.  When the line graphic's length is infinite, this returns the X-coordinate of the first point used to define the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_START_POINT_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_START_POINT_Z`

Inquires the Z-coordinate of the line graphic's start point.  When the line graphic's length is infinite, this returns the X-coordinate of the first point used to define the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_START_POINT_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/line of the line graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_LINES`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a lines graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the lines graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the lines graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_PER_LINE` | Specifies that the color of the lines are individually set using [`M3dgraLines`](../../Reference/3dgra/M3dgraLines.md) with a color array. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_END_POINTS_X`

Inquires the X-coordinates of the lines graphic's end points.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates of the lines graphic's end points, relative to its parent's coordinate system. |

#### `M_END_POINTS_Y`

Inquires the Y-coordinates of the lines graphic's end points.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinates of the lines graphic's end points, relative to its parent's coordinate system. |

#### `M_END_POINTS_Z`

Inquires the Z-coordinates of the lines graphic's end points.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinates of the lines graphic's end points, relative to its parent's coordinate system. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the lines graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the lines graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinates of the lines graphic's position. The position is the lines graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinates of the lines graphic's position. The position is the lines graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinates of the lines graphic's position. The position is the lines graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the lines graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_START_POINTS_X`

Inquires the X-coordinates of the lines graphic's start points.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates of the start points used to define the lines graphic, relative to its parent's coordinate system. |

#### `M_START_POINTS_Y`

Inquires the Y-coordinates of the lines graphic's start points.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinates of the start points used to define the lines graphic, relative to its parent's coordinate system. |

#### `M_START_POINTS_Z`

Inquires the Z-coordinates of the lines graphic's start points.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinates of the start points used to define the lines graphic, relative to its parent's coordinate system. |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the lines graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the lines graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_NODE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a node graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_POSITION_X`

Inquires the X-coordinate of the node graphic's position. The position is the node graphic's coordinate system's origin relative to the parent's coordinate system. By default, the origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the node graphic's position. The position is the node graphic's coordinate system's origin relative to the parent's coordinate system. By default, the origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the node graphic's position. The position is the node graphic's coordinate system's origin relative to the parent's coordinate system. By default, the origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_PLANE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a plane graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the plane graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CLIPPING_MODE`

Inquires the way in which the infinite plane graphic is clipped with respect to the clipping box.

| Value | Description |
| --- | --- |
| *(see [`M_CLIPPING_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CLOSEST_TO_ORIGIN_X`

Inquires the X-coordinate of the point on the plane graphic closest to the origin of its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_CLOSEST_TO_ORIGIN_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CLOSEST_TO_ORIGIN_Y`

Inquires the Y-coordinate of the point on the plane graphic closest to the origin of its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_CLOSEST_TO_ORIGIN_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CLOSEST_TO_ORIGIN_Z`

Inquires the Z-coordinate of the point on the plane graphic closest to the origin of its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_CLOSEST_TO_ORIGIN_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COEFFICIENT_A`

Inquires the coefficient _A_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| *(see [`M_COEFFICIENT_A`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COEFFICIENT_B`

Inquires the coefficient _B_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| *(see [`M_COEFFICIENT_B`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COEFFICIENT_C`

Inquires the coefficient _C_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| *(see [`M_COEFFICIENT_C`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COEFFICIENT_D`

Inquires the coefficient _D_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| *(see [`M_COEFFICIENT_D`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_EDITABLE`

Inquires if the plane is editable interactively.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_ROTATION`

Inquires the size of the rotation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_ROTATION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_EDITABLE_HANDLE_SIZE_TRANSLATION`

Inquires the size of the translation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| *(see [`M_EDITABLE_HANDLE_SIZE_TRANSLATION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the plane graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_NORMAL_X`

Inquires the X-component of the plane graphic's normal unit vector.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_NORMAL_Y`

Inquires the Y-component of the plane graphic's normal unit vector.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_NORMAL_Z`

Inquires the Z-component of the plane graphic's normal unit vector.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the plane graphic's position. The position is the plane graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the point on the plane graphic closest to the origin of its parent's coordinate system, and its Z-axis is the plane graphic's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the plane graphic's position. The position is the plane graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the point on the plane graphic closest to the origin of its parent's coordinate system, and its Z-axis is the plane graphic's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the plane graphic's position. The position is the plane graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the point on the plane graphic closest to the origin of its parent's coordinate system, and its Z-axis is the plane graphic's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RECLIPPING_MODE`

Inquires when to automatically reclip the infinite plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_RECLIPPING_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the plane graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE`

Inquires whether the plane graphic is rotatable interactively. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_X`

Inquires whether the plane graphic is rotatable interactively around the X-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_X`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_Y`

Inquires whether the plane graphic is rotatable interactively around the Y-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_Y`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE_Z`

Inquires whether the plane graphic is rotatable interactively around the Z-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_ROTATABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the plane graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the plane graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE`

Inquires whether the plane graphic is translatable interactively. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE_Z`

Inquires whether the plane graphic is translatable interactively in the Z-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TRANSLATABLE_Z`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_POINT_CLOUD`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a point cloud graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the point cloud graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines in the point cloud graphic when they are set to a color using [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_COMPONENT`

Inquires the component used to color the points in the point cloud graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_COMPONENT`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_NULL` | Specifies that the color of the points in the point cloud graphic is set to a single color using [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_COLOR_COMPONENT_BAND`

Inquires the band used to color the point cloud graphic when the point cloud graphic's color is set using [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_COMPONENT_BAND`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS`

Inquires the limits of the values in the component used to color the point cloud graphic when the point cloud graphic is colored using [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS_MAX`

Inquires the maximum color value when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS_MAX`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS_MIN`

Inquires the minimum color value when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS_MIN`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LUT_ID`

Inquires the identifier of the current LUT buffer associated with the point cloud graphic. If the current LUT is predefined (default [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md)), Aurora Imaging Library returns the type of predefined colormap to which the LUT is set ([`M_COLORMAP_...`](../../Reference/3dgra/M3dgraCopy.md)). If the current LUT is user-defined, the identifier of an internal copy of the user-defined LUT is returned.  For a user-defined LUT for which an internal copy exists, you must not free the associated buffer. You can, however, modify the buffer.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LUT_ID`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_COLOR_LUT_SIZE`

Inquires the size of the point cloud graphic's LUT.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the size of the point cloud graphic's LUT. |

#### `M_COLOR_LUT_SIZE_BAND`

Inquires the number of bands of the point cloud graphic's LUT.

| Value | Description |
| --- | --- |
| `1` | Specifies that the point cloud graphic's LUT has one band. |
| `3` | Specifies that the point cloud graphic's LUT has three bands. |

#### `M_COLOR_USE_LUT`

Inquires whether the point cloud graphic is colored using a LUT to map values from the component specified by [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) to a color.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_USE_LUT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CONTAINER_ID`

Inquires the identifier of the container or fully-corrected depth map image buffer linked to the 3D graphic.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies an unlinked 3D graphic; [`M_NULL`](../../Reference/3dgra/M3dgraInquire.md) is returned when the 3D graphic was added using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md). |
| `Container identifier` | Specifies the Aurora Imaging Library identifier of the container linked to the 3D graphic. |
| `Image buffer identifier` | Specifies the Aurora Imaging Library identifier of the depth map image buffer linked to the 3D graphic. |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the point cloud graphic (if the 3D graphic is linked to a meshed point cloud container).

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FIX`

Inquires whether to automatically internally call[`M3dimFix`](../../Reference/3dim/M3dimFix.md)to fix NaNs and invalid mesh points.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the point cloud graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the point cloud.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the point cloud graphic's position. The position is the point cloud graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the point cloud graphic's position. The position is the point cloud graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the point cloud graphic's position. The position is the point cloud graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the point cloud graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the point cloud graphic's points in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD`

Inquires whether to use different LoDs (levels of detail) based on the display view. This improves performance when the display selects a lower LoD.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_LEVELS`

Inquires how many LoDs to generate if [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraInquire.md)is set to [`M_ENABLE`](../../Reference/3dgra/M3dgraInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_LEVELS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`

Inquires the maximum factor by which to resample the point cloud when regenerating the LoDs.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_SAMPLE_MODE`

Inquires what type of sub-sampling to perform when generating LoDs.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_SAMPLE_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the point cloud graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_POLYGON`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a polygon graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the polygon graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_TEXTURE_SIZE_BAND`

Inquires the number of bands of the polygon graphic's texture buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies that the polygon graphic's texture buffer has one band. |
| `3` | Specifies that the polygon graphic's texture buffer has three bands. |

#### `M_COLOR_TEXTURE_SIZE_X`

Inquires the width of the polygon graphic's texture buffer.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the width of the polygon graphic's texture buffer, in pixels. |

#### `M_COLOR_TEXTURE_SIZE_Y`

Inquires height of the polygon graphic's texture buffer.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the height of the polygon graphic's texture buffer, in pixels. |

#### `M_COLOR_USE_TEXTURE`

Inquires whether the polygon is set to be filled with a texture.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_USE_TEXTURE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_FALSE` | Specifies to fill the polygon with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the polygon graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_KEYING_COLOR`

Inquires the pixel value to display as transparent for the polygon graphic.  This setting is only used if the polygon is filled with a texture buffer. Any portion of the polygon graphic that would be displayed with this color is instead made transparent, regardless of the polygon graphic's [`M_OPACITY`](../../Reference/3dgra/M3dgraInquire.md)setting. This can be used to cut out parts of a polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_KEYING_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POINTS_X`

Inquires the X-coordinate of each of the polygon graphic's vertices.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-component of each of the polygon graphic's vertices. |

#### `M_POINTS_Y`

Inquires the Y-coordinate of each of the polygon graphic's vertices.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-component of each of the polygon graphic's vertices. |

#### `M_POINTS_Z`

Inquires the Z-coordinate of each of the polygon graphic's vertices.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-component of each of the polygon graphic's vertices. |

#### `M_POSITION_X`

Inquires the X-coordinate of the polygon graphic's position. The position is the polygon graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the polygon graphic's position. The position is the polygon graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the polygon graphic's position. The position is the polygon graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the polygon graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the polygon graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the polygon graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_RECT`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a rectangle graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the rectangle graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FILL_COLOR`

Inquires the color of the solid surface of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the rectangle graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the rectangle graphic's position. The position is the center of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the rectangle graphic's position. The position is the center of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the rectangle graphic's position. The position is the center of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires on which layer the rectangle graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the rectangle graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_X`

Inquires the rectangle graphic's length along its coordinate system's X-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the rectangle graphic, in world units, along the rectangle's coordinate system's X-axis. |

#### `M_SIZE_Y`

Inquires the rectangle graphic's length along its coordinate system's Y-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the rectangle graphic, in world units, along the rectangle's coordinate system's Y-axis. |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the rectangle graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_SPHERE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a sphere graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_APPEARANCE`

Inquires whether the sphere graphic is displayed as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CENTER_X`

Inquires the X-coordinate of the sphere graphic's center point.

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_X`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the sphere graphic's center point.

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Y`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the sphere graphic's center point.

| Value | Description |
| --- | --- |
| *(see [`M_CENTER_Z`](Reference/3dgeo/M3dgeoInquire.md))* |  |

#### `M_COLOR`

Inquires the color of the points and lines of the sphere graphic.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FILL_COLOR`

Inquires the color of the solid surfaces of the sphere graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_SAME_AS_COLOR` | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraInquire.md). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_GRAPHIC_RESOLUTION`

Inquires the resolution of the sphere graphic's mesh.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_RESOLUTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the sphere graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the sphere graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the sphere graphic's position. The position is the sphere graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the sphere graphic's center, and its orientation is inherited, upon creation, from its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the sphere graphic's position. The position is the sphere graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the sphere graphic's center, and its orientation is inherited, upon creation, from its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the sphere graphic's position. The position is the sphere graphic's coordinate system's origin relative to the parent's coordinate system. The origin is the sphere graphic's center, and its orientation is inherited, upon creation, from its parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RADIUS`

Inquires the sphere graphic's radius.

| Value | Description |
| --- | --- |
| *(see [`M_RADIUS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the sphere graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Inquires the sphere graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Inquires the thickness of the points/lines of the sphere graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the sphere graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_TEXT`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a text graphic specified by the [`Label`](../../Reference/3dgra/M3dgraInquire.md) parameter.

#### `M_BACKGROUND_COLOR`

Inquires the text graphic's background color.

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_BACKGROUND_MODE`

Inquires the text graphic's background mode.

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Inquires the text graphic's text color.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dgra/M3dgraControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FIXED_FONT_SIZE`

Inquires the font size in pixels for text graphics with fixed scaling ([`M_FIXED_SCALE`](../../Reference/3dgra/M3dgraText.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FIXED_FONT_SIZE`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_FONT`

Inquires the text graphic's font.

| Value | Description |
| --- | --- |
| *(see [`M_FONT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT_AUTO_SELECT`

Inquires the automatic font selection behavior of the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_FONT_AUTO_SELECT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT_SIZE`

Inquires the font size.

| Value | Description |
| --- | --- |
| *(see [`M_FONT_SIZE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_GRAPHIC_TEXT`

Inquires the text that is displayed in the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_TEXT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TARGET_LABEL`

Inquires which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Inquires whether the text graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Inquires the opacity of the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_POSITION_X`

Inquires the X-coordinate of the text graphic's position. The position is the text graphic's coordinate system's origin relative to the parent's coordinate system. By default, the origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_X`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the text graphic's position. The position is the text graphic's coordinate system's origin relative to the parent's coordinate system. By default, the origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Y`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the text graphic's position. The position is the text graphic's coordinate system's origin relative to the parent's coordinate system. By default, the origin is the same as the parent's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION_Z`](Reference/3dgra/M3dgraInquire.md))* |  |

#### `M_RENDER_LAYER`

Inquires which layer the text graphic is rendered on when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_ALIGN_HORIZONTAL`

Inquires the horizontal alignment of the text in the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_ALIGN_HORIZONTAL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_ALIGN_VERTICAL`

Inquires the vertical alignment of the text in the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_ALIGN_VERTICAL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_BORDER`

Inquires the borders around the text.  Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific border.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_BORDER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_DIRECTION`

Inquires the direction to draw the text that is displayed in the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_DIRECTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Inquires the visibility of the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

### Combination Constants — For inquiring a position or rotation relative to the 3D graphics list's root node

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the position or rotation a 3D graphic relative to the 3D graphics list's root node.

| Value | Description |
| --- | --- |
| `M_RELATIVE_TO_ROOT` | Specifies to get the position or rotation of a 3D graphic relative to the 3D graphics list's root node. |

### Combination Constants — For inquiring the number of points in a dots graphic or vertices in a polygon graphic

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the number of points in a dots graphic or vertices in a polygon graphic.

| Value | Description |
| --- | --- |
| `M_NB_ELEMENTS` | Specifies to get the number of points in a dots graphic or vertices in a polygon graphic. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
