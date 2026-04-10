---
doctype: Reference
module: 3dgra
function: M3dgraControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraControl"
---

# M3dgraControl

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

> Control the 3D graphics in a 3D graphics list, as well as their default settings.

## Syntax

```c
void M3dgraControl(
    AIL_ID     List3dgraId,  //out
    AIL_INT64  Label,        //in
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control the settings of a 3D graphic in a 3D graphics list, as well as default settings for 3D graphics subsequently added to the list.

You can control the default settings of the 3D graphics list using [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraControl.md). These settings are applied to any 3D graphics that you subsequently add to the 3D graphics list. They are not applied retroactively to 3D graphics already in the 3D graphics list.

You can control the settings for all existing descendants of the specified 3D graphic using[`M_RECURSIVE`](../../Reference/3dgra/M3dgraControl.md). This affects all descendants for which the setting is available (even if the setting is not available for the specified 3D graphic). It does not affect 3D graphics that you subsequently add as children of the specified 3D graphic.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list to control. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `Label` *(in, AIL_INT64)*

Specifies the label of the 3D graphic to control. You can also control the default settings of the 3D graphics list itself.

*For specifying the 3D graphic label*

| Value | Description |
| --- | --- |
| `M_DEFAULT_SETTINGS` | Specifies to control the default settings of the 3D graphics that are added to the 3D graphics list. These settings will not be applied to the 3D graphics that are already in the 3D graphics list. |
| `M_LIST` | Specifies to control the 3D graphics list itself. |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the setting.

## Parameter Associations

### For the 3D graphics list itself

The following control types allow you to control the 3D graphics list when [`Label`](../../Reference/3dgra/M3dgraControl.md) is [`M_LIST`](../../Reference/3dgra/M3dgraControl.md). These control types affect all graphics in the graphics list.

---

### `Graphics list identifier with Label set to M_LIST`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md).

#### `M_EDITABLE_APPEARANCE`

Sets the appearance of the interactively editable graphic on display such that it appears as a solid surface, wireframe, or points. This is the appearance applied to a graphic when [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) is [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` |  |
| `M_SOLID_WITH_WIREFRAME` *(default)* | Specifies a solid appearance with a wireframe. |

#### `M_EDITABLE_COLOR`

