---
doctype: Reference
module: class
function: MclassSplitDataset
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassSplitDataset"
---

# MclassSplitDataset

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

> Split the source dataset context into two destination dataset contexts.

## Syntax

```c
void MclassSplitDataset(
    AIL_ID     SplitContextClassId,             //in
    AIL_ID     SrcDatasetContextClassId,        //in
    AIL_ID     DstFirstDatasetContextClassId,   //out
    AIL_ID     DstSecondDatasetContextClassId,  //out
    AIL_DOUBLE Percentage,                      //in
    AIL_ID     SplitResultClassId,              //in
    AIL_INT64  ControlFlag                      //in
)
```

## Description

This function splits the source dataset context into two destination dataset contexts, according to a percentage. This is typically done to split all of your data ([`SrcDatasetContextClassId`](../../Reference/class/MclassSplitDataset.md)) into the training dataset context ([`DstFirstDatasetContextClassId`](../../Reference/class/MclassSplitDataset.md)) and the development dataset context ([`DstSecondDatasetContextClassId`](../../Reference/class/MclassSplitDataset.md)), which you will use with [`MclassTrain`](../../Reference/class/MclassTrain.md) to train your classifier context. This function ensures that the proper portion of entries for each class in the source dataset context are in both the first and second destination dataset contexts.

You can consider the split to be a type of copy of dataset entries from the source dataset to either the first or second destination dataset. All source dataset entries are copied (including class definitions) and unaltered. Entries already in the destination are overwritten.

All datasets (source and destinations) must be for the same classification task; that is, you must have allocated all of them with either [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) (for image classification, object detection, segmentation, or anomaly detection) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md) (for feature classification).

To establish the first and second destination dataset contexts, Aurora Imaging Library randomly selects source entries for the split. Typically, a different random selection of entries is chosen each time you call this function. To select the same random entries, use [`M_SPLIT_..._FIXED_SEED`](../../Reference/class/MclassSplitDataset.md).

If the source dataset context contains augmented entries, they can end up in both the first and second destination dataset contexts. In this case, you must not use one of these destinations as your development dataset context or testing dataset context, since only the training dataset context can contain augmented entries. To inquire whether the source dataset contexts have augmented entries, use [`M_NUMBER_OF_AUGMENTED_ENTRIES`](../../Reference/class/MclassInquire.md).

An augmented entry is never split from the source entry with which it was augmented. This can cause the percentage of entries in the first and second destination dataset contexts to differ from the specified percentage. If you do not have augmented data, source entries are only copied to one destination.

## Parameters

### `SplitContextClassId` *(in, AIL_ID)*

Specifies the predefined split classification context. This predefined context establishes whether to use a different random selection of entries to split the source dataset context, or to use the random selection of entries associated to a fixed seed.

*For specifying the predefined split classification context*

| Value | Description |
| --- | --- |
| `M_SPLIT_ANO_CONTEXT_DEFAULT` | Specifies to use a different random selection of entries from the source dataset context to establish the first and second destination dataset contexts, for anomaly detection. |
| `M_SPLIT_ANO_CONTEXT_FIXED_SEED` | Specifies to use the random selection of entries that is associated to a fixed seed, for anomaly detection. In this case, the first and second destination dataset contexts are established using the same random selection of entries from the source dataset context. This allows you to repeat the same split operation, provided you make an identical call to [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) (that is, you specify the same dataset contexts and split percentage). |
| `M_SPLIT_CONTEXT_DEFAULT` | Specifies to use a different random selection of entries from the source dataset context to establish the first and second destination dataset contexts, for image classification (CNN) or feature classification (tree ensemble). |
| `M_SPLIT_CONTEXT_FIXED_SEED` | Specifies to use the random selection of entries that are associated to a fixed seed, for image classification (CNN) or feature classification (tree ensemble). In this case, the first and second destination dataset contexts are established using the same random selection of entries from the source dataset context. This allows you to repeat the same split operation, provided you make an identical call to [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) (that is, you specify the same dataset contexts and split percentage). |
| `M_SPLIT_DET_CONTEXT_DEFAULT` | Specifies to use a different random selection of entries from the source dataset context to establish the first and second destination dataset contexts, for object detection. |
| `M_SPLIT_DET_CONTEXT_FIXED_SEED` | Specifies to use the random selection of entries that is associated to a fixed seed, for object detection. In this case, the first and second destination dataset contexts are established using the same random selection of entries from the source dataset context. This allows you to repeat the same split operation, provided you make an identical call to [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) (that is, you specify the same dataset contexts and split percentage). |
| `M_SPLIT_SEG_CONTEXT_DEFAULT` | Specifies to use a different random selection of entries from the source dataset context to establish the first and second destination dataset contexts, for segmentation. |
| `M_SPLIT_SEG_CONTEXT_FIXED_SEED` | Specifies to use the random selection of entries that is associated to a fixed seed, for segmentation. In this case, the first and second destination dataset contexts are established using the same random selection of entries from the source dataset context. This allows you to repeat the same split operation, provided you make an identical call to [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) (that is, you specify the same dataset contexts and split percentage). |

### `SrcDatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the source dataset context.

### `DstFirstDatasetContextClassId` *(out, AIL_ID)*

Specifies the identifier of the first destination dataset context.

### `DstSecondDatasetContextClassId` *(out, AIL_ID)*

Specifies the identifier of the second destination dataset context.

### `Percentage` *(in, AIL_DOUBLE)*

Specifies the percentage of entries in the source dataset context that should be in the first destination dataset context. The remaining entries in the source dataset context will be in the second destination dataset context. The percentage value can range between 0.0 to 100.0, inclusive.

### `SplitResultClassId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
