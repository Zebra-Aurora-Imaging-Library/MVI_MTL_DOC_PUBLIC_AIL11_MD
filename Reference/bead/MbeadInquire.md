---
doctype: Reference
module: bead
function: MbeadInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadInquire"
---

# MbeadInquire

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

> Inquire information about a bead context, template, or result.

## Syntax

```c
AIL_INT MbeadInquire(
    AIL_ID    ContextOrResultBeadId,  //in
    AIL_INT   LabelOrIndex,           //in
    AIL_INT64 InquireType,            //in
    void *    UserVarPtr              //out
)
```

## Description

This function inquires information about a specified bead context, a template contained within the context, or a bead result buffer. You can also use this function to inquire about operations performed with [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md). Values are returned in the unit in which they were set (that is, either pixel or world units), using [`MbeadControl`](../../Reference/bead/MbeadControl.md) and [`M_..._INPUT_UNITS`](../../Reference/bead/MbeadControl.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/bead/MbeadControl.md) (for example, in [`MbeadControl`](../../Reference/bead/MbeadControl.md)), [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) will return [`M_DEFAULT`](../../Reference/bead/MbeadInquire.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/bead/MbeadInquire.md) to the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter.

## Parameters

### `ContextOrResultBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context or the identifier of the bead result buffer about which to inquire. Both the bead context and result buffer must have been previously allocated on the required system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md) or [`MbeadAllocResult`](../../Reference/bead/MbeadAllocResult.md), respectively.

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the bead template about which to inquire, or specifies that you are inquiring about a global setting of the bead context or result. Unless otherwise specified, the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter values require that the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter specifies the identifier of a bead context.

*For specifying what to inquire about*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) is set to a bead context, the default is the same as [`M_CONTEXT`](../../Reference/bead/MbeadInquire.md).

If [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) is set to a bead result, the default is the same as [`M_GENERAL`](../../Reference/bead/MbeadInquire.md). |
| `M_TEMPLATE_INDEX` | Specifies the index of an existing template about which to inquire. |
| `M_TEMPLATE_LABEL` | Specifies the label of an existing template about which to inquire. |
| `M_CONTEXT` | Inquires information about a global setting of a bead context. |
| `M_GENERAL` | Inquires information about a setting of a bead result buffer. When using [`M_GENERAL`](../../Reference/bead/MbeadInquire.md), [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) must specify a bead result buffer. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### To inquire about a bead template setting for the training phase

To inquire about a bead template setting for the training phase, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to one of the following values. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to a specific template, using either **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**.

---

### `M_INDIVIDUAL_WIDTH_NOMINAL`

Inquires the specified width of the template at the position of each of its vertices. This value applies only to templates whose path follows a polyline.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width. |

---

### `M_INTENSITY_NOMINAL_MODE`

Inquires how to establish the nominal pixel intensity of the template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to establish the nominal pixel intensity from the training image. |
| `M_DISABLE` *(default)* | Specifies that [`M_INTENSITY_NOMINAL_MODE`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `M_USER_DEFINED` | Specifies that you will explicitly set the pixel intensity, using [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadInquire.md). |

---

### `M_POSITION_X`

Inquires the X-coordinates of the vertices in a template whose path follows a polyline. You can add, delete, or modify a template's vertices using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates of the template's vertices. |

---

### `M_POSITION_Y`

Inquires the Y-coordinates of the vertices in a template whose path follows a polyline. You can add, delete, or modify a template's vertices using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinates of the template's vertices. |

---

### `M_SIZE`

Inquires the number of vertices in a template whose path follows a polyline. You can add, delete, or modify a template's vertices using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of points. |

---

### `M_TEMPLATE_CIRCLE_CENTER_X`

Inquires the X-coordinate of the center of a template whose path follows a circle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the center's X-coordinate, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TEMPLATE_CIRCLE_CENTER_Y`

Inquires the Y-coordinate of the center of a template whose path follows a circle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the center's Y-coordinate, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TEMPLATE_CIRCLE_RADIUS`

Inquires the radius of a template whose path follows a circle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the radius, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TEMPLATE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_CLOSEST_POINT_MAX_DISTANCE`](../../Reference/bead/MbeadInquire.md), [`M_FAIL_WARNING_OFFSET`](../../Reference/bead/MbeadInquire.md), [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadInquire.md), [`M_OFFSET_MAX`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_CIRCLE_CENTER_X`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_CIRCLE_CENTER_Y`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_CIRCLE_RADIUS`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_SEGMENT_END_X`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_SEGMENT_END_Y`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_SEGMENT_START_X`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_SEGMENT_START_Y`](../../Reference/bead/MbeadInquire.md), [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadInquire.md), [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadInquire.md), and [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md) inquire types, as well as the units with which to interpret the template's position and dimension values specified using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md). This essentially inquires the input coordinate system used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_TEMPLATE_SEGMENT_END_X`

