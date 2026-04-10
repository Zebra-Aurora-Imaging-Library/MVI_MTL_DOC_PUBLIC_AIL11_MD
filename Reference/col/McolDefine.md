---
doctype: Reference
module: col
function: McolDefine
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolDefine"
---

# McolDefine

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

> Define or delete a color-sample or color element.

## Syntax

```c
void McolDefine(
    AIL_ID     ContextId,         //in
    AIL_ID     SrcImageBufId,     //in
    AIL_INT    UserLabelOrIndex,  //in
    AIL_INT64  ColorSampleType,   //in
    AIL_DOUBLE Param1,            //in
    AIL_DOUBLE Param2,            //in
    AIL_DOUBLE Param3,            //in
    AIL_DOUBLE Param4             //in
)
```

## Description

This function allows you to add a color-sample to, or remove a color-sample from, a color matching or relative color calibration context. You can also use this function to add color elements to, or remove elements from, an existing color-sample. When defining a color-sample, you are, by default, defining the first color element of the color-sample. To define a color-sample (and consequently the first color element), you can use an image or three explicit color band values.

For a relative color calibration context, you must use this function to define or delete the reference color-sample. Information about color-samples applies to the reference color-sample unless otherwise specified.

A color matching or relative color calibration context must have one or more color-samples. A relative color calibration context must have one reference color-sample.

Aurora Imaging Library uses color-samples to execute [`McolMatch`](../../Reference/col/McolMatch.md) (color matching) and [`McolTransform`](../../Reference/col/McolTransform.md) (relative color calibration). Before calling these functions, you must preprocess the context that holds the color-samples, using [`McolPreprocess`](../../Reference/col/McolPreprocess.md). Whenever you add, add to, delete, or delete from a color-sample, you must preprocess the context for these changes to take effect. You must add, or add to, color-samples one at a time; however, you can delete all color-samples at once.

When defining image type color-samples for a color matching context, Aurora Imaging Library performs a color estimation, which is based on color statistics, such as the mean. The estimate is considered the color of the color-sample and is used in subsequent operations, such as [`McolMatch`](../../Reference/col/McolMatch.md) and [`McolDraw`](../../Reference/col/McolDraw.md). When defining image type color-samples for a relative color calibration context, Aurora Imaging Library does not typically perform a color estimation. Aurora Imaging Library considers the color of the color-sample to be the color data of each individual pixel. This is what Aurora Imaging Library uses in subsequent operations that require color-samples, such as [`McolTransform`](../../Reference/col/McolTransform.md) and [`McolDraw`](../../Reference/col/McolDraw.md). In some cases, you can use the mean color of the color-sample instead ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).

The size of color-samples does not usually affect the operations that use them.

Color-sample indices start at 0. Each subsequent color-sample you add to the context is given a sequential index number. If you delete a color-sample, Aurora Imaging Library shifts all entries with higher indices down by one. This convention also applies when adding or deleting a color element to or from a color-sample. The reference color-sample does not have an index (there can only be one).

You can also assign a label to a color-sample. If you don't, Aurora Imaging Library assigns one automatically. To change it afterward, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAMPLE_LABEL_VALUE`](../../Reference/col/McolControl.md). Adding color elements to, or removing color elements from, a color-sample does not change its index or label. You cannot assign a label to the reference color-sample (there can be only one, to which Aurora Imaging Library automatically assigns a label).

You can limit the [`McolMatch`](../../Reference/col/McolMatch.md) or [`McolTransform`](../../Reference/col/McolTransform.md) operation to a region of the image buffer, using a rectangular region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

Aurora Imaging Library assumes all the color you are using is in the context's source color space ([`McolAlloc`](../../Reference/col/McolAlloc.md)). Therefore all color elements that you add must be consistent. For example, if you allocate an RGB context, bands 0, 1, and 2 are assumed to be red, green, and blue; you will therefore not receive an error if you try to match RGB target areas with HSL color-samples. Similarly, you will not receive an error if you try to match a 16-bit target area with an 8-bit color-sample. Even when the color-samples are inconsistent, Aurora Imaging Library still processes them as is, which can produce misleading results. However, Aurora Imaging Library will not allow you to add color elements of different types to the same color-sample. If you do, you will receive an error. For relative color calibration, all color data must be RGB. Aurora Imaging Library processes it as RGB even if it is not.

Color space encoding must be set according to the actual dynamic range of your color elements. By default, Aurora Imaging Library assumes an 8-bit color space encoding (regardless of the buffer's depth). For color matching, you can specify a different encoding, such as 16-bit, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_ENCODING`](../../Reference/col/McolControl.md). For relative color calibration, Aurora Imaging Library always assumes 8-bit. No other encoding can be set.

You can use [`McolMask`](../../Reference/col/McolMask.md) to mask regions of a color element in a specified color-sample.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the context in which to add or delete a color-sample. The context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md) or [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md). If you are adding or deleting a color element from a color-sample, this parameter specifies the context in which the color-sample you wish to add or delete elements from is located.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer to use to define or add elements to a color-sample. If you are defining or adding elements to a color-sample from three explicit color band values, or if you are deleting color-samples or color elements, set this parameter to `M_NULL`.

### `UserLabelOrIndex` *(in, AIL_INT)*

Specifies a color-sample. You can add the color-sample to, or delete the color-sample (or all color-samples) from, the specified context ([`ContextId`](../../Reference/col/McolDefine.md)). You can also add color elements to, or delete color elements from, the color-sample.

