---
doctype: Reference
module: class
function: MclassControlEntry
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassControlEntry"
---

# MclassControlEntry

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

> Control an entry in a dataset context.

## Syntax

```c
void MclassControlEntry(
    AIL_ID       DatasetContextClassId,  //in
    AIL_INT64    EntryIndex,             //in
    AIL_UUID     EntryKey,               //in
    AIL_INT64    RegionIndex,            //in
    AIL_INT64    ControlType,            //in
    AIL_DOUBLE   ControlValue,           //in
    const void * ControlValuePtr,        //in
    AIL_INT      ControlValuePtrSize     //in
)
```

## Description

This function allows you to control a setting of an entry in a dataset context using its index or key. You can typically inquire these settings, using [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md).

To add entries to, or delete entries from, a dataset context, use [`MclassControl`](../../Reference/class/MclassControl.md). You can also import data from a CSV file to a dataset context, using [`MclassImport`](../../Reference/class/MclassImport.md).

## Parameters

### `DatasetContextClassId` *(in, AIL_ID)*

Specifies the dataset on which to control settings. These contexts are allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md).

### `EntryIndex` *(in, AIL_INT64)*

Specifies the entry to control, using the entry's index. Set this parameter to one of the following values.

*For specifying the index of the entry to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the entry index is not required. In this case, you must set the [`EntryKey`](../../Reference/class/MclassControlEntry.md) parameter to the key (UUID) of the entry to control. |
| `Value >= 0` | Specifies the index of the entry to control. In this case, you must set the [`EntryKey`](../../Reference/class/MclassControlEntry.md) parameter to [`M_DEFAULT_KEY`](../../Reference/class/MclassControlEntry.md). |

### `EntryKey` *(in, AIL_UUID)*

Specifies the entry to control, using the entry's unique key (UUID). Set this parameter to one of the following values.

*For specifying the key (UUID) of the entry to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT_KEY` | Specifies that the entry's key (UUID) is not required. In this case, you must set the [`EntryIndex`](../../Reference/class/MclassControlEntry.md) parameter to the index of the entry to control. |
| `AIL_UUID Value` | Specifies the key (UUID) of the entry to control. In this case, you must set the [`EntryIndex`](../../Reference/class/MclassControlEntry.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassControlEntry.md). The key is defined as an Aurora Imaging Library universal unique identifier (UUID). To get an entry's key, call [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md) with [`M_ENTRY_KEY`](../../Reference/class/MclassInquireEntry.md). |

### `RegionIndex` *(in, AIL_INT64)*

Specifies to control the entry, or to control the entry's region. Set this parameter to one of the following values.

*For specifying whether to control the entry or the entry's region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to control the entry.

You must specify [`M_DEFAULT`](../../Reference/class/MclassControlEntry.md) if you are using a features dataset context. |
| `M_REGION_INDEX` | Specifies to control the entry's region. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value for the control. If this information is not required, set this parameter to `M_DEFAULT`.

### `ControlValuePtr` *(in, *void)*

Specifies the address of the variable which contains information for the type of control to set. If this information is not required, set this parameter to `M_NULL`.

### `ControlValuePtrSize` *(in, AIL_INT)*

Specifies the number of elements in [`ControlValuePtr`](../../Reference/class/MclassControlEntry.md). If this information is not needed, set this parameter to `M_DEFAULT`.

## Parameter Associations

### For an entry in a dataset context (images or features)

To control an entry in a dataset, the [`ControlType`](../../Reference/class/MclassControlEntry.md) and corresponding [`ControlValue`](../../Reference/class/MclassControlEntry.md) and [`ControlValuePtr`](../../Reference/class/MclassControlEntry.md)parameter settings can be set to the following values. In this case, set the [`DatasetContextClassId`](../../Reference/class/MclassControlEntry.md) parameter to the identifier of either an images dataset context or a features dataset context, unless otherwise specified, and set the [`RegionIndex`](../../Reference/class/MclassControlEntry.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassControlEntry.md).

---

### `M_AUGMENTATION_SOURCE`

Sets whether an entry is an augmented entry, according to its index.  For entries added to a dataset by [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), this key refers to the prepared original image should it be present in the dataset.

| Value | Description |
| --- | --- |
| `M_NOT_AUGMENTED` | Specifies that the entry is not an augmented entry. |
| `M_UNKNOWN` | Specifies that the entry is augmented, however, the original data is not known. |
| `Value >= 0` | Specifies that the entry is augmented, and that it originates from (was performed with) the specified entry index. The entry from which the augmentation originates is considered a source or parent entry. |

---

### `M_AUGMENTATION_SOURCE_KEY`

Sets whether an entry is an augmented entry, according to its key (UUID).  For entries added to a dataset by [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), this key refers to the prepared original image should it be present in the dataset.

| Value | Description |
| --- | --- |
| `M_NULL_KEY` | Specifies that the entry is not an augmented entry. |
| `M_UNKNOWN_KEY` | Specifies that the entry is augmented, and that the entry key (UUID) from which it was augmented is not known. |
| `AIL_UUID Value` | Specifies that the entry is augmented, and that it originates from (was performed with) the specified key. The entry from which the augmentation originates is considered a source or parent entry. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).  To specify an _AIL_UUID_ value when using a C compiler, you must call the _AIL_UUID_ version of this function (MclassControlEntryAilUuid). When using a C++ or other compiler, [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) internally calls the AIL_UUID version of this function.  | AIL_UUID version of MclassControlEntry | | --- | | void MclassControlEntryAilUuid | (AIL_ID DatasetContextClassId, AIL_INT64 EntryIndex, AIL_UUID EntryKey, AIL_INT64 RegionIndex, AIL_INT64 ControlType, AIL_UUID ControlValue, const void *ControlValuePtr, AIL_INT ControlValuePtrSize) | |

---

### `M_ENTRY_IMAGE_PATH`

Sets the location from which to get the entry's image, such that [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md) gives a valid path. If [`M_ROOT_PATH`](../../Reference/class/MclassControl.md) is empty, [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md) should be an absolute path, otherwise it should be a relative path to [`M_ROOT_PATH`](../../Reference/class/MclassControl.md). The maximum size of the entire file path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md) + [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md)) is 259 characters.

| Value | Description |
| --- | --- |
| `"///DATASET///FilePath"` | Specifies the file path under the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md). If the ///DATASET/// path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"FilePath"` | Specifies the file path. |

