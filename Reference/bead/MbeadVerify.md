---
doctype: Reference
module: bead
function: MbeadVerify
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadVerify"
---

# MbeadVerify

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

> Verify beads in the target image according to their corresponding template.

## Syntax

```c
void MbeadVerify(
    AIL_ID    ContextBeadId,     //in
    AIL_ID    TargetImageBufId,  //in
    AIL_ID    ResultBeadId,      //out
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function verifies that beads in the target image conform to their corresponding templates in the context. Results of the measured bead are written in the provided result buffer and can be read using [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md). To retrieve whether a measured bead has passed or failed the specifications of its template, use the [`M_STATUS`](../../Reference/bead/MbeadGetResult.md) result. To draw results, use [`MbeadDraw`](../../Reference/bead/MbeadDraw.md).

Before your first call to [`MbeadVerify`](../../Reference/bead/MbeadVerify.md), you must train the context using [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). Certain modifications to the bead context, such as adding, deleting, or modifying templates with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md), or changing some control settings with [`MbeadControl`](../../Reference/bead/MbeadControl.md), require you to retrain the context before any subsequent call to [`MbeadVerify`](../../Reference/bead/MbeadVerify.md). To inquire the training status of the templates in the context, use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md).

If you have associated the target image with a camera calibration context, you can specify that certain input settings be interpreted in world units. To do so, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) set to [`M_WORLD`](../../Reference/bead/MbeadControl.md). Note that if you set this constant to [`M_WORLD`](../../Reference/bead/MbeadControl.md) but you don't pass [`MbeadVerify`](../../Reference/bead/MbeadVerify.md) a calibrated target image, the function will generate an error.

If you are specifying pixel units, make sure the pixel sizes are consistent across all camera calibrations (for example, pixel sizes should be the same in the training and verification images). When using different camera calibration contexts, it is recommended that you use world units.

## Parameters

### `ContextBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context that contains the templates with which to validate the beads in the target image. The bead context must have been previously allocated on the required system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md).

### `TargetImageBufId` *(in, AIL_ID)*

Specifies the identifier of the target image buffer that contains the beads that must be verified, according the templates contained within the bead context. The image buffer must be a 1-band 8-bit unsigned buffer.

### `ResultBeadId` *(out, AIL_ID)*

Specifies the identifier of the bead result buffer in which to write the results of the verification. The bead result buffer must have been previously allocated on the required system using [`MbeadAllocResult`](../../Reference/bead/MbeadAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
