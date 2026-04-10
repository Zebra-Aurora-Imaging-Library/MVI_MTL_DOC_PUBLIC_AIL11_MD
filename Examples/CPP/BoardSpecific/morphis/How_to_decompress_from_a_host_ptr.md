---
title: "How_to_decompress_from_a_host_ptr"
description: "Example of how to decompress data from a host pointer"
ms.snippet: "boardspecific.morphis.How_to_decompress_from_a_host_ptr"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# How_to_decompress_from_a_host_ptr

> Example of how to decompress data from a host pointer

**Language:** C++ | **Version:** 8.0+

```cpp
while(!MosKbhit())
{
 MappTimer(M_TIMER_READ, &TimeStart);

 // pJP2KCodeStream is a pointer to valid jpeg2000 data.
 MbufInquire(AilImageCompress, M_HOST_ADDRESS, &pJP2KCodeStream);

 // Map an Aurora Imaging Library buffer on the Host pointer.
 MbufCreateColor(AilSystem,  SizeBand, SizeX, SizeY, 8L+M_UNSIGNED,
				 M_IMAGE + M_COMPRESS + M_JPEG2000_LOSSY
				 + (SizeBand == 3? M_YUV16 + M_PLANAR: 0),
				 M_HOST_ADDRESS,
				 M_DEFAULT,
				 (void**)&pJP2KCodeStream,
				 &AilImageCreateCompress);

 // Decompress the buffer.
 MbufCopy(AilImageCreateCompress, AilImageDisp);
}
```