---

### `M_ENTRY_USER_STRING`

Sets a user string to store meta information.

| Value | Description |
| --- | --- |
| `"String"` | Specifies a string containing meta information. This string must not contain commas. |

---

### `M_ENTRY_WEIGHT`

Sets a weight that indicates the importance of the feature entry.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the weight, as a _double_. |

---

### `M_RAW_DATA`

Sets an array of values (a set of features).  In this case, set the [`DatasetContextClassId`](../../Reference/class/MclassControlEntry.md) parameter to the identifier of a features dataset context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not required. |

---

### `M_REGION_DELETE`

Sets the index of a region to remove from a specified entry.

| Value | Description |
| --- | --- |
| `0 < Value < M_NUMBER_OF_REGIONS` | Specifies a specific region to delete within the entry.  Note that there is always at least one region in an entry. Region index 0 cannot be deleted. |

---

### `M_USER_CONFIDENCE`

Sets how confident you are in the entry's ground truth ([`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md)). The confidence is not used in any calculations; it is for information purposes only. For example, when sharing datasets among multiple users, it can be useful to know how confident you should be of an entry's ground truth.  You must decide the confidence scale to use; all confidence values should follow the chosen convention.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the user confidence. |

### For an entry or a region of an entry in a dataset context (images or features)

To control an entry or to control a region of an entry, the [`ControlType`](../../Reference/class/MclassControlEntry.md) and corresponding [`ControlValue`](../../Reference/class/MclassControlEntry.md) and [`ControlValuePtr`](../../Reference/class/MclassControlEntry.md)parameter settings can be set to the following values. In this case, set the [`DatasetContextClassId`](../../Reference/class/MclassControlEntry.md) parameter to the identifier of a dataset context and, for an images dataset context, set the [`RegionIndex`](../../Reference/class/MclassControlEntry.md) parameter to **M_REGION_INDEX()**, and for a features dataset context, set the [`RegionIndex`](../../Reference/class/MclassControlEntry.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassControlEntry.md).

---

### `M_AUTHOR_NAME`

Sets the author's name.

| Value | Description |
| --- | --- |
| `"Name"` | Specifies the author's name. |

---

### `M_CLASS_INDEX_GROUND_TRUTH`

