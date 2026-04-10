---
doctype: Reference
module: class
function: MclassAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassAllocResult"
---

# MclassAllocResult

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

> Allocate a classification result buffer to hold training, prediction, or statistic results.

## Syntax

```c
AIL_ID MclassAllocResult(
    AIL_ID    SysId,            //in
    AIL_INT64 ResultType,       //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  ResultClassIdPtr  //out
)
```

## Description

This function allocates a classification result buffer, on the specified system, to store the results from an [`MclassTrain`](../../Reference/class/MclassTrain.md), [`MclassPredict`](../../Reference/class/MclassPredict.md), or [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) operation.

When the classification result buffer is no longer required, release it using[`MclassFree`](../../Reference/class/MclassFree.md)unless [`M_UNIQUE_ID`](../../Reference/class/MclassAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/class/MclassAllocResult.md) was specified, the smart identifier manages the classification result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the result buffer. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of classification result buffer to allocate. Set this parameter to one of the following values:

*For specifying the type of classification result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_PREDICT_ANO_RESULT` | Specifies an anomaly detection prediction result buffer. This buffer holds the results produced from calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a predefined anomaly detection classifier context ([`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md)). |
| `M_PREDICT_CNN_RESULT` | Specifies a CNN prediction result buffer. This buffer holds the results produced from calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a predefined CNN classifier context ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md)). |
| `M_PREDICT_DET_RESULT` | Specifies an object detection prediction result buffer. This buffer holds the results produced from calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a predefined object detection classifier context ([`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md)). |
| `M_PREDICT_ONNX_RESULT` | Specifies an ONNX prediction result buffer. This buffer holds the results produced from calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with an ONNX classifier context ([`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md)). |
| `M_PREDICT_SEG_RESULT` | Specifies a segmentation prediction result buffer. This buffer holds the results produced from calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a predefined segmentation classifier context ([`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md)). |
| `M_PREDICT_TREE_ENSEMBLE_RESULT` | Specifies a tree ensemble prediction result buffer. This buffer holds the results produced from calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a tree ensemble classifier context ([`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)). |
| `M_STAT_ANO_RESULT` | Specifies a statistics classification result buffer for anomaly detection. This buffer holds the results produced from calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) with a statistics classification context for anomaly detection ([`M_STAT_ANO`](../../Reference/class/MclassAlloc.md)). |
| `M_STAT_CNN_RESULT` | Specifies a statistics classification result buffer for image classification. This buffer holds the results produced from calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) with a statistics classification context for image classification ([`M_STAT_CNN`](../../Reference/class/MclassAlloc.md)). |
| `M_STAT_DET_RESULT` | Specifies a statistics classification result buffer for object detection. This buffer holds the results produced from calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) with a statistics classification context for object detection ([`M_STAT_DET`](../../Reference/class/MclassAlloc.md)). |
| `M_STAT_SEG_RESULT` | Specifies a statistics classification result buffer for segmentation. This buffer holds the results produced from calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) with a statistics classification context for segmentation ([`M_STAT_SEG`](../../Reference/class/MclassAlloc.md)). |
| `M_STAT_TREE_ENSEMBLE_RESULT` | Specifies a statistics classification result buffer for feature classification. This buffer holds the results produced from calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) with a statistics classification context for feature classification ([`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)). |
| `M_TRAIN_ANO_RESULT` | Specifies an anomaly training result buffer. This buffer holds the results produced from calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with an anomaly detection training context ([`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md)). |
| `M_TRAIN_CNN_RESULT` | Specifies a CNN training result buffer. This buffer holds the results produced from calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with a CNN training context ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)). |
| `M_TRAIN_DET_RESULT` | Specifies an object detection training result buffer. This buffer holds the results produced from calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with an object detection training context ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)). |
| `M_TRAIN_SEG_RESULT` | Specifies a segmentation training result buffer. This buffer holds the results produced from calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with a segmentation training context ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)). |
| `M_TRAIN_TREE_ENSEMBLE_RESULT` | Specifies a tree ensemble training result buffer. This buffer holds the results produced from calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with a tree ensemble training context ([`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ResultClassIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the classification result buffer identifier or specifies the data type that the function should use to return the result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated classification result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated classification result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CLASS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the classification result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the prediction result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated prediction result buffer.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAllocResult.md) is written as the identifier. |
| `Address in which to write the statistics classification result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated statistics classification result buffer.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAllocResult.md) is written as the identifier. |
| `Address in which to write the training result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated training result buffer.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the classifier result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CLASS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/class/MclassAllocResult.md) was specified).
