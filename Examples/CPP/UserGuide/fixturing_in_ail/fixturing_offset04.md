---
title: "fixturing_offset04"
description: "Using a fixturing offset object"
ms.snippet: "userguide.fixturing_in_ail.fixturing_offset04"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# fixturing_offset04

> Using a fixturing offset object

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set up phase. */

/* Find the model contained in the Model Finder context ContextId
in the training image. */
MmodFind(ModContextId, TrainingImageId, ModResultId);

/* Make sure the model is found. */
/* ... */

/* Learn the location of the model reference location in the
relative coordinate system of the training image. */
McalFixture(M_NULL, FixturingOffsetId, M_LEARN_OFFSET, M_RESULT_MOD,
	ModResultId, 0, /* Model finder result and occurrence index. */
	M_DEFAULT, M_DEFAULT, M_DEFAULT);

/* Critical loop. */
while (!Stop)
{
	/* Acquire image. */
	/* ... */

	/* Find the model in GrabbedImageId. */
	MmodFind(ModContextId, GrabbedImageId, ModResultId);

	/* Make sure the model is found. */
	/* ... */

	/* Move the relative coordinate system of GrabbedImageId,
	with the learned offset,
	according to the location of the occurrence of the model. */
	McalFixture(GrabbedImageId, FixturingOffsetId, M_MOVE_RELATIVE, M_RESULT_MOD,
		ModResultId, 0, /* Model finder result and occurrence index. */
		M_DEFAULT, M_DEFAULT, M_DEFAULT);

	/* Do some analysis. */
	/* ... */
}
```
