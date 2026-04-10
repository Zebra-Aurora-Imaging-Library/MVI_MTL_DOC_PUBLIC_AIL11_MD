---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Bead
section: Verifying_beads
module_tag: bead
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / bead / Verifying beads"
---

# Verifying beads and retrieving results

Once you have successfully trained the bead templates, you can verify them against their corresponding measured beads in a target image, using [`MbeadVerify`](../../Reference/bead/MbeadVerify.md). By default, to pass verification, each trained point in a template must have a corresponding measured point that was successfully found in its measured bead.

*[Image: BeadVerificationIntroExample.png]*

After calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md), the main result you will typically be interested in is [`M_STATUS`](../../Reference/bead/MbeadGetResult.md), which returns whether Aurora Imaging Library considers the measured beads (one or all) or the measured points (one or all) to have passed or failed verification. You can also retrieve more specific status results, such as whether a bead's width at a specific point has passed or failed the maximum width setting ([`M_STATUS_WIDTH_MAX`](../../Reference/bead/MbeadGetResult.md)). Such results can help you ascertain why the bead's general status ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)) is failing. For more information, see [Status and other results](S05_Verifying_beads.md).

You can affect the verification process and the results it calculates by modifying the verification settings using [`MbeadControl`](../../Reference/bead/MbeadControl.md) before calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md). For example, you can use [`M_ACCEPTANCE`](../../Reference/bead/MbeadControl.md) to lower the percentage of trained points that must have a corresponding measured point (found point) in the target image for the bead to have a passing status.

Note that you can only modify foreground, smoothness, and threshold settings during the training phase; for more information, see the relevant subsections in [Defining training and modifying templates](S04_Defining_training_and_modifying_templates.md).

Aurora Imaging Library interprets certain verification input values related to position and dimension type settings (such as [`M_OFFSET_MAX`](../../Reference/bead/MbeadControl.md)) in either pixel or world units. To set the units, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). This essentially establishes the input coordinate system to use. If Aurora Imaging Library interprets input settings in world units and you don't use a calibrated target image, [`MbeadVerify`](../../Reference/bead/MbeadVerify.md) generates an error.

## Score and acceptance

The score refers to the percentage of trained points that have a corresponding measured point with a passing status ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)), relative to the total number of trained points in the template. For every measured point with a failing status (not found), Aurora Imaging Library lowers the bead's score proportionally. For example, if a bead template has 100 trained points and 69 are found to have a passing status, Aurora Imaging Library returns a score of 69% for the measured bead.

Many reasons can cause a point to have a failing status. It can, for example, fall outside the boundaries of the search box, or its corresponding bead width can be lower than the specified value.

To specify the minimum score you're willing to accept for the template's corresponding measured bead to have a passing status, use the [`M_ACCEPTANCE`](../../Reference/bead/MbeadControl.md) control. To retrieve whether the measured bead's score has itself passed (is greater than or equal to) or failed (is less than) the acceptance set, use [`M_STATUS_SCORE`](../../Reference/bead/MbeadGetResult.md). If the requirements of [`M_ACCEPTANCE`](../../Reference/bead/MbeadControl.md) are not met or surpassed by the score of the measured bead, its main status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)) will fail, regardless of any other setting.

To get the score of the template's corresponding measured bead, as well as the number of trained points that have a corresponding measured point, call [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_SCORE`](../../Reference/bead/MbeadGetResult.md) and [`M_NUMBER_FOUND`](../../Reference/bead/MbeadGetResult.md), respectively.

## Gaps

A gap refers to a section of the measured bead that corresponds to a single trained point, or to a set of consecutive trained points, that were expected but not found during verification. By default, a measured bead is expected to be gap free.

*[Image: BeadGapIntroExample.png]*

To manage the extent to which you are willing to accept gaps during verification, you can use [`MbeadControl`](../../Reference/bead/MbeadControl.md) to set the:

