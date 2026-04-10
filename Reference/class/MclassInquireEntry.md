---
doctype: Reference
module: class
function: MclassInquireEntry
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassInquireEntry"
---

# MclassInquireEntry

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

> Inquire about an entry in a dataset context.

## Syntax

```c
AIL_INT MclassInquireEntry(
    AIL_ID    DatasetContextClassId,  //in
    AIL_INT64 EntryIndex,             //in
    AIL_UUID  EntryKey,               //in
    AIL_INT64 RegionIndex,            //in
    AIL_INT64 InquireType,            //in
    void *    UserVarPtr              //out
)
```

## Description

This function allows you to inquire about an entry in a dataset context using its index or key. You can typically control these settings, using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md).

## Parameters

### `DatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the dataset about which to inquire settings. These contexts are allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md).

### `EntryIndex` *(in, AIL_INT64)*

Specifies the entry to inquire about, using the entry's index. Set this parameter to one of the following values.

*For specifying the index of an entry to inquire about*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the index of an entry is not required. |
| `Value >= 0` | Specifies the index of an entry. |

### `EntryKey` *(in, AIL_UUID)*

Specifies the entry to inquire about, using the entry's unique key (AIL_UUID). Set this parameter to one of the following values.

*For specifying the key (UUID) of an entry to inquire about*

| Value | Description |
| --- | --- |
| `M_DEFAULT_KEY` | Specifies that the unique key of an entry is not required. |
| `AIL_UUID Value` | Specifies the unique key of an entry. |

### `RegionIndex` *(in, AIL_INT64)*

Specifies which region in an entry to inquire. For an images dataset, there is always at least one region in the dataset, at index 0. For a features dataset, use [`M_DEFAULT`](../../Reference/class/MclassInquireEntry.md).

*For specifying which region to inquire*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the function call refers to the entry, and not to a specific region within the entry. |
| `M_REGION_INDEX` | Specifies the region to inquire, using its index.

Refers to a specific region within the entry. Note that there is always at least one region in an entry. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For an entry in a dataset context (images or features)

To inquire about an entry in a dataset, the [`InquireType`](../../Reference/class/MclassInquireEntry.md) parameter can be set to the following values. In this case, set the [`DatasetContextClassId`](../../Reference/class/MclassInquireEntry.md) parameter to the identifier of either an images dataset context or a features dataset context, unless otherwise specified, and set the [`RegionIndex`](../../Reference/class/MclassInquireEntry.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassInquireEntry.md).

---

### `M_ANOMALOUS`

Inquires whether an entry is considered anomalous. This is based on the entry's regions and their ground truths.  If an entry's whole image region ([`M_WHOLE_IMAGE`](../../Reference/class/MclassInquireEntry.md)) has a ground truth, only that class is used to determine whether the entry is considered anomalous (if other regions exist, their ground truth classes are not checked). If the whole image region does not have a ground truth, all of the entry's regions are checked to determine whether the entry is considered anomalous. If any entry region checked is associated to an anomalous ground truth class, the entry is considered anomalous.  If [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to the index of an anomalous class, unlabeled pixels are also checked, which can lead to an entry being considered anomalous.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the entry is not considered anomalous. |
| `M_TRUE` | Specifies that the entry is considered anomalous. |

---

### `M_ANOMALY_DETECTION_PATH`

Inquires the file path to the anomaly detection scores files, such that [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_ANOMALY_DETECTION_PATH`](../../Reference/class/MclassInquireEntry.md) gives a valid path. If [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) is empty, [`M_ANOMALY_DETECTION_PATH`](../../Reference/class/MclassInquireEntry.md) should be an absolute path, otherwise it should be a relative path to [`M_ROOT_PATH`](../../Reference/class/MclassControl.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

---

### `M_ANOMALY_DETECTION_PATH_ABS`

Inquires the absolute path to the anomaly detection scores file which corresponds to the concatenation of [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_ANOMALY_DETECTION_PATH`](../../Reference/class/MclassInquireEntry.md).

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the absolute path. |

---

### `M_AUGMENTATION_SOURCE`

Inquires whether an entry is an augmented entry, according to the index.  For entries added to a dataset by [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), this index refers to the prepared original image should it be present in the dataset.

| Value | Description |
| --- | --- |
| `M_NOT_AUGMENTED` | Specifies that the entry is not an augmented entry. |
| `M_UNKNOWN` | Specifies that the entry is augmented, however, the original data is not known. |
| `Value >= 0` | Specifies that the entry is augmented, and that it originates from (was performed with) the specified entry index. |

---

### `M_AUGMENTATION_SOURCE_KEY`

Inquires whether an entry is an augmented entry, according to the defined key (UUID).  For entries added to a dataset by [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), this key refers to the prepared original image should it be present in the dataset.

| Value | Description |
| --- | --- |
| *(see [`M_AUGMENTATION_SOURCE_KEY`](Reference/class/MclassControlEntry.md))* |  |

---

### `M_ENTRY_IMAGE_PATH`

