---
doctype: Reference
module: class
function: MclassStatCalculate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassStatCalculate"
---

# MclassStatCalculate

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

> Calculates statistics on the datasets.

## Syntax

```c
void MclassStatCalculate(
    AIL_ID    StatContextClassId,     //in
    AIL_ID    DatasetContextClassId,  //in
    AIL_INT64 EntryIndex,             //in
    AIL_UUID  EntryKey,               //in
    AIL_ID    StatResultClassId,      //out
    AIL_INT64 ControlFlag             //in
)
```

## Description

This function calculates statistics on the classification datasets. This requires specifying a statistics classification context, which you must have allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md), [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md), [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), or [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). To control a statistics classification context, use [`MclassControl`](../../Reference/class/MclassControl.md).

You can calculate statistics for specific dataset entries, using the [`EntryIndex`](../../Reference/class/MclassStatCalculate.md) or [`EntryKey`](../../Reference/class/MclassStatCalculate.md) parameters, if you are specifying [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), or pixel-level [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md).

After calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md), you can obtain the statistics from the result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md).

For datasets with more than 256 class definitions, using default colors will result in repeated colors.

Before calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md), the statistics classification context must be in a preprocessed state ([`MclassPreprocess`](../../Reference/class/MclassPreprocess.md)). To inquire this, call [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_PREPROCESSED`](../../Reference/class/MclassInquire.md).

## Parameters

### `StatContextClassId` *(in, AIL_ID)*

Specifies the identifier of a statistics classification context. This context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md), [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md), [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), or [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md).

### `DatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the images or features dataset on which to calculate statistics. These contexts are allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). Note that [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md), [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md), [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md) require an images dataset, and [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md) requires a feature dataset.

### `EntryIndex` *(in, AIL_INT64)*

Specifies the index of the dataset entry for which to calculate statistic results. This is only possible when using [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), and when calculating pixel level statistics for [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md).

*For specifying the index of an entry*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the index of an entry is not required. |
| `Value >= 0` | Specifies the index of an entry. |

### `EntryKey` *(in, AIL_UUID)*

Specifies the key (_AIL_UUID_) of the entry for which to calculate statistic results. This is only possible when using [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), and when calculating pixel level statistics for [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md).

*For specifying the unique key of an entry*

| Value | Description |
| --- | --- |
| `M_DEFAULT_KEY` | Specifies that the unique key of an entry is not required. |
| `Value` | Specifies the unique key of an entry. |

### `StatResultClassId` *(out, AIL_ID)*

Specifies the identifier of the statistics classification result buffer in which to write the results of the statistics calculation. The statistics classification result buffer must have been previously allocated on the required system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
