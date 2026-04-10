---
title: "specific_augmentations"
description: "Setting specific augmentations for a dataset when the defaults are not sufficient"
ms.snippet: "userguide.classification.specific_augmentations"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# specific_augmentations

> Setting specific augmentations for a dataset when the defaults are not sufficient

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate a data preparation context (this will also work for data preparation contexts acquired using the M_PREPARE_DATA_CONTEXT_ID inquire)
auto DataPreparationContext = MclassAlloc(SysId, M_PREPARE_IMAGES_CNN, M_DEFAULT, M_UNIQUE_ID);

// Inquire the internal augment context
AIL_ID AugmentContext = MclassInquire(DataPreparationContext, M_DEFAULT, M_AUGMENT_CONTEXT_ID + M_TYPE_AIL_ID, M_NULL);

// Enable some augmentations
MimControl(AugmentContext, M_AUG_HUE_OFFSET_OP, M_ENABLE);
MimControl(AugmentContext, M_AUG_HUE_OFFSET_OP + M_PROBABILITY, 0.5);
MimControl(AugmentContext, M_AUG_HUE_OFFSET_OP_MAX, 360);
MimControl(AugmentContext, M_AUG_HUE_OFFSET_OP_MIN, 0);

MimControl(AugmentContext, M_AUG_LIGHTING_DIRECTIONAL_OP, M_ENABLE);
MimControl(AugmentContext, M_AUG_LIGHTING_DIRECTIONAL_OP + M_PROBABILITY, 0.5);
MimControl(AugmentContext, M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MAX, 1.2);
MimControl(AugmentContext, M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MIN, 0.8);
```
