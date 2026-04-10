---
doctype: Reference
module: 3ddisp
function: M3ddispLut
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispLut"
---

# M3ddispLut

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

> Set a LUT to use to color the points of point cloud graphics in a 3D display.

## Syntax

```c
void M3ddispLut(
    AIL_ID    Display3dId,             //out
    AIL_INT64 DisplayPointCloudLabel,  //in
    AIL_ID    LutBufId,                //in
    AIL_INT64 Operation                //in
)
```

## Description

This function sets the LUT buffer to use for point cloud graphics selected to the specified 3D display. [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) maps the third band of the source point cloud's range component through the specified LUT.

You can specify to set a LUT for all point cloud graphics selected to the display, or for a specific point cloud graphic.

> **Note:** If you set the LUT for all point cloud graphics ([`M_ALL`](../../Reference/3ddisp/M3ddispLut.md) or [`M_DEFAULT`](../../Reference/3ddisp/M3ddispLut.md)), subsequent point cloud graphics selected to the same 3D display (using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md)) will use the specified LUT.

For more control when applying a LUT buffer (for example, to apply the LUT to a different band or a different component altogether), use [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) instead of [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md). Specifically, call [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_COLOR_LUT`](../../Reference/3dgra/M3dgraCopy.md) to associate the LUT with the internal 3D graphics list; then, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_COLOR_USE_LUT`](../../Reference/3dgra/M3dgraControl.md) set to [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md), and [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) and [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md) set to the required component and band, respectively.

To cease using a LUT with point clouds shown in the 3D display, call [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) with [`LutBufId`](../../Reference/3ddisp/M3ddispLut.md) set to [`M_DEFAULT`](../../Reference/3ddisp/M3ddispLut.md).

For more information, see [Lookup tables in general](../../UserGuide/C24_Lookup_tables/S01_Lookup_tables_in_general.md) and [Applying LUTs](../../UserGuide/C43_3D_Display_and_graphics/S04_Color_and_display_settings_for_3D_data.md).

## Parameters

### `Display3dId` *(out, AIL_ID)*

Specifies the identifier of the 3D display for which to set the LUT buffer.

### `DisplayPointCloudLabel` *(in, AIL_INT64)*

Specifies the label of the point cloud graphic to which to apply the LUT.

*For specifying the label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to apply the LUT to all point cloud graphics currently and subsequently selected to the 3D display. This is also applied to point cloud graphics that were added using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md).

> **Note:** Note that this setting changes the default settings of the internal 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraControl.md) and [`M_COLOR_USE_LUT`](../../Reference/3dgra/M3dgraControl.md), [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md), [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md)). |
| `Value` | Specifies the label of the point cloud graphic whose LUT to set. This is typically the return value of a call to [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md). |

### `LutBufId` *(in, AIL_ID)*

Specifies the identifier of a LUT buffer. This parameter should be set to one of the following values:

*For the identifier of the previously allocated LUT buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies not to use a LUT buffer. All LUT settings are restored to their defaults. The point cloud graphic is colored based on its source container's reflectance or intensity component ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) set to [`M_AUTO_COLOR`](../../Reference/3dgra/M3dgraControl.md)). |
| `M_COLORMAP_GRAYSCALE` | Specifies to use a predefined LUT buffer with a grayscale colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from black to white as the indices increase. Note that this colormap is effectively the same as no colormap, except that you can apply the [`M_FLIP`](../../Reference/3ddisp/M3ddispLut.md) combination value to invert the grayscale values.

*[Image: colormap_grayscale.png]* |
| `M_COLORMAP_HOT` | Specifies to use a predefined LUT buffer with a hot colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from black, to red, to yellow, and then, to white along the RGB cube as the indices increase. This colormap can be useful for displaying infrared images.

*[Image: Hot_colormap.png]* |
| `M_COLORMAP_HUE` | Specifies to use a predefined LUT buffer with a hue colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap is cyclic and transitions from red, to yellow, to green, to cyan, to blue, to magenta, and then, to red along the edge of the hue circle as the indices increase. This colormap is useful for displaying the hue component of an HSL image.

*[Image: Hue_colormap.png]* |
| `M_COLORMAP_JET` | Specifies to use a predefined LUT buffer with a jet colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from dark blue, to blue, to cyan, to yellow, to red, and then, to dark red along the RGB cube as the indices increase. This colormap can be useful for displaying 3D elevation maps and thermal imaging.

*[Image: Jet_colormap.png]* |
| `M_COLORMAP_SPECTRUM` | Specifies to use a predefined LUT buffer with a spectrum colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from red, to yellow, to green, to blue, and then, to violet along the RGB cube as the indices increase. This colormap is a representation of the visual spectrum according to wavelengths. It can also be useful for displaying 3D elevation maps and thermal imaging.

*[Image: spectrum_colormap.png]* |
| `M_COLORMAP_TURBO` | Specifies to use a predefined LUT buffer with a turbo colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap transitions from dark blue, to blue, to cyan, to yellow, to red, and then, to dark red along the RGB cube as the indices increase. This colormap is similar to [`M_COLORMAP_JET`](../../Reference/3ddisp/M3ddispLut.md), but with smoother transitions.

*[Image: Turbo_colormap.png]* |
| `M_COLORMAP_WHEEL` | Specifies to use a predefined LUT buffer with a colorwheel colormap as the LUT of a point cloud graphic in a 3D graphics list. The predefined LUT buffer has 3 bands and has a size of 65536. The colormap is cyclic and transitions from blue, to magenta, to yellow, to green, and then, to blue again as the indices increase. This colormap is designed to be perceptually uniform (a given change in data is designed to be visually perceived as an equal change in color). This colormap can be useful for displaying 3D elevation maps.

*[Image: Wheel_colormap.png]* |
| `LUT buffer ID` | Specifies the identifier of the LUT buffer to use as the LUT of a point cloud graphic in a 3D graphics list. The LUT buffer must have been previously allocated using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md)with [`M_LUT`](../../Reference/buf/MbufAllocColor.md). Only 8-bit unsigned LUT buffers are supported. |

*For specifying to flip the LUT*

| Value | Description |
| --- | --- |
| `M_FLIP` | Specifies to flip the LUT's values (inverse LUT). |

### `Operation` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