- Maximum allowable length of any one gap in the measured bead ([`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md)). For a measured bead to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)), it cannot have a gap that is longer than [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md), regardless of any other setting. To retrieve the actual length of the longest gap result, use [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadGetResult.md).
- Total allowable gap (all gaps together) in the measured bead ([`M_GAP_TOLERANCE`](../../Reference/bead/MbeadControl.md)). For a measured bead to have a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)), the total combined length of all gaps in the measured bead, relative to the full length of the expected measured bead, must be less than [`M_GAP_TOLERANCE`](../../Reference/bead/MbeadControl.md), regardless of any other setting. To retrieve the actual percentage of the total length of all gaps in the measured bead result, use [`M_GAP_COVERAGE`](../../Reference/bead/MbeadGetResult.md).

The following example illustrates two beads, each 50 units long. [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md) is set to 10 units and [`M_GAP_TOLERANCE`](../../Reference/bead/MbeadControl.md) is set to 50%. The first bead fails because it has a gap with a length that is greater than 10 units. The second bead passes because there is no gap with a length greater than 10 units, and the total length of all gaps is less than 50% of the bead's length.

*[Image: BeadGapLengthTolerance.png]*

To retrieve whether the gaps in the measured bead have themselves passed (are less than or equal to) or failed (are greater than) the gap constraints, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_STATUS_GAP_MAX`](../../Reference/bead/MbeadGetResult.md) or [`M_STATUS_GAP_TOLERANCE`](../../Reference/bead/MbeadGetResult.md).

## Search box

The search box is the area along the bead in the target image in which to find each trained point's corresponding measured point, when you call [`MbeadVerify`](../../Reference/bead/MbeadVerify.md). To retrieve whether Aurora Imaging Library has found a trained point's associated measured point, use [`M_STATUS_FOUND`](../../Reference/bead/MbeadGetResult.md). You can also use this result to retrieve whether the entire measured bead was found. For more information, see [Status and other results](S05_Verifying_beads.md).

Aurora Imaging Library bases the search box's height, width, and spacing on the trained box established during training ([`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_TRAINED_BOX_HEIGHT`](../../Reference/bead/MbeadInquire.md), [`M_TRAINED_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md), and [`M_TRAINED_BOX_SPACING`](../../Reference/bead/MbeadInquire.md)). For verification, you can increase the width of the search box by adding margins to the width of the trained box, using [`M_BOX_WIDTH_MARGIN`](../../Reference/bead/MbeadControl.md).

*[Image: BeadTrainingSearchBoxMarginOnlyForVerify.png]*

You must specify the margin as a percentage of the trained nominal width of the bead template, which you can inquire with [`M_TRAINED_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md). Aurora Imaging Library calculates the width of the search box as: `_SearchBoxWidthForVerification_ = [`M_TRAINED_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md) + ([`M_TRAINED_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md) x [`M_BOX_WIDTH_MARGIN`](../../Reference/bead/MbeadControl.md))`.

For example, if the trained box width ([`M_TRAINED_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md)) is 100 pixels, the trained nominal width ([`M_TRAINED_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md)) is 60 pixels, and the specified margin ([`M_BOX_WIDTH_MARGIN`](../../Reference/bead/MbeadControl.md)) for the search box of the bead template is 50%, the width of the search box is 130 pixels (100 + (60 x 0.5)). The search box width is therefore 30 pixels (15 pixels on each side) wider than the width of the trained box. By default, the additional margin that Aurora Imaging Library adds is half the trained nominal width of the bead template.

For verification, the height and spacing of the search box is always the same as the height and spacing established for the trained box ([`M_TRAINED_BOX_HEIGHT`](../../Reference/bead/MbeadInquire.md) and [`M_TRAINED_BOX_SPACING`](../../Reference/bead/MbeadInquire.md)). For more information, see [Defining, training, and modifying bead templates](S04_Defining_training_and_modifying_templates.md).

### Fail warning

Only trained points with a corresponding measured point that falls within its search box can be given a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)). In some cases, you might want the measured point to fail verification, but you still want Aurora Imaging Library to find it, if it is within a certain distance from the limits of its search box's margin. This can mark the difference between measured points that are truly not there (for example, missing glue along the path), versus measured points that are misplaced (for example, an unexpected bulge of glue protruding along the typical glue path). To specify an additional width for the search box in which a measured point can be found but have a failed status, use [`M_FAIL_WARNING_OFFSET`](../../Reference/bead/MbeadControl.md).

*[Image: BeadTrainingSearchBoxFailWarningOnlyForVerify.png]*

When a trained point's corresponding measured point falls inside its search box's fail-warning zone, you can still retrieve its results, such as its found position. These results are not possible for points that fall outside the fail-warning zone. If the width of the search box is greater than [`M_FAIL_WARNING_OFFSET`](../../Reference/bead/MbeadControl.md), this setting has no effect.

## Position and offset

Position refers to either the X- and Y-location of each trained point in the template (which you can retrieve using [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_TRAINED_POSITION_X`](../../Reference/bead/MbeadGetResult.md) and [`M_TRAINED_POSITION_Y`](../../Reference/bead/MbeadGetResult.md)) or the X- and Y-location of each corresponding measured point in the measured bead (which you can retrieve using [`M_POSITION_X`](../../Reference/bead/MbeadGetResult.md) and [`M_POSITION_Y`](../../Reference/bead/MbeadGetResult.md)). The offset refers to the distance between these two sets of corresponding positions. Positions are taken at the center of the bead width.

