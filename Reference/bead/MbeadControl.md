---
doctype: Reference
module: bead
function: MbeadControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadControl"
---

# MbeadControl

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

> Control a bead context or template setting.

## Syntax

```c
void MbeadControl(
    AIL_ID     ContextBeadId,  //out
    AIL_INT    LabelOrIndex,   //in
    AIL_INT64  ControlType,    //in
    AIL_DOUBLE ControlValue    //in
)
```

## Description

This function allows you to control the specified setting for a bead context, or for the templates contained within the context. You can use these settings to manage the training ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)) and verification phases ([`MbeadVerify`](../../Reference/bead/MbeadVerify.md)). You can inquire most of these settings using [`MbeadInquire`](../../Reference/bead/MbeadInquire.md).

Settings for the training phase have an indirect affect on the verification phase, since it uses trained templates. Therefore if you modify a training phase setting, or a setting that applies to both the training and verification phases, you must re-call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) before the next call to [`MbeadVerify`](../../Reference/bead/MbeadVerify.md). Settings that are exclusively for the verification phase are not used by, and have no effect on, the training phase; if you modify such settings, you need not re-call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).

This function's settings affect either the template's training status (established after calling [`MbeadTrain`](../../Reference/bead/MbeadTrain.md)) or verification status (established after calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md)). Depending on the status, you might have to adjust the related settings. To retrieve information about the training status, use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md); to retrieve information about the verification status, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_STATUS`](../../Reference/bead/MbeadGetResult.md).

You can also use this function to control operations with [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md).

If you are specifying pixel units, make sure the pixel sizes are consistent across all camera calibrations (for example, pixel sizes should be the same in the training and verification images). When using different camera calibration contexts, it is recommended that you use world units.

## Parameters

### `ContextBeadId` *(out, AIL_ID)*

Specifies the identifier of the bead context to control. The bead context must have been previously allocated on the required system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the bead template (one or all) to control, or specifies that you are controlling a global setting of the bead context. This parameter should be set to one of the following values:

*For specifying what to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_TEMPLATE_INDEX` | Specifies the index of an existing template to control. |
| `M_TEMPLATE_LABEL` | Specifies the label of an existing template to control. |
| `M_ALL` | Applies the specified control setting to all the templates contained within the bead context. In this case, the control type setting must be supported by all the templates. |
| `M_CONTEXT` *(default)* | Controls a global setting of a bead context. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### To control bead templates for the training phase

The following [`ControlType`](../../Reference/bead/MbeadControl.md) and corresponding [`ControlValue`](../../Reference/bead/MbeadControl.md) parameter settings are used to control templates (one or all) in a bead context for the training phase ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadControl.md) parameter to a specific template (**M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**) or to all templates ([`M_ALL`](../../Reference/bead/MbeadControl.md)).

---

### `M_INTENSITY_NOMINAL_MODE`