Sets the index of the class definition that represents the entry's ground truth.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to delete the entry's ground truth, when using whole image regions. To inquire if you are using whole image regions ([`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md)), [`M_REGION_TYPE`](../../Reference/class/MclassInquireEntry.md) must return [`M_WHOLE_IMAGE`](../../Reference/class/MclassInquireEntry.md). To delete the ground truth, you must also set the [`ControlValuePtr`](../../Reference/class/MclassControlEntry.md) parameter to [`M_NULL`](../../Reference/class/MclassControlEntry.md) and the [`ControlValuePtrSize`](../../Reference/class/MclassControlEntry.md) parameter to 0. |
| `M_DONT_CARE_CLASS` | Specifies that pixels assigned to the region do not contribute to the training loss, when using descriptor based regions (that is, any region index other than 0). To inquire if you are using descriptor based regions ([`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md)), [`M_REGION_TYPE`](../../Reference/class/MclassInquireEntry.md) must return [`M_DESCRIPTOR_BASED`](../../Reference/class/MclassInquireEntry.md).  This don't care class ([`M_DONT_CARE_CLASS`](../../Reference/class/MclassInquireEntry.md)) is always available as a class definition that you can assign as the ground truth to a descriptor based region (that is, you do not need to add a don't care class definition).  Typically, regions assigned to the don't care class will be areas of the image where the class of the object is unknown. As well, these regions can be assigned to the border pixels between two classes where labeling the edge between them can be difficult.  You can set the color of the don't care regions with the [`M_DONT_CARE_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) control. |
| `0 <= Value < M_NUMBER_OF_CLASSES` | Specifies the class definition index (ground truth), as an _integer_. |

---

### `M_DESCRIPTOR`

Replaces the descriptors for the region. The same restrictions to the descriptors apply here as they do in [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md). You must pass either an image buffer identifier or a graphics list identifier. Region index 0 cannot be replaced. This will only replace descriptors of the same type.  If you replace a mask descriptor, the region mask path ([`M_REGION_MASK_PATH`](../../Reference/class/MclassControlEntry.md)) will remain the same and the mask file on the disk will be overwritten.  This control is mainly meant for modifying existing region descriptors. When adding them for the first time it is recommended to use [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md).

| Value | Description |
| --- | --- |
| `2D graphics list identifier` | Sets the identifier of the 2D graphics list from which to establish the descriptor for the region. Graphics must not be defined in world units. |
| `Image buffer identifier` | Sets the identifier of the image buffer from which to establish the added region. This image buffer must not have an ROI associated with it ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)). |

---

### `M_DESCRIPTOR_ADD`

Adds new descriptors for the region. The resulting group of descriptors must comply with the same restrictions as in [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md). You must pass either an image buffer identifier or a graphics list identifier. Region index 0 cannot be modified.  This control is mainly meant for adding to existing region descriptors. When adding them for the first time it is recommended to use [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md).

| Value | Description |
| --- | --- |
| `2D graphics list identifier` | Sets the identifier of the 2D graphics list from which to establish the descriptor for the region. Graphics must not be defined in world units. |
| `Image buffer identifier` | Sets the identifier of the image buffer from which to establish the added region. This image buffer must not have an ROI associated with it ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)). |

---

### `M_DESCRIPTOR_DELETE`

Deletes all descriptors of a given type. No descriptors can be deleted from region index 0.

| Value | Description |
| --- | --- |
| `M_ALL_DESCRIPTORS` | Specifies to delete all descriptors. |
| `M_DESCRIPTOR_TYPE_BOX` | Specifies to delete all box-type descriptors. |
| `M_DESCRIPTOR_TYPE_MASK` | Specifies to delete all mask-type descriptors. |
| `M_DESCRIPTOR_TYPE_POLYGON` | Specifies to delete all polygon-type descriptors. |

---

### `M_REGION_MASK_PATH`

Sets the mask path of a region descriptor.  The buffer file that this path points to should be a 1-band buffer where all non-zero pixels are assumed to be part of the region. As well, this buffer should have the same size as the entry image pointed to by the control [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md). You cannot set the mask path for region index 0.

| Value | Description |
| --- | --- |
| `"///DATASET///FilePath"` | Specifies the file path under the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md). If the ///DATASET/// path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"FilePath"` | Specifies the file path. |

---

### `M_REGION_USE`

Sets whether the specified region will be used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_USE`](../../Reference/class/MclassControlEntry.md). |
| `M_IGNORE` | Specifies that the region is to be ignored. |
| `M_USE` | Specifies that the region can be used. |

This value only applies to an images dataset context.

This value only applies to a features dataset context.