Inquires the file path, such that [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassInquireEntry.md) gives a valid path. If [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) is empty, [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassInquireEntry.md) should be an absolute path, otherwise it should be a relative path to [`M_ROOT_PATH`](../../Reference/class/MclassControl.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"///DATASET///FilePath"` | Specifies the file path under the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md). |
| `"FilePath"` | Specifies the file path. |

---

### `M_ENTRY_IMAGE_PATH_ABS`

Inquires the absolute path of the entry image, which is the concatenation of [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md).

| Value | Description |
| --- | --- |
| `"FilePath"` | Specifies the file path. |

---

### `M_ENTRY_KEY`

Inquires the unique identifier for an entry.

---

### `M_ENTRY_USER_STRING`

Inquires the user string that holds meta information.

| Value | Description |
| --- | --- |
| `"String"` | Specifies a string containing meta information. |

---

### `M_ENTRY_WEIGHT`

Inquires the weight that indicates the importance of the feature entry.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the weight, as a _double_. |

---

### `M_EXTERNAL_SOURCE_KEY`

Inquires whether an entry has an external augmentation source, specified by its key (UUID). This value is set during a call to [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) and refers to the UUID of the source image in the source dataset.

| Value | Description |
| --- | --- |
| `M_NULL_KEY` | Specifies that the entry has no external augmentation source. This is the default value for entries created outside of [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). |
| `AIL_UUID Value` | Specifies that the entry has an external augmentation source with this key (UUID). |

---

### `M_NUMBER_OF_REGIONS`

Inquires the number of regions in this entry. The number of regions is equal to 1 plus the number of regions added, using [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md). The number of regions starts at 1 because region 0 refers to the region representing the whole image ([`M_WHOLE_IMAGE`](../../Reference/class/MclassInquireEntry.md) region), which is always present.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of regions, as an _integer_. |

---

### `M_RAW_DATA`

Inquires an array of feature values (set of features).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not required. |
| `M_NULL` | Specifies no values. |

---

### `M_SEGMENTATION_PATH`

Inquires the file path to the segmentation scores files, such that [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_SEGMENTATION_PATH`](../../Reference/class/MclassInquireEntry.md) gives a valid path. If [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) is empty, [`M_SEGMENTATION_PATH`](../../Reference/class/MclassInquireEntry.md) should be an absolute path, otherwise it should be a relative path to [`M_ROOT_PATH`](../../Reference/class/MclassControl.md).  If there is more than one file associated with this entry (because the number of classes is > 1024), the pipe delimiter is used between the files to differentiate them (for example, _GUID_0.mbufi|GUID_1.mbufi|GUID_2.mbufi_).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

---

### `M_SEGMENTATION_PATH_ABS`

Inquires the absolute path to the segmentation scores file which corresponds to the concatenation of [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_SEGMENTATION_PATH`](../../Reference/class/MclassInquireEntry.md).  If there is more than one file associated with this entry (because the number of classes is > 1024), the pipe delimiter is used between the files to differentiate them (for example, _Abs\Path\To\Segmentations\GUID_0.mbufi|Abs\Path\To\Segmentations\GUID_1.mbufi|Abs\Path\To\Segmentations\GUID_2.mbufi_).

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the absolute region mask path. |

---

### `M_USER_CONFIDENCE`

Inquires how confident you are in the entry's ground truth ([`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassInquireEntry.md)).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the user confidence. |

### For an entry or a region of an entry in a dataset context (images or features)

To inquire about a region in an entry or to inquire about an entry, the [`InquireType`](../../Reference/class/MclassInquireEntry.md) parameter can be set to the following values. In this case, set the [`DatasetContextClassId`](../../Reference/class/MclassInquireEntry.md) parameter to the identifier of any dataset context, unless otherwise specified. For an images dataset context, set the [`RegionIndex`](../../Reference/class/MclassInquireEntry.md) parameter to **M_REGION_INDEX()**, and for a features dataset context, set the [`RegionIndex`](../../Reference/class/MclassInquireEntry.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassInquireEntry.md).

---

### `M_AUTHOR_NAME`

Inquires the author's name.

| Value | Description |
| --- | --- |
| `"Name"` | Specifies the author's name. |

---

### `M_CLASS_INDEX_GROUND_TRUTH`

Inquires the index of the class definition that represents the entry's ground truth.  If there is no ground truth index, [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassInquireEntry.md) returns nothing to [`UserVarPtr`](../../Reference/class/MclassInquireEntry.md). To inquire if there is any ground truth index set, use [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassInquireEntry.md) + [`M_NB_ELEMENTS`](../../Reference/class/MclassInquireEntry.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to delete the entry's ground truth, when using whole image regions. |
| `M_DONT_CARE_CLASS` | Specifies that pixels assigned to the region do not contribute to the training loss, when using descriptor based regions (that is, any region index other than 0). |
| `0 <= Value < M_NUMBER_OF_CLASSES` | Specifies the class definition index (ground truth), as an _integer_. |

---

### `M_NUMBER_OF_DESCRIPTOR_TYPE_BOX`

Inquires the number of bounding box-type descriptors.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of bounding box-type descriptors. |

---

### `M_NUMBER_OF_DESCRIPTOR_TYPE_MASK`

Inquires the number of mask-type descriptors.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of mask-type descriptors. |

---

### `M_NUMBER_OF_DESCRIPTOR_TYPE_POLYGON`

Inquires the number of polygon-type descriptors.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of polygon-type descriptors. |

---

### `M_REGION_MASK_PATH`

Inquires the path of a region descriptor context, relative to [`M_ROOT_PATH`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path of the region descriptor context. |

---

### `M_REGION_MASK_PATH_ABS`

Inquires the absolute region mask path.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the absolute region mask path. |

---

### `M_REGION_TYPE`

Inquires the type of the region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DESCRIPTOR_BASED` | Specifies that the region is descriptor based. Regions at any index other than 0 will be of this type. |
| `M_WHOLE_IMAGE` *(default)* | Specifies that the region type of the entry is set to the whole image. Regions at index 0 are always of this type. |

---

### `M_REGION_USE`

Inquires whether a region is to be used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_USE`](../../Reference/class/MclassInquireEntry.md). |
| `M_IGNORE` | Specifies that the region is to be ignored. |
| `M_USE` | Specifies that the region can be used. |

### Combination Constants — For inquiring the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported for the dataset entry.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For casting the requested information to the required data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

This value only applies to an images dataset context.

This value only applies to a features dataset context.
