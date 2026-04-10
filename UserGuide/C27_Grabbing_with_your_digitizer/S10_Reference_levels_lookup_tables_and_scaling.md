---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Reference_levels_lookup_tables_and_scaling
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Reference levels lookup tables and scaling"
---

# Reference levels, lookup tables, and scaling

Aurora Imaging Library provides functions to improve the appearance of a grabbed image on input (if your hardware allows it). You can adjust the brightness and contrast of the images, as well as the hue and saturation for color grabs, by fine-tuning the controls of the analog-to-digital converters in your system. You can also correct and precondition the input data prior to storing it, through scaling, or by mapping it through an input LUT.

## Black and white reference levels

When digitizing images, the black and white reference levels determine the zero and full-scale levels, respectively, of the input voltage range. The analog-to-digital converters convert any voltage above the white reference level to the maximum pixel value, and any voltage below the black reference level to a zero pixel value.

Zebra digitizers support fine-tuning of these reference levels. By reducing or increasing either or both the black and white reference levels, you affect the brightness of the image. By reducing one reference level and increasing the other, you affect the contrast of the image.

*[Image: referen.png]*

Aurora Imaging Library linearly represents the distance between the minimum and maximum voltages. The same is done for the white reference level adjustment range. These units are the values by which you can adjust the specified reference level, using [`MdigControl`](../../Reference/dig/MdigControl.md).

To calculate the value to pass to [`MdigControl`](../../Reference/dig/MdigControl.md), use the following equation with the appropriate voltages specified for your particular board.

*[Image: RefLevelEq.png]*

The smallest voltage increment supported by your board can differ such that consecutive reference-level settings might produce the same result.

Note, the new reference level might not take effect until the next grab, at which point, a certain amount of delay might be incurred as the hardware adjusts to the reference-level changes.

## Color image reference levels

When grabbing composite color images, [`MdigControl`](../../Reference/dig/MdigControl.md) provides specific control parameters to adjust the levels of contrast, brightness, hue, and saturation. These levels can be set to values from 0 to 255; use values appropriate for your particular board.

## Mapping grabbed data through a LUT

If supported by your digitizer, grabbed data is mapped through a physical lookup table (LUT). By default, the physical LUT is transparent; that is, it is loaded with data so that each input pixel is mapped to its original value. You can load the physical LUT with your own custom LUT data if, for example, you want to correct or precondition grabbed data. To do so, copy a LUT buffer to the digitizer's physical input LUT, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md). Aurora Imaging Library uses the data format (DCF) of the digitizer to determine whether a physical LUT is supported. If it is not, an error is generated.

The characteristics of the LUT buffer and the digitizer's DCF establishes how the physical LUT is actually configured.

| Configuration of the digitizer's LUT | Determining factor |
| --- | --- |
| Number of entries in the digitizer's physical LUT | Data being grabbed (DCF) |
| Depth (8- or 16-bit) | LUT buffer |
| Number of bands | DCF & LUT buffer |

The digitizer's physical LUT is typically configured to have the same number of components (bands) as either the LUT buffer or the data to be grabbed (determined by the digitizer's DCF), depending on which has more bands. The digitizer's physical LUT is also configured to have the same number of entries as the maximum possible value per band of the data to grab. The depth of a digitizer's physical LUT is configured to be either 8- or 16-bits per band, depending on if the LUT buffer depth is 8-bit or 16-bit, respectively. Cameras, however, are not so limited. When dealing with a 10- or 12-bit camera, use a 16-bit destination grab buffer and load the digitizer's physical LUT with the difference zero-padded.

To copy the data from a LUT buffer to the digitizer's physical LUT, the number of entries in the LUT buffer must match those of the digitizer's physical LUT. In addition, if the digitizer's physical LUT cannot support the depth of the LUT buffer, an error will occur.

LUT buffer data is loaded into the digitizer's physical LUT, as follows:

| LUT buffer | Digitizer's physical LUT | Result |
| --- | --- | --- |
| 1 band | 1 band | The LUT buffer is copied directly into the digitizer's physical LUT. |
| 1 band | 3 band | The LUT buffer is copied into each component of the digitizer's physical LUT. |
| 3 band | 1 band | The first band of the LUT buffer is copied into the digitizer's physical LUT. |
| 3 band | 3 band | Each of the LUT buffer's bands are copied into the corresponding component of the digitizer's physical LUT. |

If the destination grab buffer depth is larger than that of the digitizer's physical LUT, the bits from the physical LUT are copied over and the remaining upper bits in the destination grab buffer are set to 0. If the digitizer's physical LUT depth is greater than that of the destination host grab buffer, the most-significant bits of the data (the non-zero values) are used when the data is grabbed.

To revert back to using a transparent LUT, you must copy the default LUT ([`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md) set to [`M_DEFAULT`](../../Reference/dig/MdigControl.md)) to the digitizer's physical input LUT.

## Scaling

The [`MdigControl`](../../Reference/dig/MdigControl.md) function allows you to scale grabbed data horizontally and vertically. If you scale grabbed data, the stored image size is different from the original image by the specified factors in the X- and/or Y-direction. The scaled image is written in contiguous locations in the image buffer, starting from the top-left corner. For example, if you set both the X- and Y-scaling factors to 0.5, only one column and one row out of two are written to the image buffer (starting with the first row and column).

*[Image: SUBSAM.png]*

The X- and Y-scaling factors are independent. Note, depending on the Zebra frame grabber and camera used, some scaling factors might not be available.

To disable scaling, set the scaling factors to 1.
