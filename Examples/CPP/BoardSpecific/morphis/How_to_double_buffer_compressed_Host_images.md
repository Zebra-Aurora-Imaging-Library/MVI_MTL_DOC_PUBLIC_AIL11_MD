---
title: "How_to_double_buffer_compressed_Host_images"
description: "Example of how to double-buffer compressed Host images."
ms.snippet: "boardspecific.morphis.How_to_double_buffer_compressed_Host_images"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# How_to_double_buffer_compressed_Host_images

> Example of how to double-buffer compressed Host images.

**Language:** C++ | **Version:** 8.0+

```cpp
// Grab images into Host buffers.
for(i = 0; i < 2; i++)
   MdigGrab(AilDigitizer, AilImage[i]);

// Set the compression to be asynchronous.
MthrControl(AilSystem, M_DMA_COPY_MODE, M_ASYNCHRONOUS);

i = 0;
MbufCopy(AilImage[i], AilImageOnBoard[i]);
while(!MosKbhit())
   {
   MappTimer(M_TIMER_READ, &TimeStart);

   // Perform the compression of on-board image asynchronously.
   MbufCopy(AilImageOnBoard[i], AilImageCompress);

   // While the image is being compressed, copy the next image into
   // on-board memory.
   i = 1 - i;
   MbufCopy(AilImage[i], AilImageOnBoard[i]);

   // Wait for the compression to terminate.
   MthrWait(AilSystem, M_THREAD_WAIT, M_NULL);

   MappTimer(M_TIMER_READ, &TimeEnd);
   MosPrintf(AIL_TEXT("\rPerforming precompressions of host buffers at "));
   MosPrintf(AIL_TEXT(" %3.1f frames/sec"), 1.0/(TimeEnd-TimeStart));
   MosPrintf(AIL_TEXT(" (%1.4f sec)"), (TimeEnd-TimeStart));
   }

MthrControl(AilSystem, M_DMA_COPY_MODE, M_SYNCHRONOUS);
```
