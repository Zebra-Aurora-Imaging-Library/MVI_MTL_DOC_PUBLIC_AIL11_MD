---
doctype: Reference
module: bead
function: MbeadTrain
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadTrain"
---

# MbeadTrain

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

> Train the templates in a bead context.

## Syntax

```c
void MbeadTrain(
    AIL_ID    ContextBeadId,       //in
    AIL_ID    TrainingImageBufId,  //in
    AIL_INT64 ControlFlag          //in
)
```

## Description

This function trains the templates for a verification operation and sets internal processing settings to optimize future calculations for speed and robustness. This function can also internally save or free a training image ([`TrainingImageBufId`](../../Reference/bead/MbeadTrain.md)).

You must call this function before your first call to [`MbeadVerify`](../../Reference/bead/MbeadVerify.md). Certain modifications to a bead context, such as adding, deleting, or modifying templates with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md), or changing some control settings with [`MbeadControl`](../../Reference/bead/MbeadControl.md), require you to retrain the templates before any subsequent call to [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

To inquire the training status of the templates in the context, use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md). Typically you should not call [`MbeadVerify`](../../Reference/bead/MbeadVerify.md) until all the templates in the context have been completely trained. You might have to modify your settings and retrain the templates more than once before you can perform a successful verification. To draw the templates in the context (trained or untrained), use [`MbeadDraw`](../../Reference/bead/MbeadDraw.md).

To train the templates, Aurora Imaging Library must establish their trained points, which are based on the most current settings, such as the spacing between the expected position of the trained points, the template's path (polyline, circle, or segment), and width. To train the templates, Aurora Imaging Library also uses the training image, if it is required by the template's settings.

By default, you must use a training image, since the default path a template follows is a polyline that is refined by the training image ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) set to [`M_DEFAULT`](../../Reference/bead/MbeadControl.md) or [`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)). If a template follows a path that is a fixed polyline, circle, or segment ([`M_POLYLINE`](../../Reference/bead/MbeadControl.md), [`M_CIRCLE`](../../Reference/bead/MbeadControl.md), or [`M_SEGMENT`](../../Reference/bead/MbeadControl.md)), the path's position will not be refined, even if you specify a training image. Similarly, Aurora Imaging Library uses the training image by default to establish the width of the template's path ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) set to [`M_DEFAULT`](../../Reference/bead/MbeadControl.md) or [`M_AUTO_...`](../../Reference/bead/MbeadControl.md)). If the nominal width is explicitly set, a training image is not required and is ignored if provided.

If you have associated the training image with a camera calibration context, you can specify that certain input settings for the training phase be interpreted in world units. To do so, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) and/or [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) set to [`M_WORLD`](../../Reference/bead/MbeadControl.md). To use a calibrated training image, you can either pass one to this function, or you can specify the camera calibration context to associate to the training image, using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/bead/MbeadControl.md). If both camera calibrations are available, Aurora Imaging Library ignores the camera calibration set with [`M_ASSOCIATED_CALIBRATION`](../../Reference/bead/MbeadControl.md). Note that if Aurora Imaging Library is interpreting input settings for the training phase in world units, but you don't use a calibrated training image, [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) will generate an error.

If you are specifying pixel units, make sure the pixel sizes are consistent across all camera calibrations (for example, pixel sizes should be the same in the training and verification images). When using different camera calibration contexts, it is recommended that you use world units.

## Parameters

### `ContextBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context that contains the templates to train. The bead context must have been previously allocated on the required system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md).

### `TrainingImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the training image with which to train the templates, or specifies to train the templates without a training image (or with a previously saved training image).

*For specifying the source*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies no training image will be used to train the templates, unless the context contains a training image from previous training. In this case, Aurora Imaging Library uses the saved training image to train the templates even if you specify [`M_NULL`](../../Reference/bead/MbeadTrain.md). |
| `Image buffer identifier` | Specifies the identifier of an image buffer containing the training image Aurora Imaging Library will use to train the templates. The image buffer must be a 1-band 8-bit unsigned buffer. All templates in the context use the same training image.

Note that the training image will be internally saved. If the context contains a training image from previous training, it will be freed from memory and the newly specified image will be used to train the templates.

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

To train the templates, Aurora Imaging Library extracts beads from the training image using processes based on the Aurora Imaging Library Measurement module. Such processes use a one-dimensional analysis of differences in pixel intensities. For more information about this type of edge extraction, see [Search algorithm](../../UserGuide/C19_Measurements/S05_Search_Algorithm.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