Using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_OFFSET_MAX`](../../Reference/bead/MbeadControl.md), you can set the maximum allowable distance between the position of the trained points in the template and the position of their corresponding measured points in the target, for your results to have a passing status ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)). By default, Aurora Imaging Library does not allow any offset.

*[Image: BeadVerificationOffset.png]*

To retrieve whether the measured bead adheres to the specified offset condition, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_STATUS_OFFSET`](../../Reference/bead/MbeadGetResult.md). Keep in mind that measured points must always fall within the search box, otherwise they are not found and the offset constraint does not apply. The maximum offset should therefore fall within the search box. Depending on your settings, a measured point can be found within the search box ([`M_STATUS_FOUND`](../../Reference/bead/MbeadGetResult.md)), but still fail the position offset condition ([`M_STATUS_OFFSET`](../../Reference/bead/MbeadGetResult.md)).

*[Image: BeadVerificationOffsetSimple.png]*

To retrieve the actual offset calculated between each trained point and its corresponding measured point, use [`M_OFFSET`](../../Reference/bead/MbeadGetResult.md). To retrieve the greatest of these offsets, use [`M_OFFSET_MAX`](../../Reference/bead/MbeadGetResult.md). To retrieve the index of the trained point that corresponds to the measured point that represents the greatest offset, use [`M_OFFSET_MAX_INDEX`](../../Reference/bead/MbeadGetResult.md).

To retrieve the coordinates of the points on the left and right edges of the bead, which are collinear with the measured point, you can use [`M_START_POS_X`](../../Reference/bead/MbeadGetResult.md)/ [`M_START_POS_Y`](../../Reference/bead/MbeadGetResult.md)and [`M_END_POS_X`](../../Reference/bead/MbeadGetResult.md)/ [`M_END_POS_Y`](../../Reference/bead/MbeadGetResult.md), respectively. Note that left and right are established from the bead's direction, which follows the sequence of measured points in increasing order. You can only retrieve this information for stripe-beads.

*[Image: BeadLeftandRightPointFromMeasuredPoint.png]*

## Width

Width refers to the bead's thickness, from side to side, along its path. To retrieve the width of the measured bead at each of its measured points, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_WIDTH_VALUE`](../../Reference/bead/MbeadGetResult.md). You can also retrieve the average width of the measured bead ([`M_WIDTH_AVERAGE`](../../Reference/bead/MbeadGetResult.md)), its maximum ([`M_WIDTH_MAX`](../../Reference/bead/MbeadGetResult.md)) and minimum ([`M_WIDTH_MIN`](../../Reference/bead/MbeadGetResult.md)) width, as well as the index of the trained point that corresponds to the measured point representing the maximum ([`M_WIDTH_MAX_INDEX`](../../Reference/bead/MbeadGetResult.md)) and minimum ([`M_WIDTH_MIN_INDEX`](../../Reference/bead/MbeadGetResult.md)) width.

By default, the width of the bead at each measured point must be the same as the width of the bead template at each corresponding trained point, to get a passing status result ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)), regardless of any other setting. For stripe-beads ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md)), you can use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadControl.md) and [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadControl.md) to set a valid delta negative and delta positive tolerance between the width of the measured bead and the nominal width of the template.

To retrieve whether width results adhere to (pass or fail) the specified width conditions, use [`M_STATUS_WIDTH_MAX`](../../Reference/bead/MbeadGetResult.md) and [`M_STATUS_WIDTH_MIN`](../../Reference/bead/MbeadGetResult.md).

## Angle

The angle of a bead refers to the angles represented by its points. For each point, an angle is established using a theoretical line that passes through that point, and is perpendicular to the tangent line that represents the bead within that point's training or search box.

*[Image: BeadVerificationAngle.png]*

Using the [`M_ANGLE_ACCURACY_MAX_DEVIATION`](../../Reference/bead/MbeadControl.md) control type, you can set a maximum angular difference between the angle of the bead template at each of its trained points, and the angle of the corresponding measured bead at each of its measured points. By default, there can be no difference.

If you do not specify an angular deviation, Aurora Imaging Library considers the width of the bead at each measured point to be along the angle established at its corresponding trained point. By specifying an angular deviation, Aurora Imaging Library can refine its width measurement accordingly.

*[Image: BeadVerificationAngleWithExampleWidth.png]*

