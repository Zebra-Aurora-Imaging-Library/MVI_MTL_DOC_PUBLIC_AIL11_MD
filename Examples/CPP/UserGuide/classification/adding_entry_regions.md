---
title: "adding_entry_regions"
description: "Uses MclassEntryAddRegion() to add a bounding box to a dataset"
ms.snippet: "userguide.classification.adding_entry_regions"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# adding_entry_regions

> Uses MclassEntryAddRegion() to add a bounding box to a dataset

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate a graphics list
AIL_UNIQUE_GRA_ID BBox = MgraAllocList(AilSystem, M_DEFAULT, M_UNIQUE_ID);

// Add a rectangle graphic to the graphics list
MgraRect(M_DEFAULT, BBox, 4, 4, 16, 16);

// Add the rectangle to the entry in the dataset
MclassEntryAddRegion(Dataset, EntryIdx, M_DEFAULT_KEY, M_DESCRIPTOR_TYPE_BOX, BBox, M_NULL, ClassIdx, M_DEFAULT);
```