Sets the default color of the points and lines of the interactively editable graphics added to the 3D graphics list. This is the color applied to a graphic when [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) is [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_CYAN` *(default)* | Specifies the color cyan. |

#### `M_EDITABLE_OPACITY`

Sets the default opacity of the interactively editable graphics added to the 3D graphics list. This is the opacity applied to a graphic when [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) is [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the opacity of the 3D graphic. A 3D graphic with an opacity of 100.0 is completely opaque, while an opacity of 0.0 makes it completely transparent (invisible). |

### For the default settings of a 3D graphics list

The following [`ControlType`](../../Reference/3dgra/M3dgraControl.md)and corresponding[`ControlValue`](../../Reference/3dgra/M3dgraControl.md) parameter settings are used to change the default settings of a 3D graphics list.

---

### `Graphics list ID with Label set to M_DEFAULT_SETTINGS`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraControl.md) specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the default appearance of the 3D graphics added to the 3D graphics list, to either display them as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_POINTS` | Specifies a points appearance. The 3D graphic appears as a set of points representing its vertices. |
| `M_SOLID` *(default)* | Specifies a solid appearance. |
| `M_SOLID_WITH_POINTS` | Specifies a solid appearance with points. |
| `M_SOLID_WITH_WIREFRAME` | Specifies a solid appearance with a wireframe. |
| `M_WIREFRAME` | Specifies a wireframe appearance. The 3D graphic appears as a set of lines connecting its vertices. |

#### `M_BACKGROUND_COLOR`

Sets the default background color for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the text graphic's background color. |
| `M_COLOR_BLACK` *(default)* | Specifies the color black. |

#### `M_BACKGROUND_MODE`

Sets the default background mode for text graphics added to the 3D graphics list. You can specify the default background color for text graphics using [`M_BACKGROUND_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OPAQUE` | Specifies that the background of the text graphic is filled with the current background color. |
| `M_TRANSPARENT` *(default)* | Specifies that the text graphic does not have a background. |

#### `M_COLOR`

Sets the default color of the points and lines of the 3D graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_COLOR_AXIS_X`

Sets the default color of the X-axis of axis graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies the default value. The default value is a red color defined by **M_RGB888()** with values 215, 0, and 0. |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |

#### `M_COLOR_AXIS_Y`

Sets the default color of the Y-axis of axis graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies the default value. The default value is a green color defined by **M_RGB888()** with values 50, 255, and 50. |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |

#### `M_COLOR_AXIS_Z`

Sets the default color of the Z-axis of axis graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies the default value. The default value is a blue color defined by **M_RGB888()** with values 0, 140, and 255. |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |

#### `M_COLOR_COMPONENT`

Sets the default color of the points in the point cloud graphics added to the 3D graphics list, using one of the source container's components. You can specify a component using its identifier or its type.  If there is not exactly one component with the specified identifier or type, or if the specified component does not have the same size and dimensions as the container's range component, the color defaults to [`M_NULL`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** This control type is ignored if [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) is used to color the point cloud graphic using a LUT; in this case, the range component is always used.

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies to color the point cloud graphic according to the
                                    component that  stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md)or [`M_COMPONENT_DISPARITY`](../../Reference/3dgra/M3dgraControl.md)component of the container. |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores the A coordinate map provided by the camera. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores the B coordinate map provided by the camera. |
| `M_COMPONENT_CUSTOM + n` | Specifies to color the point cloud graphic according to the
                                    component that  has a custom component type, identified by _n_, where n can be a value between 0 and 255. |
| `M_COMPONENT_DISPARITY` | Specifies to color the point cloud graphic according to the
                                    component that  stores a disparity map. |
| `M_COMPONENT_INFRARED` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image of visible light. |
| `M_COMPONENT_MESH_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md)component of the container. |
| `M_COMPONENT_METADATA` | Specifies to color the point cloud graphic according to the
                                    component that  stores metadata information. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image where each band represents the intensity of a specific wavelength of light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the buffer stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies to color the point cloud graphic according to the
                                    component that  stores 3D distance/position information. |
| `M_COMPONENT_REFLECTANCE` | Specifies to color the point cloud graphic according to the
                                    component that  stores a reflectance map. |
| `M_COMPONENT_REGION_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md) component of the container. |
| `M_COMPONENT_SCATTER` | Specifies to color the point cloud graphic according to the
                                    component that  stores a scatter map. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image of ultraviolet light. |
| `M_NULL` | Specifies that the color of the points in the point cloud graphic is set to the single color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |
| `M_AUTO_COLOR` *(default)* | Specifies to color the point cloud graphic according to specific components in the source container. If the container is a point cloud, the point cloud graphic is colored based on the reflectance component. If the reflectance component does not exist, the intensity component is used. If there is no reflectance or intensity component, this control is set to [`M_NULL`](../../Reference/3dgra/M3dgraControl.md), and the point cloud graphic will be colored using the single color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).  If the container is a depth map, the point cloud graphic is colored according to the range component's third band.  > **Note:** Note that this setting ignores [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md). All three component bands are used for point clouds, while the 3rd component band is used tor depth maps. |

#### `M_COLOR_COMPONENT_BAND`

Sets the component's band to use by default for point cloud graphics added to the 3D graphics list when [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) is set to a component type.  If the band does not exist, or if [`M_ALL_BANDS`](../../Reference/3dgra/M3dgraControl.md) is used on a component that does not have 3 bands, band 0 is used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` *(default)* | Specifies all the bands of the component. |
| `Value >= 0` | Specifies the index of the band to use.  The relationship between index value and band for RGB, HSL, and YUV buffers is the following:  |   |   | | --- | --- | | 0 | Corresponds to the red band (for RGB parent buffers), the hue band (for HSL parent buffers), and the Y band (for YUV parent buffers). | | 1 | Corresponds to the green band (for RGB parent buffers), the saturation band (for HSL parent buffers), and the U band (for YUV parent buffers). | | 2 | Corresponds to the blue band (for RGB parent buffers), the luminance band (for HSL parent buffers), and the V band (for YUV parent buffers). | |

#### `M_COLOR_LIMITS`

Sets the default limits of the values in the component used to color the point cloud graphics added to the 3D graphics list. Values between the minimum and the maximum are remapped linearly to values between the minimum and maximum possible display values. The values beyond the minimum or maximum are saturated. When [`M_COLOR_USE_LUT`](../../Reference/3dgra/M3dgraControl.md) is [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md), the minimum and maximum get mapped to the LUT's extreme values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BUFFER_LIMITS` *(default)* | Specifies to use the buffer's minimum and maximum.  > **Note:** Note that this is unavailable for floating-point buffers, and defaults to [`M_DATA_EXTREMES_PER_BAND`](../../Reference/3dgra/M3dgraControl.md). |
| `M_DATA_EXTREMES_GLOBAL` | Specifies to use the global minimum and maximum of the buffer's data. If the buffer has multiple bands, a single minimum and maximum is used for all of them. |
| `M_DATA_EXTREMES_PER_BAND` | Specifies to use the minimum and maximum of the buffer's data in each of its bands. If the buffer has multiple bands, each band has its own minimum and maximum. |
| `M_USER_DEFINED` | Specifies to use the minimum and maximum manually set with [`M_COLOR_LIMITS_MIN`](../../Reference/3dgra/M3dgraControl.md) and [`M_COLOR_LIMITS_MAX`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_COLOR_LIMITS_MAX`

Sets the default maximum color value for point cloud graphics added to the 3D graphics list when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the maximum color value to use. |

#### `M_COLOR_LIMITS_MIN`

Sets the default minimum color value for point cloud graphics added to the 3D graphics list when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the minimum color value to use. |

#### `M_COLOR_USE_LUT`

Sets whether to color the point cloud graphics added to the 3D graphics list by mapping each value of the component specified by [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) to a color in a LUT, and then using that color to display the corresponding point. The default LUT is [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md), but you can change it using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md). If the LUT is set to [`M_NULL`](../../Reference/3dgra/M3dgraCopy.md) using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_COLOR_LUT`](../../Reference/3dgra/M3dgraCopy.md), this setting will be ignored even if it is set to [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md). Note that if [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md) is set to [`M_ALL_BANDS`](../../Reference/3dgra/M3dgraControl.md) and the specified component does not have 3 bands, band 0 is used.  This control type is a more flexible alternative than using [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md), which applies a LUT exclusively to the source point cloud's range component.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies not to use a LUT. |
| `M_TRUE` | Specifies to use a LUT. |

#### `M_COLOR_USE_TEXTURE`

Specifies whether to fill the polygon graphics added to the 3D graphics list with a texture by default. If the polygon graphic has no texture buffer, [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md) is used regardless of this setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` | Specifies to fill the polygon graphic with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md). |
| `M_TRUE` *(default)* | Specifies to fill the polygon graphic with a texture buffer. |

#### `M_DISPLAY_BASES`

Sets whether the cylinder graphic's bases are displayed or not by default when a cylinder graphic is added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the cylinder graphic's bases are not displayed. |
| `M_ENABLE` *(default)* | Specifies that the cylinder graphic's bases are displayed. |

#### `M_FILL_COLOR`

Sets the default color of the solid surfaces of the 3D graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_FIXED_FONT_SIZE`

Sets the default font size in pixels for text graphics with fixed scaling ([`M_FIXED_SCALE`](../../Reference/3dgra/M3dgraText.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the height of the font in pixels. |

#### `M_FONT`

Sets the default font for text graphics added to the 3D graphics list.  Aurora Imaging Library will use TrueType and Unicode features to draw text. This allows you to draw text using different sizes and TrueType fonts installed on your computer. This also allows you to draw any Unicode text (depending on the font).

| Value | Description |
| --- | --- |
| `M_FONT_DEFAULT_TTF` | Specifies the default TrueType font of your operating system. |
| `"FontFamily:Weight:Slant"` | Specifies the font according to the following format, _Family_:_Weight_:_Slant_.  _Family_ must be set to the name of the font's family, such as Arial, Times New Roman, and Wingdings.  _Weight_ can be set to one of the following: Book, Thin, ExtraLight, UltraLight, Light, Normal, Regular, Medium, SemiBold, DemiBold, Bold, ExtraBold, UltraBold, Heavy, Black, ExtraBlack, or UltraBlack.  _Slant_ can be set to one of the following: Italic, Oblique, or Roman.  You can omit _Weight_ and _Slant_; also, you can provide the _Weight_ and _Slant_ in any order. |
| `"FontFile"` | Specifies a full path to a TrueType file name. |

#### `M_FONT_AUTO_SELECT`

Sets the default automatic font selection behavior for text graphics added to the 3D graphics list.  If automatic font selection is enabled for a text graphic, Aurora Imaging Library searches for a suitable font to draw the text if the currently selected font (set using [`M_FONT`](../../Reference/3dgra/M3dgraControl.md)) does not support the character code. Aurora Imaging Library will first attempt to make its selection from already used fonts, and then from system fonts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not search for a suitable font. An error is returned if the character code cannot be drawn using the current TrueType font. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library will search for a suitable font. |

#### `M_FONT_SIZE`

Sets the default font size for text graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the font size. This is the height of one line of text, in world units. |

#### `M_GRAPHIC_RESOLUTION`

Sets the default mesh resolution of 3D graphics added to the 3D graphics list. This applies to arc, filled arc, axis, cylinder, and sphere graphics.  > **Note:** Higher graphic resolutions settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the resolution of the 3D graphic's mesh. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NO_LABEL` *(default)* | Specifies that the handle graphic does not have a target graphic.  The handle graphic will move or rotate when dragged and will generate hook events. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node.  The target graphic will move or rotate when dragged and will generate hook events. |

#### `M_HANDLE_TYPE`

Sets whether the graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the graphic is not a handle and cannot be clicked and dragged. |
| `M_HANDLE_CLICKABLE` | Specifies that the graphic is a clickable handle.  If this setting is enabled, an [`M_HANDLE_GRAPHIC_CLICKED`](../../Reference/3dgra/M3dgraHookFunction.md) event is generated but the graphic will not be moved. |
| `M_HANDLE_ROTATION_Z` | Specifies that the graphic is a handle that can be rotated around its Z-axis. |
| `M_HANDLE_TRANSLATION_X` | Specifies that the graphic is a handle that can be translated along its X-axis. |
| `M_HANDLE_TRANSLATION_Y` | Specifies that the graphic is a handle that can be translated along its Y-axis. |
| `M_HANDLE_TRANSLATION_Z` | Specifies that the graphic is a handle that can be translated along its Z-axis. |

#### `M_KEYING_COLOR`

Sets the default pixel value to display as transparent for polygon graphics added to the 3D graphics list.  This setting is only used if the polygon graphic is filled with a texture buffer. Any portion of the polygon graphic that would be displayed with this color is instead made transparent, regardless of the polygon graphic's [`M_OPACITY`](../../Reference/3dgra/M3dgraControl.md)setting. This can be used to cut out parts of a polygon graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the texture's keying value. |
| `M_NONE` *(default)* | Specifies that the texture does not have a keying value. |
| `0 <= Value <= 255` | Specifies a grayscale keying value, used when the texture is a 1-band buffer. |

#### `M_OPACITY`

Sets the default opacity of the 3D graphics added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the opacity of the 3D graphic. A 3D graphic with an opacity of 100.0 is completely opaque, while an opacity of 0.0 makes it completely transparent (invisible). |

#### `M_RENDER_LAYER`

Sets the default layer on which the 3D graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 9` *(default)* | Specifies the layer on which the 3D graphic will be rendered. |

#### `M_SHADING`

Sets the default shading of the 3D graphics added to the 3D graphics list. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** This setting does not apply to text, node, line, arc, filled arc, grid, or dots graphics. You can control the default shading mode for text graphics using [`M_TEXT_SHADING`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FLAT` | Specifies that the 3D graphic is displayed with flat shading. Each face of the 3D graphic's mesh is shown with a single brightness level based on its normal relative to the view. The faces of the 3D graphic's mesh are clearly distinguishable with this setting, because flat shading does not perform any smoothing. |
| `M_GOURAUD` *(default)* | Specifies that the 3D graphic is displayed with Gouraud shading. The triangles that make up the faces of the 3D graphic's mesh are shown with smooth shading, calculated by interpolating the brightness level between each vertex based on the triangle's normal relative to the view. The faces of the 3D graphic's mesh are typically distinguishable with this setting, but some smoothing is involved. |
| `M_NONE` | Specifies that the 3D graphic is displayed without shading. The same uniform lighting is used regardless of the angle at which the object is viewed. |
| `M_PHONG` | Specifies that the 3D graphic is displayed with Phong shading. The 3D graphic is shaded smoothly based on a complex reflection model. The faces of the 3D graphic's mesh are typically not distinguishable with this setting, because the surface of the mesh is shown with advanced smoothing. |

#### `M_TEXT_ALIGN_HORIZONTAL`

Sets the default horizontal justification of the text of the text graphic. This also affects the alignment of the text relative to the origin of the text graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` | Specifies that the text is horizontally centered. |
| `M_LEFT` *(default)* | Specifies that the text is left-aligned. |
| `M_RIGHT` | Specifies that the text is right-aligned. |

#### `M_TEXT_ALIGN_VERTICAL`

Sets the default vertical justification of the text of the text graphic. This also affects the alignment of the text relative to the origin of the text graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOTTOM` | Specifies that the text is bottom-aligned. |
| `M_CENTER` | Specifies that the text is vertically centered. |
| `M_TOP` *(default)* | Specifies that the text is top-aligned. |

#### `M_TEXT_BORDER`

Sets the default borders around text graphics added to the 3D graphics list.  Note that the possible settings can be combined. For example, to draw a box around the text, specify [`M_TOP`](../../Reference/3dgra/M3dgraControl.md)+[`M_BOTTOM`](../../Reference/3dgra/M3dgraControl.md)+[`M_LEFT`](../../Reference/3dgra/M3dgraControl.md)+[`M_RIGHT`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOTTOM` | Specifies that a line is drawn underneath the text. |
| `M_LEFT` | Specifies that a line is drawn to the left of the text. |
| `M_NONE` *(default)* | Specifies that no border is drawn around the text. This setting cannot be combined with any other setting. |
| `M_RIGHT` | Specifies that a line is drawn to the right of the text. |
| `M_TOP` | Specifies that a line is drawn above the text. |

#### `M_TEXT_DIRECTION`

Sets the default direction to draw text when a text graphic is added to the 3D graphics list.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LEFT_TO_RIGHT` *(default)* | Specifies that text will be drawn from left to right. |
| `M_RIGHT_TO_LEFT` | Specifies that text will be drawn from right to left. |

#### `M_TEXT_SHADING`

Controls the default shading of the text graphics added to the 3D graphics list. Shaded text graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.  > **Note:** Displaying a shaded 3D graphic requires more GPU processing power than displaying an unshaded 3D graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the text graphic is displayed without shading. |
| `M_ENABLE` | Specifies that the text graphic is displayed with shading. |

#### `M_THICKNESS`

Sets the default thickness of the points/lines of the 3D graphics added to the 3D graphics list in the 3D world, on screen.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the point/line thickness, in pixels. |

#### `M_VIEW_BASED_LOD`

Sets whether to use different LoDs (levels of detail) based on the display view. This improves performance when the display selects a lower LoD.  When shown with a lower LoD, the point cloud might exclude points which the user would have been able to see at full LoD. However, new points will never be inferred or created in a lower LoD.  > **Note:** When this is enabled, the display can pause noticeably when generating new LoDs. When live grabbing from a 3D sensor, the grab thread might also be blocked during this process. You should only use this setting if needed (for less powerful CPU/GPUs), and if so you should set any other LoD settings before enabling LoD to prevent needless regeneration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not use LoDs. |
| `M_ENABLE` | Specifies to use LoDs. |

#### `M_VIEW_BASED_LOD_LEVELS`

Sets how many LoDs to generate if [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraControl.md)is set to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md). For example, a value of 1 indicates to only use the native LoD and a value of 3 indicates to generate two degraded LoDs.  Generating more LoDs increases memory consumption and the time required to regenerate LoDs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of LoDs. |

#### `M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`

Sets the maximum factor by which to resample the point cloud when regenerating the LoDs. Setting this to 1.0 would be effectively the same as setting [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraControl.md) to [`M_DISABLE`](../../Reference/3dgra/M3dgraControl.md).  For example, with 3 LoD levels and a sample factor max of 8, the display will generate 2 LoDs (in addition to the full LoD); one with approximately 1/8 the number of points as the source, and one with approximately 1/4 the number of points as the source.  When using random sampling, the lowest LoD will have (NumValidPoints / FactorMax) points. When using grid sampling, this factor is used to determine the grid size for each LoD.  Increasing this value without increasing the number of LoD levels will reduce the memory consumption and can reduce the time to regenerate the LoDs. However, it will also reduce the likelihood of the display using the generated LoDs, thereby reducing the performance benefit of enabling LoD (unless the display's degradation level is also decreased).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 1.0` *(default)* | Specifies the maximum level of detail. |

#### `M_VIEW_BASED_LOD_SAMPLE_MODE`

Sets what type of sub-sampling to perform when generating LoDs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SUBSAMPLE_GRID` *(default)* | Specifies to use grid sampling to generate LoDs.  Grid sampling gives a good representation of the full detail point cloud, and is less likely to be visibly degraded when [`M_LOD_DEGRADATION_LEVEL`](../../Reference/3ddisp/M3ddispControl.md) level is set to a low value in the display. |
| `M_SUBSAMPLE_RANDOM` | Specifies to use random sampling to generate LoDs.  Random sampling requires significantly less time to regenerate the LoDs than grid sampling. |

#### `M_VISIBLE`

Sets the default visibility of the 3D graphics added to the 3D graphics list. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` | Specifies that the 3D graphic is not visible. |
| `M_TRUE` *(default)* | Specifies that the 3D graphic is visible. |

### For the settings of a 3D graphic

The following [`ControlType`](../../Reference/3dgra/M3dgraControl.md)and corresponding[`ControlValue`](../../Reference/3dgra/M3dgraControl.md) parameter settings are used to change the settings of a 3D graphic.

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_ARC`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and an arc graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_ANGLE`

Sets the arc graphic's angle.

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the arc graphic's angle, in degrees. Negative values can only happen when defining an arc with [`M_NORMAL_AND_ANGLE`](../../Reference/3dgra/M3dgraArcFill.md) with a negative angle. The returned angle is negative so it still respects the right hand rule. |

#### `M_APPEARANCE`

Sets the appearance of the arc graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the arc graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_GRAPHIC_RESOLUTION`

Sets the resolution of the arc graphic's mesh. This is the number of edges on the arc graphic. For example, if this value is set to 3, the arc graphic is shown as 3 lines.  > **Note:** Higher graphic resolutions settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the number of edges on the arc graphic. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the arc graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RADIUS`

Sets the arc graphic's radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the arc graphic's radius, in world units. |

#### `M_RENDER_LAYER`

Sets on which layer the arc graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the arc graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the arc graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_ARC_FILL`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a filled arc graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_ANGLE`

Sets the filled arc graphic's angle.

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the filled arc graphic's angle, in degrees. Negative values can only happen when defining a filled arc with [`M_NORMAL_AND_ANGLE`](../../Reference/3dgra/M3dgraArcFill.md) with a negative angle. The returned angle is negative so it still respects the right hand rule. |

#### `M_APPEARANCE`

Sets the appearance of the filled arc graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the filled arc graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the filled arc graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_GRAPHIC_RESOLUTION`

Sets the resolution of the filled arc graphic's mesh. This is the number of edges on the filled arc graphic. For example, if this value is set to 3, the filled arc graphic is shown as 3 lines.  > **Note:** Higher graphic resolutions settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the number of edges on the filled arc graphic. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the filled arc graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the filled arc graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RADIUS`

Sets the filled arc graphic's radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the filled arc graphic's radius, in world units. |

#### `M_RENDER_LAYER`

Sets on which layer the filled arc graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the filled arc graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the filled arc graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the filled arc graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_AXIS`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and an axis graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the axis graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the axis graphic. This will set all three axes to the same color. To assign a color to a specific axis, use [`M_COLOR_AXIS_...`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_COLOR_AXIS_X`

Sets the color of the axis graphic's X-axis.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies the default value. The default value is a red color defined by **M_RGB888()** with values 215, 0, and 0. |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |

#### `M_COLOR_AXIS_Y`

Sets the color of the axis graphic's Y-axis.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies the default value. The default value is a green color defined by **M_RGB888()** with values 50, 255, and 50. |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |

#### `M_COLOR_AXIS_Z`

Sets the color of the axis graphic's Z-axis.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` | Specifies the default value. The default value is a blue color defined by **M_RGB888()** with values 0, 140, and 255. |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |

#### `M_GRAPHIC_RESOLUTION`

Sets the resolution of the mesh for the cone-shaped arrow caps of the axis graphic. This is the number of edges on the axis graphic's cone-shaped arrow caps. For example, if this value is set to 3, the axis arrow caps are shown as tetrahedrons.  > **Note:** Higher graphic resolutions settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the number of edges on the axis graphic's cone-shaped arrow caps. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the axis graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_LENGTH`

Sets the length of the axis graphic's axes.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the axis graphic's axes, in world units. |

#### `M_OPACITY`

Sets the opacity of the axis graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the axis graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the axis graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the axis graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the axis graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_BOX`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a box graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the box graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the box graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_EDITABLE`

Sets whether the box graphic is editable interactively. This adds handles to the box when it is displayed so that you can modify it interactively. To establish which handles are displayed, use [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md). When a box graphic is editable interactively, a selectable axis graphic is displayed representing the box graphic's coordinate system. Click and drag on an axis (or its arrowhead) to reposition the box. To interactively change the size of the box, click and drag on a scaling handle (shown as a brown cube on each face). To change its orientation, click and drag on one of the curved arcs that sit at the center of the displayed box graphic.  To change the box graphic's appearance when editable, use[`M_EDITABLE_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md), [`M_EDITABLE_COLOR`](../../Reference/3dgra/M3dgraControl.md), and [`M_EDITABLE_OPACITY`](../../Reference/3dgra/M3dgraControl.md).  Only one box is editable at a time.  For more information, see [Interactive 3D graphics](../../UserGuide/C43_3D_Display_and_graphics/S08_Interactive_3D_graphics.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the graphic cannot be edited interactively. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` | Specifies that the graphic can be edited interactively. |

#### `M_EDITABLE_HANDLE_SIZE_ROTATION`

Sets the size of the rotation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_EDITABLE_HANDLE_SIZE_SCALE`

Sets the size of the scaling handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_EDITABLE_HANDLE_SIZE_TRANSLATION`

Sets the size of the translation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the box graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the box graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the box graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the box graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE`

Sets whether the box graphic is rotatable interactively. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be rotated. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be rotated. |

#### `M_ROTATABLE_X`

Sets whether the box graphic is rotatable interactively around the X-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be rotated around the X-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be rotated around the X-axis. |

#### `M_ROTATABLE_Y`

Sets whether the box graphic is rotatable interactively around the Y-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be rotated around the Y-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be rotated around the Y-axis. |

#### `M_ROTATABLE_Z`

Sets whether the box graphic is rotatable interactively around the Z-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be rotated around the Z-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be rotated around the Z-axis. |

#### `M_SCALABLE`

Sets whether the box graphic is scalable interactively. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled. |

#### `M_SCALABLE_X`

Sets whether the box graphic is scalable interactively in the X-direction. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the X-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the X-direction. |

#### `M_SCALABLE_X0`

Sets whether the box graphic is scalable interactively in the X-direction by the individual handle, initially corresponding to the minimum X-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_SCALABLE_X`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the X-direction using the X0 handle. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the X-direction using the X0 handle. |

#### `M_SCALABLE_X1`

Sets whether the box graphic is scalable interactively in the X-direction by the individual handle, initially corresponding to the maximum X-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_SCALABLE_X`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the X-direction using the X1 handle. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the X-direction using the X1 handle. |

#### `M_SCALABLE_Y`

Sets whether the box graphic is scalable interactively in the Y-direction. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the Y-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the Y-direction. |

#### `M_SCALABLE_Y0`

Sets whether the box graphic is scalable interactively in the Y-direction by the individual handle, initially corresponding to the minimum Y-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_SCALABLE_Y`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the Y-direction using the Y0 handle. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the Y-direction using the Y0 handle. |

#### `M_SCALABLE_Y1`

Sets whether the box graphic is scalable interactively in the Y-direction by the individual handle, initially corresponding to the maximum Y-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_SCALABLE_Y`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the Y-direction using the Y1 handle. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the Y-direction using the Y1 handle. |

#### `M_SCALABLE_Z`

Sets whether the box graphic is scalable interactively in the Z-direction. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the Z-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the Z-direction. |

#### `M_SCALABLE_Z0`

Sets whether the box graphic is scalable interactively in the Z-direction by the individual handle, initially corresponding to the minimum Z-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_SCALABLE_Z`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the Z-direction using the Z0 handle. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the Z-direction using the Z0 handle. |

#### `M_SCALABLE_Z1`

Sets whether the box graphic is scalable interactively in the Z-direction by the individual handle, initially corresponding to the maximum Z-position of the box. To show the scaling handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), [`M_SCALABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_SCALABLE_Z`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be scaled in the Z-direction using the Z1 handle. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be scaled in the Z-direction using the Z1 handle. |

#### `M_SHADING`

Sets the box graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_X`

Sets the box graphic's length along its coordinate system's X-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the box graphic, in world units, along the box's coordinate system's X-axis. |

#### `M_SIZE_Y`

Sets the box graphic's length along its coordinate system's Y-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the box graphic, in world units, along the box's coordinate system's Y-axis. |

#### `M_SIZE_Z`

Sets the box graphic's length along its coordinate system's Z-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the box graphic, in world units, along the box's coordinate system's Z-axis. |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the box graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE`

Sets whether the box graphic is translatable interactively. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be translated. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be translated. |

#### `M_TRANSLATABLE_X`

Sets whether the box graphic is translatable interactively in the X-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be translated in the X-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be translated in the X-direction. |

#### `M_TRANSLATABLE_Y`

Sets whether the box graphic is translatable interactively in the Y-direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be translated in the Y-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be translated in the Y-direction. |

#### `M_TRANSLATABLE_Z`

Sets whether the box graphic is translatable interactively in the Z-Direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the box cannot be translated in the Z-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the box can be translated in the Z-direction. |

#### `M_VISIBLE`

Sets the visibility of the box graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_CYLINDER`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a cylinder graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the cylinder graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CLIPPING_MODE`

Sets the way in which the infinite cylinder graphic is clipped with respect to the clipping box. The graphic is clipped when added to the display; you can use [`M_RECLIP`](../../Reference/3dgra/M3dgraControl.md) to manually reclip the graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLIP_TO_BOX` *(default)* | Specifies to use the clipping box. If the graphic does not intersect with the clipping box, [`M_CLIP_TO_PROJECTION`](../../Reference/3dgra/M3dgraControl.md) is used instead. |
| `M_CLIP_TO_PROJECTION` | Specifies to use the projection of the clipping box. |

#### `M_COLOR`

Sets the color of the points and lines of the cylinder graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_DISPLAY_BASES`

Sets whether the cylinder graphic's bases are displayed or not.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the cylinder graphics's bases are not displayed. |
| `M_ENABLE` *(default)* | Specifies that the cylinder graphic's bases are displayed. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the cylinder graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_GRAPHIC_RESOLUTION`

Sets the resolution of the cylinder graphic's mesh. This is the number of edges on the cylinder graphic's bases and the number of faces that make up the sides of the cylinder. For example, if this value is set to 3, the cylinder graphic is shown as a triangular prism.  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the number of edges on the cylinder graphic's bases and the number of faces that make up the sides of the cylinder. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the cylinder graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_LENGTH`

Sets the cylinder graphic's length.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the cylinder graphic's length, in world units. |

#### `M_OPACITY`

Sets the opacity of the cylinder graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RADIUS`

Sets the cylinder graphic's radius.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the cylinder graphic's radius, in world units. |

#### `M_RECLIP`

Reclips the cylinder graphic according to the [`M_CLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

#### `M_RENDER_LAYER`

Sets on which layer the cylinder graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the cylinder graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the cylinder graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the cylinder graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_DOTS`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a dots graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_COLOR`

Sets the color of the points in the dots graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events. The target graphic will move or rotate when dragged and will generate hook events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the dots graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the dots graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the dots graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the dots graphic's points in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the dots graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_GRID`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a grid graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the grid graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the grid graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_EDITABLE`

Sets whether the grid graphic is editable interactively. This adds handles to the grid when it is displayed so that you can modify it interactively. To establish which handles are displayed, use [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md). When a grid graphic is editable interactively, a selectable axis graphic is displayed representing the grid graphic's coordinate system. Click and drag on an axis (or its arrowhead) to reposition the grid. To interactively change the size of the grid, click and drag on a scaling handle (shown as a brown cube on each face). To change its orientation, click and drag on one of the curved arcs that sit at the center of the displayed grid graphic.  To change the grid graphic's appearance when editable, use[`M_EDITABLE_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md), [`M_EDITABLE_COLOR`](../../Reference/3dgra/M3dgraControl.md), and [`M_EDITABLE_OPACITY`](../../Reference/3dgra/M3dgraControl.md).  Only one grid is editable at a time.  For more information, see [Interactive 3D graphics](../../UserGuide/C43_3D_Display_and_graphics/S08_Interactive_3D_graphics.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the graphic cannot be edited interactively. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` | Specifies that the graphic can be edited interactively. |

#### `M_EDITABLE_HANDLE_SIZE_ROTATION`

Sets the size of the rotation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_EDITABLE_HANDLE_SIZE_TRANSLATION`

Sets the size of the translation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the grid graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the grid graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_NUMBER_OF_TILES_X`

Sets the number of cells along the grid graphic's X-axis.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells along the grid graphic's X-axis. |

#### `M_NUMBER_OF_TILES_Y`

Sets the number of cells along the grid graphic's Y-axis.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells along the grid graphic's Y-axis. |

#### `M_OPACITY`

Sets the opacity of the grid graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the grid graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE`

Sets whether the grid graphic is rotatable interactively. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the grid cannot be rotated. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the grid can be rotated. |

#### `M_ROTATABLE_X`

Sets whether the grid graphic is rotatable interactively around the X-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the grid cannot be rotated around the X-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the grid can be rotated around the X-axis. |

#### `M_ROTATABLE_Y`

Sets whether the grid graphic is rotatable interactively around the Y-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the grid cannot be rotated around the Y-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the grid can be rotated around the Y-axis. |

#### `M_ROTATABLE_Z`

Sets whether the grid graphic is rotatable interactively around the Z-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the grid cannot be rotated around the Z-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the grid can be rotated around the Z-axis. |

#### `M_SPACING_X`

Sets the spacing between the lines along the grid graphic's X-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the spacing between the lines along the grid graphic's X-axis, in world units. |

#### `M_SPACING_Y`

Sets the spacing between the lines along the grid graphic's Y-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the spacing between the lines along the grid graphic's Y-axis, in world units. |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the grid graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE`

Sets whether the grid graphic is translatable interactively. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the grid cannot be translated. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the grid can be translated. |

#### `M_TRANSLATABLE_Z`

Sets whether the grid graphic is translatable interactively in the Z-Direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the grid cannot be translated in the Z-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the grid can be translated in the Z-direction. |

#### `M_VISIBLE`

Sets the visibility of the grid graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_LINE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a line graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the line graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CLIPPING_MODE`

Sets the way in which the infinite line graphic is clipped with respect to the clipping box. The graphic is clipped when added to the display; you can use [`M_RECLIP`](../../Reference/3dgra/M3dgraControl.md) to manually reclip the graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLIP_TO_BOX` *(default)* | Specifies to use the clipping box. If the graphic does not intersect with the clipping box, [`M_CLIP_TO_PROJECTION`](../../Reference/3dgra/M3dgraControl.md) is used instead. |
| `M_CLIP_TO_PROJECTION` | Specifies to use the projection of the clipping box. |

#### `M_COLOR`

Sets the color of the points and lines of the line graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the line graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_LENGTH`

Sets the line graphic's length.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the line graphic's length, in world units. |

#### `M_OPACITY`

Sets the opacity of the line graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RECLIP`

Reclips the line graphic according to the [`M_CLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

#### `M_RENDER_LAYER`

Sets on which layer the line graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/line of the line graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the line graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_LINES`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a lines graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the lines graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the lines graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the lines graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the lines graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the lines graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the lines graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the lines graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_PLANE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a plane graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the plane graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_CLIPPING_MODE`

Sets the way in which the infinite plane graphic is clipped with respect to the clipping box. The graphic is clipped when added to the display; you can use [`M_RECLIP`](../../Reference/3dgra/M3dgraControl.md) to manually reclip the graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLIP_TO_BOX` *(default)* | Specifies to use the clipping box. If the graphic does not intersect with the clipping box, [`M_CLIP_TO_PROJECTION`](../../Reference/3dgra/M3dgraControl.md) is used instead. |
| `M_CLIP_TO_PROJECTION` | Specifies to use the projection of the clipping box. |

#### `M_COLOR`

Sets the color of the points and lines of the plane graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_EDITABLE`

Sets whether the plane graphic is editable interactively. This adds handles to the plane when it is displayed so that you can modify it interactively. To establish which handles are displayed, use [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md). When a plane graphic is editable interactively, a selectable axis graphic is displayed representing the plane graphic's coordinate system. Click and drag on an axis (or its arrowhead) to reposition the plane. To interactively change the size of the plane, click and drag on a scaling handle (shown as a brown cube on each face). To change its orientation, click and drag on one of the curved arcs that sit at the center of the displayed plane graphic.  To change the plane graphic's appearance when editable, use[`M_EDITABLE_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md), [`M_EDITABLE_COLOR`](../../Reference/3dgra/M3dgraControl.md), and [`M_EDITABLE_OPACITY`](../../Reference/3dgra/M3dgraControl.md).  Only one plane is editable at a time.  For more information, see [Interactive 3D graphics](../../UserGuide/C43_3D_Display_and_graphics/S08_Interactive_3D_graphics.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the graphic cannot be edited interactively. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` | Specifies that the graphic can be edited interactively. |

#### `M_EDITABLE_HANDLE_SIZE_ROTATION`

Sets the size of the rotation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_EDITABLE_HANDLE_SIZE_TRANSLATION`

Sets the size of the translation handles, as a factor of the default size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the size of the handle graphic. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the plane graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the plane graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RECLIP`

Reclips the plane graphic according to the [`M_CLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

#### `M_RECLIPPING_MODE`

Sets when to automatically reclip the infinite plane graphic according to the [`M_CLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEVER_AUTO_RECLIP` | Specifies to never automatically reclip the graphic. The infinite graphic will only be reclipped if [`M_RECLIP`](../../Reference/3dgra/M3dgraControl.md) is specified. |
| `M_RECLIP_ON_HANDLE_RELEASE` *(default)* | Specifies to automatically reclip after the graphic is manipulated with a handle. |

#### `M_RENDER_LAYER`

Sets on which layer the plane graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_ROTATABLE`

Sets whether the plane graphic is rotatable interactively. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the plane cannot be rotated. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the plane can be rotated. |

#### `M_ROTATABLE_X`

Sets whether the plane graphic is rotatable interactively around the X-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the plane cannot be rotated around the X-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the plane can be rotated around the X-axis. |

#### `M_ROTATABLE_Y`

Sets whether the plane graphic is rotatable interactively around the Y-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the plane cannot be rotated around the Y-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the plane can be rotated around the Y-axis. |

#### `M_ROTATABLE_Z`

Sets whether the plane graphic is rotatable interactively around the Z-axis. To show the rotation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_ROTATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the plane cannot be rotated around the Z-axis. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the plane can be rotated around the Z-axis. |

#### `M_SHADING`

Sets the plane graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the plane graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TRANSLATABLE`

Sets whether the plane graphic is translatable interactively. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the plane cannot be translated. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the plane can be translated. |

#### `M_TRANSLATABLE_Z`

Sets whether the plane graphic is translatable interactively in the Z-Direction. To show the translation handles, you must set [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md) and [`M_TRANSLATABLE`](../../Reference/3dgra/M3dgraControl.md) to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies the plane cannot be translated in the Z-direction. You can still modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). |
| `M_ENABLE` *(default)* | Specifies the plane can be translated in the Z-direction. |

#### `M_VISIBLE`

Sets the visibility of the plane graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_POINT_CLOUD`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a point cloud graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the point cloud graphic on display as a solid surface, wireframe, or points. This setting is only used when the point cloud graphic is associated with a meshed point cloud container.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the point cloud graphic. To color the point cloud graphic using this color, set [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) to [`M_NULL`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_COLOR_COMPONENT`

Sets the color of the points in the point cloud graphic using one of the source container's components. You can specify a component using its identifier, or its type.  If there is not exactly one component with the specified identifier or type, or if the specified component does not have the same size and dimensions as the container's range component, the color defaults to [`M_NULL`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** This control type is ignored if [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) is used to color the point cloud graphic using a LUT; in this case, the range component is always used.

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies to color the point cloud graphic according to the
                                    component that  stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md)or [`M_COMPONENT_DISPARITY`](../../Reference/3dgra/M3dgraControl.md)component of the container. |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores the A coordinate map provided by the camera. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores the B coordinate map provided by the camera. |
| `M_COMPONENT_CUSTOM + n` | Specifies to color the point cloud graphic according to the
                                    component that  has a custom component type, identified by _n_, where n can be a value between 0 and 255. |
| `M_COMPONENT_DISPARITY` | Specifies to color the point cloud graphic according to the
                                    component that  stores a disparity map. |
| `M_COMPONENT_INFRARED` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image of visible light. |
| `M_COMPONENT_MESH_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md)component of the container. |
| `M_COMPONENT_METADATA` | Specifies to color the point cloud graphic according to the
                                    component that  stores metadata information. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image where each band represents the intensity of a specific wavelength of light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the buffer stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies to color the point cloud graphic according to the
                                    component that  stores 3D distance/position information. |
| `M_COMPONENT_REFLECTANCE` | Specifies to color the point cloud graphic according to the
                                    component that  stores a reflectance map. |
| `M_COMPONENT_REGION_AIL` | Specifies to color the point cloud graphic according to the
                                    component that  stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/3dgra/M3dgraControl.md) component of the container. |
| `M_COMPONENT_SCATTER` | Specifies to color the point cloud graphic according to the
                                    component that  stores a scatter map. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies to color the point cloud graphic according to the
                                    component that  stores an intensity image of ultraviolet light. |
| `M_NULL` | Specifies that the color of the points in the point cloud graphic is set to a single color using [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |
| `M_AUTO_COLOR` *(default)* | Specifies to color the point cloud graphic according to specific components in the source container. If the container is a point cloud, the point cloud graphic is colored based on the reflectance component. If the reflectance component does not exist, the intensity component is used. If there is no reflectance or intensity, [`M_NULL`](../../Reference/3dgra/M3dgraControl.md) is used. If it is set to [`M_NULL`](../../Reference/3dgra/M3dgraControl.md), the points of the point cloud graphic are colored using [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).  If the container is a depth map, the point cloud graphic is colored according to the range component's 3rd band. |

#### `M_COLOR_COMPONENT_BAND`

Sets the component's band to use when [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) is set to a component type.  If the specified band does not exist, band 0 is used.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_COMPONENT_BAND`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS`

Sets the limits of the values in the component used to color the point cloud graphic. Values between the minimum and the maximum are remapped linearly to values between the minimum and maximum possible display values. The values beyond the minimum or maximum are saturated. When [`M_COLOR_USE_LUT`](../../Reference/3dgra/M3dgraControl.md) is [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md), the minimum and maximum get mapped to the LUT's extreme values.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS_MAX`

Sets the maximum color value when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS_MAX`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_LIMITS_MIN`

Sets the minimum color value when [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS_MIN`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR_USE_LUT`

Sets whether to color the point cloud graphic by mapping each value of the component specified by [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) to a color in a LUT, and then using that color to display the corresponding point. The default LUT is [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md), but you can change it using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md). If the LUT is set to [`M_NULL`](../../Reference/3dgra/M3dgraCopy.md) using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_COLOR_LUT`](../../Reference/3dgra/M3dgraCopy.md), this setting will be ignored even if it is set to [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md). Note that if [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md) is set to [`M_ALL_BANDS`](../../Reference/3dgra/M3dgraControl.md) and the specified component does not have 3 bands, band 0 is used.  This control type is a more flexible alternative than using [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md), which applies a LUT exclusively to the source point cloud's range component.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_USE_LUT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the point cloud graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_FIX`

Sets whether to automatically internally call [`M3dimFix`](../../Reference/3dim/M3dimFix.md)to fix NaNs and invalid mesh points. If you know that your point cloud has no invalid points, disabling this will improve performance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to not automatically fix the point cloud. |
| `M_ENABLE` *(default)* | Specifies to automatically fix the point cloud. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the point cloud graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the point cloud graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the point cloud graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the point cloud graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting is only applicable for meshed point cloud containers with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the point cloud graphic's points in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD`

Sets whether to use different LoDs (levels of detail) based on the display view. This improves performance when the display selects a lower LoD.  When shown with a lower LoD, the point cloud might exclude points which the user would have been able to see at full LoD. However, new points will never be inferred or created in a lower LoD.  > **Note:** When this is enabled, the 3D display will take significantly longer to rescan any change to the graphic that requires regenerating the LoDs. You should only use this setting if needed (for less powerful CPU/GPUs), and if so you should set any other LoD settings before enabling LoD to prevent needless regeneration.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_LEVELS`

Sets how many LoDs to generate if [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraControl.md)is set to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md). A value of 1 indicates to only use the native LoD and a value of 3 indicates to generate two degraded LoDs.  Generating more LoDs increases memory consumption and the time required to regenerate LoDs.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_LEVELS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`

Sets the maximum factor by which to rescale the point cloud when regenerating the LoDs. Setting this to 1.0 would produce effectively no change in the LoD.  With 3 LoD levels and a sample factor max of 8, the display will generate 2 LoDs (in addition to the full LoD); one with approximately 1/8 the number of points as the source, and one with approximately 1/4 the number of points as the source.  When using random sampling, the lowest LoD will have (NumValidPoints / FactorMax) points. When using grid sampling, this factor is used to determine the grid size for each LoD.  Increasing this value without increasing the number of LoD levels will reduce the memory consumption and can reduce the time to regenerate the LoDs. However, it will also reduce the likelihood of the display using the generated LoDs (unless the display's degradation level is also decreased).

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VIEW_BASED_LOD_SAMPLE_MODE`

Sets what type of sub-sampling to perform when generating LoDs.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_BASED_LOD_SAMPLE_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the point cloud graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_POLYGON`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a polygon graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the polygon graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the polygon graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_COLOR_USE_TEXTURE`

Specifies whether to fill the polygon graphic with a texture. If the polygon graphic has no texture buffer, [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md) is used regardless of this setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` | Specifies to fill the polygon graphic with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md). |
| `M_TRUE` *(default)* | Specifies to fill the polygon graphic with a texture buffer. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the polygon graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the polygon graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_KEYING_COLOR`

Sets the pixel value to display as transparent for the polygon graphic.  This setting is only used if the polygon graphic is filled with a texture buffer. Any portion of the polygon graphic that would be displayed with this color is instead made transparent, regardless of the polygon graphic's [`M_OPACITY`](../../Reference/3dgra/M3dgraControl.md)setting. This can be used to cut out parts of a polygon graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the texture's keying value. |
| `M_NONE` *(default)* | Specifies that the texture does not have a keying value. |
| `0 <= Value <= 255` | Specifies a grayscale keying value, used when the texture is a 1-band buffer. |

#### `M_OPACITY`

Sets the opacity of the polygon graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the polygon graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the polygon graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the polygon graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the polygon graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_RECT`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a rectangle graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the rectangle graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the rectangle graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the rectangle graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the rectangle graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the rectangle graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the rectangle graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the rectangle graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SIZE_X`

Sets the rectangle graphic's length along its coordinate system's X-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the rectangle graphic, in world units, along the rectangle's coordinate system's X-axis. |

#### `M_SIZE_Y`

Sets the rectangle graphic's length along its coordinate system's Y-axis.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the rectangle graphic, in world units, along the rectangle's coordinate system's Y-axis. |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the rectangle graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the rectangle graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_SPHERE`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a sphere graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_APPEARANCE`

Sets the appearance of the sphere graphic on display as a solid surface, wireframe, or points.  The color of the points, wireframe, and the outline is determined by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md), while the color of the solid surface is determined by [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the color of the points and lines of the sphere graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_FILL_COLOR`

Sets the color of the solid surfaces of the sphere graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_SAME_AS_COLOR` *(default)* | Specifies to use the same color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). |

#### `M_GRAPHIC_RESOLUTION`

Sets the resolution of the sphere graphic's mesh. This is the number of latitudinal and longitudinal subdivisions on the sphere graphic.  > **Note:** Higher graphic resolutions settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the number of latitudinal and longitudinal subdivisions on the sphere graphic. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the sphere graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the sphere graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RADIUS`

Sets the sphere graphic's radius.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the sphere graphic's radius, in world units. |

#### `M_RENDER_LAYER`

Sets on which layer the sphere graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  Render layers can be used, for example, to create 3D backgrounds or heads-up displays.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_SHADING`

Sets the sphere graphic's shading. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. This setting only applies to graphics with[`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** More advanced shading settings might require a more powerful GPU to maintain acceptable performance, especially if a large number of 3D graphics are shown at once.

| Value | Description |
| --- | --- |
| *(see [`M_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_THICKNESS`

Sets the thickness of the points/lines of the sphere graphic in the 3D world, on screen.

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the sphere graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

---

### `Graphics list ID with Label specifying a graphic of type M_GRAPHIC_TYPE_TEXT`

Specifies a 3D graphics list, allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), and a text graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter.

#### `M_BACKGROUND_COLOR`

Sets the text graphic's background color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the text graphic's background color. |
| `M_COLOR_BLACK` *(default)* | Specifies the color black. |

#### `M_BACKGROUND_MODE`

Sets the text graphic's background mode. You can specify the background color using [`M_BACKGROUND_COLOR`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_BACKGROUND_MODE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_COLOR`

Sets the text graphic's text color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the 3D graphic's color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |

#### `M_FIXED_FONT_SIZE`

Sets the font size in pixels for text graphics with fixed scaling ([`M_FIXED_SCALE`](../../Reference/3dgra/M3dgraText.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FIXED_FONT_SIZE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT`

Sets the font of the text graphic.  Aurora Imaging Library will use TrueType and Unicode features to draw text. This allows you to draw text using different sizes and TrueType fonts installed on your computer. This also allows you to draw any Unicode text (depending on the font).

| Value | Description |
| --- | --- |
| *(see [`M_FONT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT_AUTO_SELECT`

Sets the automatic font selection behavior of the text graphic.  If automatic font selection is enabled, Aurora Imaging Library searches for a suitable font to draw the text if the currently selected font (set using [`M_FONT`](../../Reference/3dgra/M3dgraControl.md)) does not support the character code. Aurora Imaging Library will first attempt to make its selection from already used fonts, and then from system fonts.

| Value | Description |
| --- | --- |
| *(see [`M_FONT_AUTO_SELECT`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_FONT_SIZE`

Sets the font size of the text graphic. This setting is ignored for text graphics with fixed scaling ([`M_FIXED_SCALE`](../../Reference/3dgra/M3dgraText.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FONT_SIZE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_GRAPHIC_TEXT`

Sets the text of the text graphic.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the address of the null-terminated (\0) ASCII string to be displayed. |

#### `M_HANDLE_TARGET_LABEL`

Sets which graphic to use as the target for handle movement or clicking events.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TARGET_LABEL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_HANDLE_TYPE`

Sets whether the text graphic can be clicked and dragged.

| Value | Description |
| --- | --- |
| *(see [`M_HANDLE_TYPE`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_OPACITY`

Sets the opacity of the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_RENDER_LAYER`

Sets on which layer the text graphic is rendered when shown in a display.  The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.  This is useful, for example, to create label text that is always visible to the user, regardless of the view.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_ALIGN_HORIZONTAL`

Sets the horizontal justification of the text of the text graphic. This also affects the alignment of the text relative to the origin of the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_ALIGN_HORIZONTAL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_ALIGN_VERTICAL`

Sets the vertical justification of the text of the text graphic. This also affects the alignment of the text relative to the origin of the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_ALIGN_VERTICAL`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_BORDER`

Sets borders around the text.  Note that the possible settings can be combined. For example, to draw a box around the text, specify [`M_TOP`](../../Reference/3dgra/M3dgraControl.md)+[`M_BOTTOM`](../../Reference/3dgra/M3dgraControl.md)+[`M_LEFT`](../../Reference/3dgra/M3dgraControl.md)+[`M_RIGHT`](../../Reference/3dgra/M3dgraControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_BORDER`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_DIRECTION`

Sets the direction to draw the text graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_DIRECTION`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_TEXT_SHADING`

Sets the shading of the text graphic. Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display.  > **Note:** Displaying a shaded 3D graphic requires more GPU processing power than displaying an unshaded 3D graphic.

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_SHADING`](Reference/3dgra/M3dgraControl.md))* |  |

#### `M_VISIBLE`

Sets the visibility of the text graphic. Invisible 3D graphics do not count towards the 3D graphics list's bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_VISIBLE`](Reference/3dgra/M3dgraControl.md))* |  |

### Combination Constants — For setting a control value recursively

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the control type for the 3D graphic specified by [`Label`](../../Reference/3dgra/M3dgraControl.md) and for all of its children recursively.

| Value | Description |
| --- | --- |
| `M_RECURSIVE` | Sets the specified control type of the 3D graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter, and that of all of its children recursively, to the specified control value. Note that when  is used, the 3D graphic specified by the [`Label`](../../Reference/3dgra/M3dgraControl.md) parameter does not need to support the control type itself. This only sets the control type of the 3D graphics that can support it.  > **Note:** Note that this is only available if [`Label`](../../Reference/3dgra/M3dgraControl.md) specifies the label of a 3D graphic, or the root node. |

A new set of LoDs can be generated every time this setting or the point cloud data is changed. LoD features are not available with an Aurora Imaging Library Lite license.