*For specifying a color-sample*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Adds a color-sample to the context and automatically assigns it a label. The label is the next available numerical value following the highest label defined. |
| `M_REFERENCE_SAMPLE_ITEM` | Specifies the index of a color element (item), relative to the reference color-sample. Use **M_REFERENCE_SAMPLE_ITEM()** to delete a color element from the reference color-sample. **M_REFERENCE_SAMPLE_ITEM()** only applies to relative color calibration contexts. |
| `M_SAMPLE_INDEX` | Specifies the index of an existing color-sample. Use **M_SAMPLE_INDEX()** to delete a color-sample from the context, or to add a color element to a color-sample. |
| `M_SAMPLE_INDEX_ITEM` | Specifies the index of a color element (item), relative to a color-sample index. Use **M_SAMPLE_INDEX_ITEM()** to delete a color element from a color-sample. |
| `M_SAMPLE_LABEL` | Specifies the label of a new or an existing color-sample. Use **M_SAMPLE_LABEL()** to: add a new color-sample to the context, add a color element to an existing color-sample, or delete an existing color-sample from the context. |
| `M_SAMPLE_LABEL_ITEM` | Specifies the index of a color element (item), relative to a color-sample label. Use **M_SAMPLE_LABEL_ITEM()** to delete a color element from a color-sample. |
| `M_ALL` | Specifies all color-samples. Use [`M_ALL`](../../Reference/col/McolDefine.md) to delete all color-samples from the context. For relative color calibration contexts, [`M_ALL`](../../Reference/col/McolDefine.md) also deletes the reference color-sample. |
| `M_REFERENCE_SAMPLE` | Specifies the reference color-sample. Use [`M_REFERENCE_SAMPLE`](../../Reference/col/McolDefine.md) to: add the reference color-sample to the context, delete the reference color-sample from the context, or add a color element to the reference color-sample. Aurora Imaging Library automatically assigns the last possible label value (2<sup>12-1</sup>) to [`M_REFERENCE_SAMPLE`](../../Reference/col/McolDefine.md). [`M_REFERENCE_SAMPLE`](../../Reference/col/McolDefine.md) only applies to relative color calibration contexts. |

### `ColorSampleType` *(in, AIL_INT64)*

Specifies the type of the color-sample or color element.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the color-sample or color element. Its definition is dependent on the color-sample or color element type chosen.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the color-sample or color element. Its definition is dependent on the color-sample or color element type chosen.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the color-sample or color element. Its definition is dependent on the color-sample or color element type chosen.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the color-sample or color element. Its definition is dependent on the color-sample or color element type chosen.

## Parameter Associations

### For adding a color-sample or color element

The following [`ColorSampleType`](../../Reference/col/McolDefine.md) parameter settings can be used to add a color-sample to the context, or a color element to the specified color-sample.

---

### `M_IMAGE`

Specifies to add a color-sample or color element using the specified region of the source image. In this case, you must set the [`SrcImageBufId`](../../Reference/col/McolDefine.md) parameter to the identifier of an 8- or 16-bit color (3-band) unsigned image buffer, if you are using a color matching context. For a relative color calibration context, the buffer must be an 8-bit RGB color (3-band) image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the X-offset is equal to 0. |
| `Value` | Specifies the X-offset, in pixels. |
| `M_DEFAULT` | Specifies that the Y-offset is equal to 0. |
| `Value` | Specifies the Y-offset, in pixels. |
| `M_DEFAULT` | Specifies that the width is from the origin of the color-sample or color element region to the end of the source image, in the X-direction. |
| `Value` | Specifies the width, in pixels. |
| `M_DEFAULT` | Specifies that the height is from the origin of the color-sample or color element region to the end of the source image, in the Y-direction. |
| `Value` | Specifies the height, in pixels. |

---

### `M_TRIPLET`

Specifies to add a color-sample or color element using three explicit values for the color bands (RGB, HSL, or CIELAB). In this case, you must set the [`SrcImageBufId`](../../Reference/col/McolDefine.md) parameter to [`M_NULL`](../../Reference/col/McolDefine.md).

### Combination Constants — For adding elements to an already existing color-sample

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify additional color elements to add to an already existing color-sample.

| Value | Description |
| --- | --- |
| `M_ADD_COLOR_TO_SAMPLE` | Adds the specified color element to the specified color-sample. If the specified color-sample does not already exist, you will get an error. The color estimation that Aurora Imaging Library performs for the color-sample uses the new elements as well as the existing element(s). Aurora Imaging Library considers the resulting estimate the color of the color-sample and uses it in subsequent operations, such as color matching, relative color calibration, and drawing.  All color elements in a color-sample must have the same type. For example, if the existing color-sample type is [`M_IMAGE`](../../Reference/col/McolDefine.md), its additional color element(s) must also be [`M_IMAGE`](../../Reference/col/McolDefine.md). Similarly, if the color-sample is [`M_TRIPLET`](../../Reference/col/McolDefine.md), the additional element(s) must be [`M_TRIPLET`](../../Reference/col/McolDefine.md).  For [`M_IMAGE`](../../Reference/col/McolDefine.md), you must ensure that the source image from which to define the additional color element(s) has the same format as the original source image used to define the existing color-sample. |

### For deleting color-samples from the context, or color elements from the specified color-sample

The following [`ColorSampleType`](../../Reference/col/McolDefine.md) parameter setting can be used to remove color-samples from the context, or color elements from the specified color-sample. In this case, you must set the [`SrcImageBufId`](../../Reference/col/McolDefine.md) parameter to [`M_NULL`](../../Reference/col/McolDefine.md), and [`Param1`](../../Reference/col/McolDefine.md) to [`Param4`](../../Reference/col/McolDefine.md) to [`M_DEFAULT`](../../Reference/col/McolDefine.md).

---

### `M_DELETE`

Specifies to delete one or all color-samples or color elements, depending on the [`UserLabelOrIndex`](../../Reference/col/McolDefine.md) parameter setting.  Note that deleting the last remaining color-sample element (item) from a color-sample is equivalent to deleting the color-sample itself.
