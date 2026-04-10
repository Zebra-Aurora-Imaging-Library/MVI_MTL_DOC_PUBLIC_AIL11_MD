---
title: "using_multi_frame_buffer"
ms.snippet: "userguide.image_processing.using_multi_frame_buffer"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# using_multi_frame_buffer

**Language:** C++ | **Version:** 7.5+

```cpp
   //<DESCRIPTION NAME="userguide.image_processing.using_multi_frame_buffer"/>

	/* Locate peak 1d context allocation. */
	AIL_ID LocatePeakContextId = MimAlloc(AilSystemId, M_LOCATE_PEAK_1D_CONTEXT, M_DEFAULT, M_NULL);

	/* Locate peak 1d result allocation. */
	AIL_ID LocatePeakResultId = MimAllocResult(AilSystemId, M_DEFAULT, M_LOCATE_PEAK_1D_RESULT, M_NULL);

	/**/
	/* Set the maximum number of frames to store in the result. */
	MimControl(LocatePeakContextId, M_NUMBER_OF_FRAMES, 100);

	/* Set the Y-size of each frame in the image. */
	MimControl(LocatePeakContextId, M_FRAME_SIZE, 32);

	/* Set the minimum contrast for extraction. */
	MimControl(LocatePeakContextId, M_MINIMUM_CONTRAST, 50);

	/* Set the nominal peak width for extraction. */
	MimControl(LocatePeakContextId, M_PEAK_WIDTH_NOMINAL, 10);

	/* Set the peak width delta for extraction. */
	MimControl(LocatePeakContextId, M_PEAK_WIDTH_DELTA, 10);

	/* Set the maximum number of peaks per scan lane to extract per frame. */
	MimControl(LocatePeakContextId, M_NUMBER_OF_PEAKS, 2);

	/* Set the direction of the extraction process. */
	MimControl(LocatePeakContextId, M_SCAN_LANE_DIRECTION, M_VERTICAL);

	/**/
	/* Set the sort parameters for the extracted peaks to strongest peak first. */
	MimControl(LocatePeakResultId, M_SORT_CRITERION, M_PEAK_INTENSITY + M_SORT_DOWN);

	/**/
	/* Perform preprocessing. */
	MimLocatePeak1d(LocatePeakContextId, SrcId, LocatePeakResultId, M_NULL, M_NULL, M_NULL, M_PREPROCESS, M_DEFAULT);

	while (Processing)
	   {
	   /**/
	   /* Perform peak extraction on multiple frames.*/
	   MimLocatePeak1d(LocatePeakContextId, SrcId, LocatePeakResultId, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);

	   /* Extract the best peaks in all scan lanes of all frames. */
	   MimGetResultSingle(LocatePeakResultId, M_SELECT_PEAK(M_ALL, 0), M_ALL, ResultType, UserPtr);

	   /**/
	   }
	/**/
	/* Release the locate peak 1d resources. */
	MimFree(LocatePeakResultId);
	MimFree(LocatePeakContextId);
```
