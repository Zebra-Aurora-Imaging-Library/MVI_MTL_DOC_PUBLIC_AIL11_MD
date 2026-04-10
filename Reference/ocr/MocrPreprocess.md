---
doctype: Reference
module: ocr
function: MocrPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrPreprocess"
---

# MocrPreprocess

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

> Preprocess an OCR font context.

## Syntax

```c
void MocrPreprocess(
    AIL_ID    FontContextOcrId,  //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function preprocesses the specified OCR font context. This function must be called before any read or verify operation and after all the control and constraints are set to speed up all subsequent read or verify operations. Note that preprocessing will be done automatically during the first read or verify operation if this function is not explicitly called.

Each time a font-specific control or target constraint is changed the OCR font context should be preprocessed. To learn if the OCR font context requires preprocessing, call [`MocrInquire`](../../Reference/ocr/MocrInquire.md) with [`M_PREPROCESSED`](../../Reference/ocr/MocrInquire.md).

## Parameters

### `FontContextOcrId` *(in, AIL_ID)*

Specifies the OCR font context identifier.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the type of operation to perform. This parameter must be set to the following value.

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Preprocesses the OCR font context. |
