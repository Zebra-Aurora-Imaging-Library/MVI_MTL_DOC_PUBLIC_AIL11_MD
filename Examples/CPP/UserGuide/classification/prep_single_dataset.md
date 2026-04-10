---
title: "prep_single_dataset"
description: "Setting data preparation for a single dataset training"
ms.snippet: "userguide.classification.prep_single_dataset"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# prep_single_dataset

> Setting data preparation for a single dataset training

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate a training context
auto TrainContext = MclassAlloc(SysId, M_TRAIN_CNN, M_DEFAULT, M_UNIQUE_ID);

//Inquire the internal data preparation context
AIL_ID DataPreparationContext = MclassInquire(TrainContext, M_DEFAULT, M_PREPARE_DATA_CONTEXT_ID + M_TYPE_AIL_ID, M_NULL);

// Set seed for reproducibility.
MclassControl(DataPreparationContext, M_DEFAULT, M_SEED_MODE, M_USER_DEFINED);
MclassControl(DataPreparationContext, M_DEFAULT, M_SEED_VALUE, 25);

// Set some basic augmentation controls.
MclassControl(DataPreparationContext, M_DEFAULT, M_AUGMENT_NUMBER_MODE, M_FACTOR);
MclassControl(DataPreparationContext, M_DEFAULT, M_AUGMENT_NUMBER_FACTOR, 10.0);

// Enable some augmentation presets.
MclassControl(DataPreparationContext, M_DEFAULT, M_PRESET_ROTATION, M_ENABLE);
MclassControl(DataPreparationContext, M_DEFAULT, M_PRESET_TRANSLATION, M_ENABLE);
```