Typically, you should use [`M_ANGLE_ACCURACY_MAX_DEVIATION`](../../Reference/bead/MbeadControl.md) to specify a small value of only a few degrees to manage minor deviations in bead width due to some shifts in angle that can occur in certain target images during the verification phase. Large deviation values can lead to invalid results and longer processing times. To retrieve the angle of the measured bead at the position of its measured points, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_ANGLE`](../../Reference/bead/MbeadGetResult.md).

## Intensity

Intensity can be seen as the brightness of the bead, which Aurora Imaging Library establishes according to the grayscale value of its pixels. Since your training ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)) and target ([`MbeadVerify`](../../Reference/bead/MbeadVerify.md)) images must be 8-bit, the highest possible pixel value of a bead is 255 (brightest intensity), while the lowest possible pixel value is 0 (darkest intensity). Aurora Imaging Library calculates the intensity for each point (measured point or trained point) at the center of its corresponding bead width. Intensity constraints and results only apply to stripe-beads ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md)).

To retrieve the intensity of the measured bead at each of its measured points, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_INTENSITY`](../../Reference/bead/MbeadGetResult.md). You can also retrieve the measured bead's maximum ([`M_INTENSITY_MAX`](../../Reference/bead/MbeadGetResult.md)) and minimum ([`M_INTENSITY_MIN`](../../Reference/bead/MbeadGetResult.md)) intensity, as well as the index of the trained point that corresponds to the measured point representing the maximum ([`M_INTENSITY_MAX_INDEX`](../../Reference/bead/MbeadGetResult.md)) and minimum ([`M_INTENSITY_MIN_INDEX`](../../Reference/bead/MbeadGetResult.md)) intensity.

Provided that Aurora Imaging Library is able to find the bead in the search box, there are no default restrictions for the intensity of the bead. You can however use [`MbeadControl`](../../Reference/bead/MbeadControl.md) to set a nominal pixel intensity for the measured bead, and an acceptable maximum and minimum range within which the intensity of the bead must fall.

To set intensity conditions, first use [`M_INTENSITY_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) to specify how Aurora Imaging Library should establish the nominal intensity with which to validate the template's corresponding measured bead.

- By setting the nominal intensity mode to [`M_AUTO`](../../Reference/bead/MbeadControl.md), Aurora Imaging Library will establish the nominal intensity during the training phase (from the training image).
- By setting the nominal intensity mode to [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md), you must explicitly set the nominal intensity using [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md).
- By setting the nominal intensity mode to [`M_DISABLE`](../../Reference/bead/MbeadControl.md) (same as [`M_DEFAULT`](../../Reference/bead/MbeadControl.md)), Aurora Imaging Library will ignore all intensity conditions.

Once you have established the nominal intensity, either from the training image ([`M_AUTO`](../../Reference/bead/MbeadControl.md)) or with an explicit value ([`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)), you can use the following verification settings to specify a valid range within which the intensity of the measured bead must fall:

- [`M_INTENSITY_DELTA_NEG`](../../Reference/bead/MbeadControl.md), which sets the lowest pixel intensity Aurora Imaging Library considers valid for the measured bead, relative to the nominal intensity. That is, _LowestValidIntensity_ = _NominalIntensity_ - [`M_INTENSITY_DELTA_NEG`](../../Reference/bead/MbeadControl.md).
- [`M_INTENSITY_DELTA_POS`](../../Reference/bead/MbeadControl.md), which sets the highest pixel intensity Aurora Imaging Library considers valid for the measured bead, relative to the nominal intensity. That is, _HighestValidIntensity_ = _NominalIntensity_ + [`M_INTENSITY_DELTA_POS`](../../Reference/bead/MbeadControl.md).

To retrieve whether the intensity results adhere to (pass or fail) the specified intensity conditions, use [`M_STATUS_INTENSITY_MAX`](../../Reference/bead/MbeadGetResult.md) and [`M_STATUS_INTENSITY_MIN`](../../Reference/bead/MbeadGetResult.md).

## Status and other results

As previously discussed, there are many different status results available for retrieval with [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md). Depending on the settings of this function's parameters, the same status can have different meanings. For example, the main status ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)) result can retrieve:

- Whether all measured beads corresponding to all bead templates in the bead context have passed or failed verification, when you set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) and [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameters to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). In this case, [`M_STATUS`](../../Reference/bead/MbeadGetResult.md) will pass only if all status results ([`M_STATUS_...`](../../Reference/bead/MbeadGetResult.md)) for all templates in the context return [`M_PASS`](../../Reference/bead/MbeadGetResult.md).
- Whether the measured bead corresponding to a specific template has passed or failed verification, when you set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the label or index of the template and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). In this case, [`M_STATUS`](../../Reference/bead/MbeadGetResult.md) will pass only if the template's corresponding measured bead has passed all of the following: [`M_STATUS_GAP_TOLERANCE`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_GAP_MAX`](../../Reference/bead/MbeadGetResult.md), and [`M_STATUS_SCORE`](../../Reference/bead/MbeadGetResult.md).
- Whether the measured points corresponding to the specified trained points of a bead template have passed or failed verification, when you set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the label or index of a template and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific value (a trained point) or [`M_ALL`](../../Reference/bead/MbeadGetResult.md) (all trained points). In this case, [`M_STATUS`](../../Reference/bead/MbeadGetResult.md) will pass only if each trained point's corresponding measured point has passed all of the following: [`M_STATUS_SEARCH`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_FOUND`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_OFFSET`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_WIDTH_MAX`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_WIDTH_MIN`](../../Reference/bead/MbeadGetResult.md), and [`M_STATUS_GAP_MAX`](../../Reference/bead/MbeadGetResult.md).

Some of the specific [`M_STATUS_...`](../../Reference/bead/MbeadGetResult.md) results are available for either the template's corresponding measured bead or the trained points' corresponding measured points. For example, if you are retrieving the [`M_STATUS_FOUND`](../../Reference/bead/MbeadGetResult.md) result, and you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), Aurora Imaging Library returns [`M_PASS`](../../Reference/bead/MbeadGetResult.md) if all trained points have a corresponding measured point (all were found). Otherwise, Aurora Imaging Library returns [`M_FAIL`](../../Reference/bead/MbeadGetResult.md). However, if you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), Aurora Imaging Library returns [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) for each trained point, depending on whether its corresponding measured point was found. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, Aurora Imaging Library returns only the found status of its measured point.

Although status results are typically the most crucial, you can also retrieve other results related to Aurora Imaging Library's analysis of the measured bead or measured point. For example, you can retrieve the position of the training point's associated measured points ([`M_POSITION_X`](../../Reference/bead/MbeadGetResult.md) and [`M_POSITION_Y`](../../Reference/bead/MbeadGetResult.md)), the corresponding width of the measured bead at those points ([`M_WIDTH_VALUE`](../../Reference/bead/MbeadGetResult.md)), and whether the path of the measured bead ends where it began ([`M_CLOSURE`](../../Reference/bead/MbeadGetResult.md)).

### When you have a failing status

To help decipher why you have a failing status ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)), you can try:

- Pin-pointing the root cause of the failure. For example, if the failing status is based on all the bead templates in the entire context, you should get the status of each individual template, and for the ones that are failing, you should get the status of each individual point.
  Keep in mind all the conditions upon which the status is based, and how failing just one causes [`M_STATUS`](../../Reference/bead/MbeadGetResult.md) to fail. For example, if you are retrieving whether a specific template has passed or failed verification, and that template's corresponding measured bead has failed the gap tolerance constraint ([`M_STATUS_GAP_TOLERANCE`](../../Reference/bead/MbeadGetResult.md)), [`M_STATUS`](../../Reference/bead/MbeadGetResult.md) will fail, even if there are enough measured points to pass the minimum score constraint ([`M_STATUS_SCORE`](../../Reference/bead/MbeadGetResult.md)).
- Once you have isolated the failing points, determine the cause of their failure by getting more specific status results about them ([`M_STATUS_...`](../../Reference/bead/MbeadGetResult.md)). For example, you can use [`M_STATUS_SEARCH`](../../Reference/bead/MbeadGetResult.md) to determine whether Aurora Imaging Library considers it possible to establish the measured point, given the current settings. Note that, if the search box corresponding to a measured point falls outside the target image, [`M_STATUS_SEARCH`](../../Reference/bead/MbeadGetResult.md) returns [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) since you can never establish a measured point that is beyond the boundaries of the image.
- Performing drawing operations to get a visual representation of the measured bead and its measured points. For example, you can draw a cross at the position of all the measured points that were expected but not found. For more information, see [Drawing](S06_Drawing.md).

Since verification uses trained templates, settings for the training phase have an indirect affect on the verification phase. If you are unable to successfully verify beads in a target image, and you are unwilling to accept missing measured points (for example, by lowering [`M_ACCEPTANCE`](../../Reference/bead/MbeadControl.md) or allowing for gaps), you might have to modify the template and re-call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) before calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md) again.
