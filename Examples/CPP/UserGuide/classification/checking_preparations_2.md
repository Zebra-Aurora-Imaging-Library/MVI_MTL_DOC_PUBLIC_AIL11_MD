---
title: "checking_preparations_2"
description: "attaching and using hook function"
ms.snippet: "userguide.classification.checking_preparations_2"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# checking_preparations_2

> attaching and using hook function

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate a data preparation context (this will also work for data preparation contexts acquired using the M_PREPARE_DATA_CONTEXT_ID inquire)
auto DataPreparationContext = MclassAlloc(SysId, M_PREPARE_IMAGES_CNN, M_DEFAULT, M_UNIQUE_ID);

// Attach the hook function so it gets called after the preparation of each entry
MclassHookFunction(DataPreparationContext, M_PREPARE_ENTRY_POST, HookFuncCheckPrepareEntryStatus, M_NULL);

// Set some prepare data controls here
//...

// Preprocess the data preparation context
MclassPreprocess(DataPreparationContext, M_DEFAULT);

// Call prepare data which will now call the hook function
MclassPrepareData(DataPreparationContext, SrcDatasetClassId, DstDatasetClassId, M_NULL, M_DEFAULT);
```