Sets how Aurora Imaging Library establishes the nominal pixel intensity of the template. You can use the pixel intensity of the template when verifying beads. That is, Aurora Imaging Library can validate the brightness of each measured point according to the brightness of its associated trained point. Aurora Imaging Library calculates the intensity for each point at the center of its corresponding bead width.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to establish the nominal pixel intensity from the training image. |
| `M_DISABLE` *(default)* | Specifies that [`M_INTENSITY_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) has no effect. |
| `M_USER_DEFINED` | Specifies that you will explicitly set the pixel intensity, using [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_CIRCLE_CENTER_X`

Sets the X-coordinate of the center of a template whose path follows a circle. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_CIRCLE`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the center's X-coordinate, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_CIRCLE_CENTER_Y`

Sets the Y-coordinate of the center of a template whose path follows a circle. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_CIRCLE`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the center's Y-coordinate, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_CIRCLE_RADIUS`

Sets the radius of a template whose path follows a circle. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_CIRCLE`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the radius, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_INPUT_UNITS`

Sets the units with which to interpret the [`M_CLOSEST_POINT_MAX_DISTANCE`](../../Reference/bead/MbeadControl.md), [`M_FAIL_WARNING_OFFSET`](../../Reference/bead/MbeadControl.md), [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md), [`M_OFFSET_MAX`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_CIRCLE_CENTER_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_CIRCLE_CENTER_Y`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_CIRCLE_RADIUS`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_END_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_END_Y`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_START_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_START_Y`](../../Reference/bead/MbeadControl.md), [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadControl.md), [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadControl.md), and [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) control types, as well as the units with which to interpret the template's position and dimension values specified using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md). This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If world units are specified, calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) to draw features of the template, or calling [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) or [`MbeadVerify`](../../Reference/bead/MbeadVerify.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_TEMPLATE_SEGMENT_END_X`

Sets the X-coordinate of the end of a template whose path follows a segment. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_SEGMENT`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate of the end of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_SEGMENT_END_Y`

Sets the Y-coordinate of the end of a template whose path follows a segment. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_SEGMENT`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate of the end of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_SEGMENT_START_X`

Sets the X-coordinate of the start of a template whose path follows a segment. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_SEGMENT`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate of the start of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TEMPLATE_SEGMENT_START_Y`

Sets the Y-coordinate of the start of a template whose path follows a segment. This value only applies if [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) is set to [`M_SEGMENT`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate of the start of the segment, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TRAINING_BOX_HEIGHT`

Sets the height of the training box in which to establish the trained points.  *[Image: BeadTrainingSearchBoxHeight.png]*  When you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), Aurora Imaging Library uses the specified height of the training box to achieve the best height value based on all your settings (for example, if you use a training image). To inquire the actual height Aurora Imaging Library establishes (the trained height), use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_TRAINED_BOX_HEIGHT`](../../Reference/bead/MbeadInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the height, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TRAINING_BOX_INPUT_UNITS`

Sets the units with which to interpret the [`M_TRAINING_BOX_HEIGHT`](../../Reference/bead/MbeadControl.md), [`M_TRAINING_BOX_WIDTH`](../../Reference/bead/MbeadControl.md), [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md), and [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) control types. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If world units are specified, calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) to draw features of the template, or calling [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) or [`MbeadVerify`](../../Reference/bead/MbeadVerify.md), generates an error if the operation is not performed on a calibrated image. |

---

### `M_TRAINING_BOX_SPACING`

Sets the distance between the center of the training boxes in which to establish the trained points. Since Aurora Imaging Library establishes trained points at regular intervals along the template's path, the specified spacing is adjusted accordingly. To inquire the actual spacing Aurora Imaging Library uses, call [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_TRAINED_BOX_SPACING`](../../Reference/bead/MbeadInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md) has no effect.  If a template's path follows a fixed circle or a segment ([`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) set to [`M_CIRCLE`](../../Reference/bead/MbeadControl.md) or [`M_SEGMENT`](../../Reference/bead/MbeadControl.md)) and you disable spacing, Aurora Imaging Library internally creates the minimum number of critical points required to geometrically establish the path.  If a template's path follows a fixed polyline ([`M_POLYLINE`](../../Reference/bead/MbeadControl.md)) and you disable spacing, the trained points will be the same as the vertices specified when adding or modifying the template ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md)). Such vertices can, for example, come from a CAD file, where the exact path of the polyline is known (but not necessarily the width).  If a template's path follows a polyline that is refined by a training image ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)) and you disable spacing, the number of trained points will be the same as the number of vertices specified when adding or modifying the template ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md)), however Aurora Imaging Library will reposition them accordingly (with respect to the training image). |
| `Value > 0.0` *(default)* | Specifies the spacing, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TRAINING_BOX_WIDTH`

Sets the width of the training box in which to establish the trained points.  *[Image: BeadTrainingSearchBoxWidth.png]*  When you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), Aurora Imaging Library uses the specified width of the training box to achieve the best width value based on all your settings (for example, if you use a training image). To inquire the actual width Aurora Imaging Library establishes (the trained width), use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_TRAINED_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md). Note that Aurora Imaging Library does not typically alter the specified training box width.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the width, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TRAINING_PATH`

