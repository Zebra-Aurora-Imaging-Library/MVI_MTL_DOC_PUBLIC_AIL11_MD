---
doctype: Reference
module: disp
function: MdispLut
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispLut"
---

# MdispLut

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

> Associate a LUT buffer with a display.

## Syntax

```c
void MdispLut(
    AIL_ID DisplayId,  //out
    AIL_ID LutBufId    //in
)
```

## Description

This function associates a LUT buffer with the specified display. It can also be used to disassociate a LUT from the display.

If and when the display is selected ([`MdispSelect`](../../Reference/disp/MdispSelect.md)), the change required to produce the display (LUT) effect occurs. Aurora Imaging Library checks the target display to determine whether or not a LUT is supported. If not, an error is generated.

When displaying using a LUT, note the following:

- If the LUT buffer values are changed while the image is selected on the display, the changes will not take effect until the next call is made to [`MdispLut`](../../Reference/disp/MdispLut.md). That is, the LUT is not automatically updated when the LUT buffer is modified.
- You cannot select a 3-band image buffer to a display that is associated with a LUT buffer. In addition, the image buffer must be an 8- or 16-bit buffer. If these conditions are not respected, an error is generated.
- The view mode of the display cannot be set to [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md) or [`M_MULTI_BYTES`](../../Reference/disp/MdispControl.md).
- The number of LUT buffer entries must be the same as the maximum number of intensities that can be represented in the displayed buffer. In other words, if you want to map an 8-bit 1-band image (that is, an image that can have 256 intensities), your LUT must also have 256 entries.

To disassociate a LUT buffer from a display, call [`MdispLut`](../../Reference/disp/MdispLut.md) with [`M_DEFAULT`](../../Reference/disp/MdispLut.md).

For more information, see [Lookup tables in general](../../UserGuide/C24_Lookup_tables/S01_Lookup_tables_in_general.md) and [Mapping 1-band images through a LUT upon display](../../UserGuide/C25_Displaying_an_image/S09_Mapping_1band_images_through_a_LUT_upon_display.md).

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the display with which to associate the LUT buffer.

### `LutBufId` *(in, AIL_ID)*

Specifies a LUT buffer or predefined distinct colormap. This parameter should be set to one of the following values:

*For the identifier of the previously allocated LUT buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that any association to a LUT buffer is removed from the specified display. |
| `M_COLORMAP_DISTINCT_256` | Specifies the LUT buffer with a distinct colormap as the LUT buffer to associate with the display. This colormap is designed to maximize the visual difference between consecutive colors. This colormap can be useful for colorizing images containing discrete objects, such as labeled blobs.

Note that this colormap is periodic, meaning that if the given LUT buffer has more than 256 entries, the remaining entries will be filled starting from the first color in the colormap.

*[Image: Distinct_colormap.png]* |
| `M_COLORMAP_GRAYSCALE` | Specifies the LUT buffer with a grayscale colormap as the LUT buffer to associate with the display. This colormap transitions from black to white as the indices increase.

*[Image: colormap_grayscale.png]* |
| `M_COLORMAP_HOT` | Specifies the LUT buffer with a hot colormap as the LUT buffer to associate with the display. This colormap transitions from black, to red, to yellow, and then, to white along the RGB cube as the indices increase. This colormap can be useful for displaying infrared images.

*[Image: Hot_colormap.png]* |
| `M_COLORMAP_HUE` | Specifies the LUT buffer with a hue colormap as the LUT buffer to associate with the display. This colormap is cyclic and transitions from red, to yellow, to green, to cyan, to blue, to magenta, and then, to red along the edge of the hue circle as the indices increase. This colormap is suitable to be repeated. This colormap is useful for displaying the hue component of an HSL image.

*[Image: Hue_colormap.png]* |
| `M_COLORMAP_JET` | Specifies the LUT buffer with a jet colormap as the LUT buffer to associate with the display. This colormap transitions from dark blue, to blue, to cyan, to yellow, to red, and then, to dark red along the RGB cube as the indices increase. This colormap can be useful for displaying 3D elevation maps and thermal imaging.

*[Image: Jet_colormap.png]* |
| `M_COLORMAP_SPECTRUM` | Specifies the LUT buffer with a spectrum colormap as the LUT buffer to associate with the display. This colormap transitions from red, to yellow, to green, to blue, and then, to violet along the RGB cube as the indices increase. This colormap is a representation of the visual spectrum according to wavelengths. It can also be useful for displaying 3D elevation maps and thermal imaging.

*[Image: spectrum_colormap.png]* |
| `M_COLORMAP_TURBO` | Specifies the LUT buffer with a turbo colormap as the LUT buffer to associate with the display. This colormap is an improved version of the jet colormap, with smoother color transitions. It transitions from dark blue, to blue, to cyan, to yellow, to red, and then, to dark red along the RGB cube as the indices increase. This colormap can be useful for displaying 3D elevation maps and thermal imaging.

*[Image: Turbo_colormap.png]* |
| `M_COLORMAP_WHEEL` | Specifies the LUT buffer with a colorwheel colormap as the LUT buffer to associate with the display. This colormap is cyclic and transitions from blue, to magenta, to yellow, to green, and then, to blue again as the indices increase. This colormap is designed to be perceptually uniform (a given change in data is designed to be visually perceived as an equal change in color). This colormap is suitable to be repeated. This colormap can be useful for displaying 3D elevation maps.

*[Image: Wheel_colormap.png]* |
| `M_PSEUDO` | Specifies the default pseudo-color LUT buffer as the LUT buffer to associate with the display. When this LUT is used and displaying an 8-bit image buffer, each gray intensity is displayed in a different color. |
| `LUT buffer identifier` | Specifies the identifier of the custom LUT buffer (allocated with [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) with [`M_LUT`](../../Reference/buf/MbufAllocColor.md)) to associate with the display.

If you associate a three-band LUT buffer (RGB) with the display, and then select an image buffer to the display, the same image data is mapped through each band of the LUT, typically resulting in a color display effect (depends on the LUT values). |
