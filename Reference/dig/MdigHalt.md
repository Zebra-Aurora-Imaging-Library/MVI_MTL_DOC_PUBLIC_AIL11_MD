---
doctype: Reference
module: dig
function: MdigHalt
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dig / MdigHalt"
---

# MdigHalt

| Board | Supported |
| --- | --- |
| Host System | No |
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

> Halt a continuous grab from an input device.

## Syntax

```c
void MdigHalt(
    AIL_ID DigId  //in
)
```

## Description

This function stops the specified digitizer from grabbing data. It should be used when performing a continuous grab with [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md).

This function will wait for the end of the current frame before returning, to ensure the last frame is always valid.

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer.