Sets how Aurora Imaging Library establishes the path of the bead template. Along the path, Aurora Imaging Library establishes trained points at regular intervals based on the specified spacing ([`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCLE` | Specifies that the path of the bead template follows a fixed circle defined with [`M_TEMPLATE_CIRCLE_CENTER_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_CIRCLE_CENTER_Y`](../../Reference/bead/MbeadControl.md), and [`M_TEMPLATE_CIRCLE_RADIUS`](../../Reference/bead/MbeadControl.md). The trained position of the path always follows the exact position of the specified circle. If the template contains vertices ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_ADD`](../../Reference/bead/MbeadTemplate.md) or [`M_INSERT`](../../Reference/bead/MbeadTemplate.md)), Aurora Imaging Library ignores them. |
| `M_POLYLINE` | Specifies that the path of the bead template follows a fixed polyline. The trained position of the path always follows the exact position of the specified polyline. To specify the polyline, you must set its vertices when adding the template using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md). You can also modify the vertices of the polyline with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) operations such as [`M_DELETE`](../../Reference/bead/MbeadTemplate.md) and [`M_INSERT`](../../Reference/bead/MbeadTemplate.md).  Note that with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md), you can also specify the vertices of a polyline (or polygon) from a 2D graphics list. A polygon is simply a polyline that Aurora Imaging Library automatically closes by connecting the final vertex to the first with a straight line. Any operation or setting that applies to polylines also applies to polygons. |
| `M_POLYLINE_SEED` *(default)* | Specifies that the path of the bead template follows a polyline that is refined by the training image set with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). To define the polyline, you must specify its vertices when adding the template using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_ADD`](../../Reference/bead/MbeadTemplate.md). You can also modify the vertices of a polyline with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) operations such as [`M_DELETE`](../../Reference/bead/MbeadTemplate.md) and [`M_INSERT`](../../Reference/bead/MbeadTemplate.md).  Note that with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md), you can also specify the vertices of a polyline (or polygon) from a 2D graphics list. A polygon is simply a polyline that Aurora Imaging Library automatically closes by connecting the final vertex to the first with a straight line. Any operation or setting that applies to polylines also applies to polygons. |
| `M_SEGMENT` | Specifies that the path of the bead template follows a fixed segment defined with [`M_TEMPLATE_SEGMENT_START_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_START_Y`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_END_X`](../../Reference/bead/MbeadControl.md), and [`M_TEMPLATE_SEGMENT_END_Y`](../../Reference/bead/MbeadControl.md). The trained position of the path always follows the exact position of the specified segment. If the template contains vertices ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_ADD`](../../Reference/bead/MbeadTemplate.md) or [`M_INSERT`](../../Reference/bead/MbeadTemplate.md)), Aurora Imaging Library ignores them. |

---

### `M_TRAINING_WIDTH_NOMINAL`

Sets the width with which to validate the template's automatically established nominal width. [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) only applies when [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) is set to [`M_AUTO_...`](../../Reference/bead/MbeadControl.md). The width of the measured bead automatically established with the training image must respect the nominal width specified, otherwise the template will be invalid. To set scaling factors based on the nominal width, use [`M_TRAINING_WIDTH_SCALE_MAX`](../../Reference/bead/MbeadControl.md) and [`M_TRAINING_WIDTH_SCALE_MIN`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value > 0.0` | Specifies the width, relative to the input coordinate system specified using [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_TRAINING_WIDTH_SCALE_MAX`