Inquires the X-coordinate of the end of a template whose path follows a segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate of the end of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TEMPLATE_SEGMENT_END_Y`

Inquires the Y-coordinate of the end of a template whose path follows a segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate of the end of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TEMPLATE_SEGMENT_START_X`

Inquires the X-coordinate of the start of a template whose path follows a segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate of the start of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TEMPLATE_SEGMENT_START_Y`

Inquires the Y-coordinate of the start of a template whose path follows a segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate of the start of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TRAINING_BOX_HEIGHT`

Inquires the height of the search boxes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the height, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TRAINING_BOX_INPUT_UNITS`

Inquires the units with which to interpret the [`M_TRAINING_BOX_HEIGHT`](../../Reference/bead/MbeadInquire.md), [`M_TRAINING_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md), [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadInquire.md), and [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md) inquire types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_TRAINING_BOX_SPACING`

Inquires the distance between the search boxes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` *(default)* | Specifies the spacing, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TRAINING_BOX_WIDTH`

Inquires the width of the search boxes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the width, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TRAINING_PATH`

Inquires how Aurora Imaging Library establishes the path of the bead template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCLE` | Specifies that the path of the bead template follows a fixed circle defined with [`M_TEMPLATE_CIRCLE_CENTER_X`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_CIRCLE_CENTER_Y`](../../Reference/bead/MbeadInquire.md), and [`M_TEMPLATE_CIRCLE_RADIUS`](../../Reference/bead/MbeadInquire.md). |
| `M_POLYLINE` | Specifies that the path of the bead template follows a fixed polyline. |
| `M_POLYLINE_SEED` *(default)* | Specifies that the path of the bead template follows a polyline that is refined by the training image set with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). |
| `M_SEGMENT` | Specifies that the path of the bead template follows a fixed segment defined with [`M_TEMPLATE_SEGMENT_START_X`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_SEGMENT_START_Y`](../../Reference/bead/MbeadInquire.md), [`M_TEMPLATE_SEGMENT_END_X`](../../Reference/bead/MbeadInquire.md), and [`M_TEMPLATE_SEGMENT_END_Y`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TRAINING_WIDTH_NOMINAL`

Inquires the width with which to validate the template's automatically established nominal width.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` | Specifies the width, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_TRAINING_WIDTH_SCALE_MAX`

Inquires the scale factor by which to determine the upper limit (maximum permitted scale) of the bead width.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_TRAINING_WIDTH_SCALE_MAX`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` *(default)* | Specifies the maximum scale factor. |

---

### `M_TRAINING_WIDTH_SCALE_MIN`

Inquires the scale factor by which to determine the lower limit (minimum permitted scale) of the bead width.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_TRAINING_WIDTH_SCALE_MIN`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` *(default)* | Specifies the minimum scale factor. |

---

### `M_WIDTH_NOMINAL`

Inquires the nominal width to use, when it is explicitly defined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the nominal width, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_WIDTH_NOMINAL_MODE`

Inquires how to establish the nominal width of the bead template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_CONTINUOUS` | Specifies that the bead's nominal width is automatically established with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), where the corresponding width at each vertex in the template can have its own unique value. |
| `M_AUTO_UNIFORM` *(default)* | Specifies that the bead's nominal width is automatically established with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), where the corresponding width at all of the template's vertices must have the same value. |
| `M_USER_DEFINED` | Specifies that you must explicitly set the bead's nominal width. |

### To inquire about a bead template setting for the training and verification phases

To inquire about a bead template setting for the training and verification phases, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to one of the following values. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to a specific template, using either **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**.

---

### `M_FOREGROUND_VALUE`

Inquires whether the foreground of the measured bead should be darker or lighter than the background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` | Specifies that the foreground is darker than the background. |
| `M_FOREGROUND_WHITE` *(default)* | Specifies that the foreground is lighter than the background. |

---

### `M_SMOOTHNESS`

Inquires the degree of noise reduction that Aurora Imaging Library uses when performing the edge extraction of the bead from an image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value. |

---

### `M_THRESHOLD_MODE`

Inquires how to establish the threshold that Aurora Imaging Library must use when performing the edge extraction of the bead from an image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED` *(default)* | Specifies that the threshold is explicitly defined with [`M_THRESHOLD_VALUE`](../../Reference/bead/MbeadInquire.md). |

---

### `M_THRESHOLD_VALUE`

Inquires the value beneath which a grayscale variation within the image is not considered an edge of the bead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the threshold value. |

### To inquire about a trained setting of a bead template

To inquire about a trained setting of a bead template, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to one of the following values. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to a specific template, using either **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**.  Aurora Imaging Library typically bases trained values on their related training values and modifies them accordingly when you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). For example, Aurora Imaging Library adjusts the specified spacing for the search boxes ([`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md)) to achieve an even distribution of search boxes along the template's path ([`M_TRAINED_BOX_SPACING`](../../Reference/bead/MbeadInquire.md)). If you do not retrain the templates, the trained value represents the value Aurora Imaging Library uses for verification.

---

### `M_TRAINED_BOX_HEIGHT`

Inquires the trained height of the search boxes. Aurora Imaging Library bases this value on [`M_TRAINING_BOX_HEIGHT`](../../Reference/bead/MbeadInquire.md) and internally modifies it during the training phase to achieve the best possible trained height.

| Value | Description |
| --- | --- |
| `Value` | Specifies the trained height of the search boxes. |

---

### `M_TRAINED_BOX_SPACING`

Inquires the trained distance between the search boxes in which to establish the trained points. Aurora Imaging Library bases this value on [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadInquire.md) and internally modifies it during the training phase to achieve an even distribution of trained search boxes.

| Value | Description |
| --- | --- |
| `Value` | Specifies the trained distance between the search boxes. |

---

### `M_TRAINED_BOX_WIDTH`

Inquires the trained width of the search boxes. Aurora Imaging Library bases this value on a variety of settings, such as [`M_TRAINING_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md), to achieve the best possible trained width.

| Value | Description |
| --- | --- |
| `Value` | Specifies the trained width of the search boxes. |

---

### `M_TRAINED_INDIVIDUAL_WIDTH_NOMINAL`

Inquires the trained width of the template at the position of each individual trained point. Aurora Imaging Library bases this value on a variety of settings, such as [`M_WIDTH_...`](../../Reference/bead/MbeadInquire.md), to achieve the best possible trained widths.

| Value | Description |
| --- | --- |
| `Value` | Specifies the trained width for each trained point. |

---

### `M_TRAINED_POSITION_X`

Inquires the trained X-position of the trained points. Aurora Imaging Library bases this value on a variety of settings, such as [`M_TRAINING_PATH`](../../Reference/bead/MbeadInquire.md), to achieve the best possible trained positions.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the trained X-position for each trained point. |

---

### `M_TRAINED_POSITION_Y`

Inquires the trained Y-position of the trained points. Aurora Imaging Library bases this value on a variety of settings, such as [`M_TRAINING_PATH`](../../Reference/bead/MbeadInquire.md), to achieve the best possible trained positions.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the trained Y-position for each trained point. |

---

### `M_TRAINED_SIZE`

Inquires the number of trained points. Aurora Imaging Library considers the number of trained points in a template to be its size.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of trained points. |

---

### `M_TRAINED_WIDTH_NOMINAL`

Inquires the trained nominal width of the template. Aurora Imaging Library bases this value on a variety of settings, such as [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md), to achieve the best possible trained nominal width.

| Value | Description |
| --- | --- |
| `Value` | Specifies the trained nominal width of the bead. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### To inquire about a bead template setting for the verification phase

To inquire about a bead template setting for the verification phase, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to one of the following values. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to a specific template, using either **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**.

---

### `M_ACCEPTANCE`

Inquires the acceptance level for the score of the template's corresponding measured bead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level, as a percentage. |

---

### `M_ANGLE_ACCURACY_MAX_DEVIATION`

Inquires the maximum angular deviation from the automatically established angle with which to verify the width of the template's measured bead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the angular deviation, in degrees. |

---

### `M_BOX_WIDTH_MARGIN`

Inquires the additional width for the search boxes used to verify the template's measured bead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the additional width used for verification, as a percentage. |

---

### `M_FAIL_WARNING_OFFSET`

Inquires the additional width specified for the search box where a measured point can be found, but will have a failed status.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the width, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_GAP_MAX_LENGTH`

Inquires the maximum allowable length of one single gap, for the template's measured bead to have a passing status.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value >= 0.0` *(default)* | Specifies the maximum gap length, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_GAP_TOLERANCE`

Inquires the percentage of the total allowable gaps (all gaps added together), for the template's measured bead to have a passing status.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the total allowable gaps, as a percentage. |

---

### `M_INTENSITY_DELTA_NEG`

Inquires the lowest pixel intensity that is considered valid for the template's measured bead, relative to [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the lowest valid pixel intensity. |

---

### `M_INTENSITY_DELTA_POS`

Inquires the highest pixel intensity that is considered valid for the template's measured bead, relative to [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the highest valid pixel intensity. |

---

### `M_INTENSITY_NOMINAL`

Inquires the nominal pixel intensity of the template's measured bead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the nominal pixel intensity. |

---

### `M_OFFSET_MAX`

Inquires the maximum allowable distance between the position of the trained points in the template, and the position of the measured points in the target, for the measured bead to have a passing status.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_OFFSET_MAX`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` | Specifies the allowable offset, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_WIDTH_DELTA_NEG`

Inquires the valid delta negative tolerance between the width of the measured bead and the width of the template, for the measured bead to have a passing status.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` | Specifies the tolerance, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

---

### `M_WIDTH_DELTA_POS`

Inquires the valid delta positive tolerance between the width of the measured bead and the width of the template, for the measured bead to have a passing status.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadInquire.md) has no effect. |
| `Value > 0.0` | Specifies the tolerance, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

### To inquire about a global setting of a bead template

To inquire about a global bead template setting, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to one of the following values. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to a specific template, using either **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**, unless otherwise specified.

---

### `M_BEAD_TYPE`

Inquires the type of the bead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default type. |
| `M_BEAD_EDGE` | Specifies a bead template consisting of an edge. |
| `M_BEAD_STRIPE` | Specifies a bead template consisting of a stripe. |

---

### `M_INDEX_VALUE`

Inquires the index associated with the template. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to the label of a template.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the template label you specified is not valid (not associated with a template). |
| `Value >= 0` | Specifies the index value associated with the specified template label. |

---

### `M_LABEL_VALUE`

Inquires the label that Aurora Imaging Library automatically associated with the template when it was added to the context. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to the index of a template.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label. |

### To inquire about a global setting of a bead context

To inquire about a global setting of a bead context, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to one of the following values. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to [`M_CONTEXT`](../../Reference/bead/MbeadInquire.md).

---

### `M_ASSOCIATED_CALIBRATION`

Inquires the camera calibration context associated with the training image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Calibration context identifier` | Specifies the camera calibration context that is associated with the training image. |
| `M_NULL` | Specifies that there is no camera calibration context that has been associated with the training image using [`M_ASSOCIATED_CALIBRATION`](../../Reference/bead/MbeadControl.md). |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_NUMBER_OF_TEMPLATES`

Inquires the number of templates in the bead context. To add and remove templates, use [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of templates. |

---

### `M_STATUS`

Inquires the training status of the bead context. Aurora Imaging Library establishes the status after the last call to [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) (using the most current settings of the bead module).

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the context is completely trained. This indicates that each template in the context is trained. |
| `M_NOT_TRAINED` | Specifies that the context is not completely trained. This indicates that there are one or more templates in the context that are not trained. |
| `M_PARTIAL` | Specifies that the context is partially trained. This indicates that all templates in the context are either partially trained, or both partially and completely trained. [`M_PARTIAL`](../../Reference/bead/MbeadInquire.md) can also indicate that one or more sections of one or more templates were not found in the training image. |

---

### `M_TRAINING_IMAGE_SIZE_X`

Inquires the width of the internally saved training image.

| Value | Description |
| --- | --- |
| `0` | Specifies that there is no training image. |
| `Value` | Specifies the width of the training image. |

---

### `M_TRAINING_IMAGE_SIZE_Y`

Inquires the height of the internally saved training image.

| Value | Description |
| --- | --- |
| `0` | Specifies that there is no training image. |
| `Value` | Specifies the height of the training image. |

---

### `M_TRAINING_IMAGE_TYPE`

Inquires the data type and data depth of the internally saved training image.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no training image. |
| `M_UNSIGNED + 8` | Specifies that the training image is 8-bit unsigned. |

### To inquire about a global setting of a bead context or bead result buffer

To inquire about a global setting of a bead context or bead result buffer, the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to the following value. When inquiring about a bead context, set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to [`M_CONTEXT`](../../Reference/bead/MbeadInquire.md). When inquiring about a bead result buffer, set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead result buffer, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadInquire.md).

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which either the bead context or bead result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### To inquire about settings for operations with

To inquire about settings for operations with [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md), the [`InquireType`](../../Reference/bead/MbeadInquire.md) parameter can be set to the following value. In this case, you must set the [`ContextOrResultBeadId`](../../Reference/bead/MbeadInquire.md) parameter to the identifier of a bead context, and the [`LabelOrIndex`](../../Reference/bead/MbeadInquire.md) parameter to [`M_CONTEXT`](../../Reference/bead/MbeadInquire.md).

---

### `M_CLOSEST_POINT_MAX_DISTANCE`

Inquires the maximum distance specified for the [`M_CLOSEST_...`](../../Reference/bead/MbeadGetNeighbors.md) operations in [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the maximum distance is infinite. |
| `Value > 0.0` | Specifies the maximum distance, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadInquire.md). |

### Combination Constants — To inquire about the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — To inquire whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported for the bead context.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — To specify the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/bead/MbeadInquire.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/bead/MbeadInquire.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