Sets the scale factor by which to determine the upper limit (maximum permitted scale) of the bead width. Aurora Imaging Library applies the scale factor to the nominal training width ([`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_TRAINING_WIDTH_SCALE_MAX`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value > 0.0` *(default)* | Specifies the maximum scale factor. For example, a maximum scale factor of 1.2 indicates that the width of the measured bead can be up to 20% bigger than the nominal training width specified. |

---

### `M_TRAINING_WIDTH_SCALE_MIN`

Sets the scale factor by which to determine the lower limit (minimum permitted scale) of the bead width. Aurora Imaging Library applies the scale factor to the nominal width ([`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_TRAINING_WIDTH_SCALE_MIN`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value > 0.0` *(default)* | Specifies the minimum scale factor. For example, a minimum scale factor of 0.8 indicates that the width of the measured bead can be up to 20% smaller than the nominal training width specified. |

---

### `M_WIDTH_NOMINAL`

Sets the nominal width to use, when it is explicitly defined. [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) only applies when [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) is set to [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md).  The width set for [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) applies to all vertices. To modify the width of one or more vertices, use [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the nominal width, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_WIDTH_NOMINAL_MODE`

Sets how to establish the nominal width of the bead template. If the bead is a stripe ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md)), the width refers to the thickness between the stripe's outer edges. If the bead is an edge ([`M_BEAD_EDGE`](../../Reference/bead/MbeadTemplate.md)), the width refers to the thickness of the edge's intensity transition.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_CONTINUOUS` | Specifies that the bead's nominal width is automatically established with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), where the corresponding width at each vertex in the template can have its own unique value. In this case, you must use a training image to train the template ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). |
| `M_AUTO_UNIFORM` *(default)* | Specifies that the bead's nominal width is automatically established with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), where the corresponding width at all of the template's vertices must have the same value. The width corresponds to the average bead width. In this case, you must use a training image to train the template ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). |
| `M_USER_DEFINED` | Specifies that you must explicitly set the bead's nominal width. To specify the nominal width of the bead, use [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md). To specify the width of the bead corresponding to one or more vertices in the template, use [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md). |

### To control measured beads for the verification phase

The following [`ControlType`](../../Reference/bead/MbeadControl.md) and corresponding [`ControlValue`](../../Reference/bead/MbeadControl.md) parameter settings are used to control the templates' (one or all) corresponding measured bead for the verification phase ([`MbeadVerify`](../../Reference/bead/MbeadVerify.md)). In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadControl.md) parameter to a specific template (**M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**) or to all templates ([`M_ALL`](../../Reference/bead/MbeadControl.md)).

---

### `M_ACCEPTANCE`

Sets the acceptance level for the score of the template's corresponding measured bead. The measured bead can only have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)) if its score is greater than or equal to the acceptance, regardless of any other setting. To retrieve the computed score, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_SCORE`](../../Reference/bead/MbeadGetResult.md). The score indicates, as a percentage, the amount of trained points in the template that have a corresponding measured point with a passing status, relative to the total number of trained points in the template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level, as a percentage. 100.0% indicates that, for every trained point in the template, there is a corresponding measured point that has a passing status. |

---

### `M_ANGLE_ACCURACY_MAX_DEVIATION`

Sets the maximum angular deviation between the angle of the bead template at each of its trained points, and the angle of the corresponding measured bead at each of its measured points. Aurora Imaging Library considers the angle represented by the trained and measured points to be along the theoretical line that is perpendicular between the point and its corresponding bead.  *[Image: BeadVerificationAngleReference.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the angular deviation, in degrees. If you specify 0° (no deviation), Aurora Imaging Library considers the angle of the measured bead at its measured points to be the same as the angle of the bead template at its corresponding trained points. Deviation values should typically be only a few degrees, which can allow you to manage minor fluctuations in bead width that can come from angular differences in certain target images. Larger deviation values can lead to invalid results and longer processing times. |

---

### `M_BOX_WIDTH_MARGIN`

Sets an additional width for the search box Aurora Imaging Library uses to verify the template's measured bead.  Aurora Imaging Library bases the search box's width on the trained box's width, which you can inquire with [`M_TRAINED_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md). Aurora Imaging Library adds the additional width ([`M_BOX_WIDTH_MARGIN`](../../Reference/bead/MbeadControl.md)) to the sides (margins) of the search box. Aurora Imaging Library applies the additional width as a percentage of the trained nominal width of the bead template, which you can inquire with [`M_TRAINED_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md). That is, `_SearchBoxWidth_ = _TrainedBoxWidth_ + (_TrainedNominalWidth_ x _AdditionalMarginPercentage_)`.  *[Image: BeadTrainingSearchBoxMargin.png]*  For example, if the width of the trained box is 100 pixels, the trained nominal width of the bead is 60 pixels, and the additional width for the search box is 50%, the search box width is 130 pixels (100 + (60 x 0.5). In this case, the search box is 30 pixels wider (15 pixels on each if its sides) than the trained box.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the additional width used for verification, as a percentage. |

---

### `M_FAIL_WARNING_OFFSET`

Sets an additional width to the search box in which a measured point can be found, but will have a failed status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)). When measured points fall outside the search box, but within this additional width, you can still retrieve results about them, such as their found position. If [`M_FAIL_WARNING_OFFSET`](../../Reference/bead/MbeadControl.md) is less than the search box width that Aurora Imaging Library uses for validating measured points ([`M_BOX_WIDTH_MARGIN`](../../Reference/bead/MbeadControl.md)), [`M_FAIL_WARNING_OFFSET`](../../Reference/bead/MbeadControl.md) has no effect.  *[Image: BeadTrainingSearchBoxFailWarning.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the width, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_GAP_MAX_LENGTH`

Sets the maximum allowable length of one single gap in the template's measured bead, for it to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)). A gap refers to a section of the measured bead that corresponds to a single trained point, or to a set of consecutive trained points, that were expected but not found during verification. If any gap is longer than [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md), the template's measured bead will not have a passing status, regardless of any other setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value >= 0.0` *(default)* | Specifies the maximum gap length, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). A value of 0.0 indicates that no gaps are allowed. |

---

### `M_GAP_TOLERANCE`

Sets the percentage of the total allowable gap (all gaps together) in the template's measured bead, for it to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)). A gap refers to a section of the measured bead that corresponds to a single trained point, or to a set of consecutive trained points, that were expected but not found during verification. If the total combined length of all gaps in the measured bead, relative to the full length of the corresponding bead template, is greater than [`M_GAP_TOLERANCE`](../../Reference/bead/MbeadControl.md), the template's measured bead will not have a passing status, regardless of any other setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the total allowable gaps, as a percentage. A value of 0.0% indicates that no gaps are allowed. |

---

### `M_INTENSITY_DELTA_NEG`

Sets the lowest pixel intensity that is considered valid for the measured bead, relative to the nominal intensity. That is, _LowestValidIntensity_ = _NominalIntensity_ - [`M_INTENSITY_DELTA_NEG`](../../Reference/bead/MbeadControl.md). The nominal intensity can either be established by the training phase ([`M_INTENSITY_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) set to [`M_AUTO`](../../Reference/bead/MbeadControl.md)) or with an explicit value ([`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the lowest valid pixel intensity. |

---

### `M_INTENSITY_DELTA_POS`

Sets the highest pixel intensity that is considered valid for the measured bead, relative to the nominal intensity. That is, _HighestValidIntensity_ = _NominalIntensity_ + [`M_INTENSITY_DELTA_POS`](../../Reference/bead/MbeadControl.md). The nominal intensity can either be established by the training phase ([`M_INTENSITY_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) set to [`M_AUTO`](../../Reference/bead/MbeadControl.md)) or with an explicit value ([`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the highest valid pixel intensity. |

---

### `M_INTENSITY_NOMINAL`

Sets the nominal pixel intensity of the template's measured bead.  [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md) only has an effect if you set [`M_INTENSITY_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) to [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md). To specify a range of acceptable pixel-intensities for the measured bead, use [`M_INTENSITY_DELTA_NEG`](../../Reference/bead/MbeadControl.md) and [`M_INTENSITY_DELTA_POS`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the nominal pixel intensity. |

---

### `M_OFFSET_MAX`

Sets the maximum allowable distance between the position of the trained points in the template, and the position of the corresponding measured points in the target, for the measured bead to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)). Measured points must always fall within the search box (for example, [`M_BOX_WIDTH_MARGIN`](../../Reference/bead/MbeadControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_OFFSET_MAX`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value > 0.0` | Specifies the allowable offset, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_WIDTH_DELTA_NEG`

Sets the valid delta negative tolerance between the width of the measured bead and the nominal width of the corresponding template, for the measured bead to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value > 0.0` | Specifies the tolerance, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

---

### `M_WIDTH_DELTA_POS`

Sets the valid delta positive tolerance between the width of the measured bead and the nominal width of the corresponding template, for the measured bead to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadControl.md) has no effect. |
| `Value > 0.0` | Specifies the tolerance, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

### To control how Aurora Imaging Library extracts beads from an image for the training and verification phases

The following [`ControlType`](../../Reference/bead/MbeadControl.md) and corresponding [`ControlValue`](../../Reference/bead/MbeadControl.md) parameter settings are used to control how Aurora Imaging Library performs the edge extraction of the bead from the training ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)) and target ([`MbeadVerify`](../../Reference/bead/MbeadVerify.md)) image. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadControl.md) parameter to a specific template (**M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**) or to all templates ([`M_ALL`](../../Reference/bead/MbeadControl.md)).  Aurora Imaging Library extracts beads from images using processes based on the Aurora Imaging Library Measurement module. Such processes use a one-dimensional analysis of differences in pixel intensities. For more information about this type of edge extraction, see [Search algorithm](../../UserGuide/C19_Measurements/S05_Search_Algorithm.md).

---

### `M_FOREGROUND_VALUE`

Sets whether the foreground that Aurora Imaging Library uses when performing the edge extraction of the bead from an image should be darker or lighter than the background. Only beads with the specified foreground will be trained or verified, depending on which function you are calling.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` | Specifies that the foreground is darker than the background. For example, a black bead on a white surface. |
| `M_FOREGROUND_WHITE` *(default)* | Specifies that the foreground is lighter than the background. For example, a white bead on a black surface. |

---

### `M_SMOOTHNESS`

Sets the degree of noise reduction that Aurora Imaging Library uses when performing the edge extraction of the bead from an image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value.  A setting of 0.0 indicates almost no noise reduction effect, while a setting of 100.0 indicates a very strong noise reduction effect. |

---

### `M_THRESHOLD_MODE`

Sets how to establish the threshold that Aurora Imaging Library must use when performing the edge extraction of the bead from an image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED` *(default)* | Specifies that the threshold is explicitly defined with [`M_THRESHOLD_VALUE`](../../Reference/bead/MbeadControl.md). |

---

### `M_THRESHOLD_VALUE`

Sets the value beneath which a grayscale variation within the image is not considered an edge of the bead. This value only applies if [`M_THRESHOLD_MODE`](../../Reference/bead/MbeadControl.md) is set to [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the threshold value. To consider all grayscale variations as possible edges of the bead, irrespective of their grayscale value, set [`M_THRESHOLD_VALUE`](../../Reference/bead/MbeadControl.md) to 0.0. However, in this case, due to the increased number of edges, processing time might increase, and false positives might occur. |

### To associate a camera calibration context with a training image

The following [`ControlType`](../../Reference/bead/MbeadControl.md) and corresponding [`ControlValue`](../../Reference/bead/MbeadControl.md) parameter settings are used to associate a camera calibration context with the training image. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadControl.md) parameter to [`M_CONTEXT`](../../Reference/bead/MbeadControl.md).

---

### `M_ASSOCIATED_CALIBRATION`

Associates the specified camera calibration context with the training image specified in [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), if required. Aurora Imaging Library requires a calibrated training image if your training settings are specified in world units ([`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) and/or [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) set to [`M_WORLD`](../../Reference/bead/MbeadControl.md)). If you associate a camera calibration, and you pass a calibrated training image to [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), Aurora Imaging Library ignores the associated camera calibration ([`M_ASSOCIATED_CALIBRATION`](../../Reference/bead/MbeadControl.md)), and uses the image's camera calibration instead.  This value is for the training phase ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). If you modify this value, you must retrain the context before the next verification.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Removes the association between the training image and the camera calibration context. |
| `Calibration context identifier` | Specifies the camera calibration context to associate with the training image. |

### To control operations with

The following [`ControlType`](../../Reference/bead/MbeadControl.md) and corresponding [`ControlValue`](../../Reference/bead/MbeadControl.md) parameter settings are used to control operations with [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md). In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadControl.md) parameter to [`M_CONTEXT`](../../Reference/bead/MbeadControl.md).

---

### `M_CLOSEST_POINT_MAX_DISTANCE`

Sets the maximum distance for the [`M_CLOSEST_...`](../../Reference/bead/MbeadGetNeighbors.md) operations in [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the maximum distance is infinite. |
| `Value > 0.0` | Specifies the maximum distance, relative to the input coordinate system specified using [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). |

You can only use this value with stripe-beads.
