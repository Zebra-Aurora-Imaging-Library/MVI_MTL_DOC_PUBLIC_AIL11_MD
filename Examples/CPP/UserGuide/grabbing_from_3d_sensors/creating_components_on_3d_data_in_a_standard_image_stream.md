---
title: "creating_components_on_3d_data_in_a_standard_image_stream"
description: "Creating components on 3d data grabbed from a standard image stream"
ms.snippet: "userguide.grabbing_from_3d_sensors.creating_components_on_3d_data_in_a_standard_image_stream"
ms.language: "C++"
ms.env: "vc"
ms.version: "10.4"
---

# creating_components_on_3d_data_in_a_standard_image_stream

> Creating components on 3d data grabbed from a standard image stream

**Language:** C++ | **Version:** 10.4+

```cpp
/* Allocate a buffer for grabbing from 3D sensor that transmits a point cloud as four 512 by 512 blocks */
/* representing X-coordinates, Y-coordinates, Z-coordinates, and reflectance respectively.              */
AIL_ID GrabBuffer = MbufAlloc2d(AilSystem, 512, 2048, 16 + M_UNSIGNED, M_IMAGE + M_GRAB, M_NULL);

/* Calculate the host address of each type of data in the grab buffer. */
void* AddressOfRangeX = (void*)MbufInquire(GrabBuffer, M_HOST_ADDRESS, M_NULL);
void* AddressOfRangeY = (void*)(MbufInquire(GrabBuffer, M_HOST_ADDRESS, M_NULL)
   + MbufInquire(GrabBuffer, M_PITCH_BYTE, M_NULL) * 512);
void* AddressOfRangeZ = (void*)(MbufInquire(GrabBuffer, M_HOST_ADDRESS, M_NULL)
   + MbufInquire(GrabBuffer, M_PITCH_BYTE, M_NULL) * 1024);

void* AddressOfRangeXYZ[3] = { AddressOfRangeX, AddressOfRangeY, AddressOfRangeZ };

void* AddressOfReflectance[] = { (void*)(MbufInquire(GrabBuffer, M_HOST_ADDRESS, M_NULL)
   + MbufInquire(GrabBuffer, M_PITCH_BYTE, M_NULL) * 1536) };

/* Allocate a container that will have components mapped onto the same memory as the grab buffer, */
/* and a container that will store data converted for processing and display.                     */
AIL_ID Grab3dContainer = MbufAllocContainer(AilSystem, M_PROC, M_DEFAULT, M_NULL);
AIL_ID Converted3dContainer = MbufAllocContainer(AilSystem, M_DISP + M_PROC, M_DEFAULT, M_NULL);

/* Create a 3-band range component and a 1-band reflectance component */
/* mapped onto the same memory as the grab buffer.                    */
AIL_ID RangeComponent = MbufCreateComponent(Grab3dContainer, 3, 512, 512, 16 + M_UNSIGNED, M_IMAGE,
   M_HOST_ADDRESS + M_PITCH, M_DEFAULT, AddressOfRangeXYZ,
   M_COMPONENT_RANGE, M_NULL);
AIL_ID ReflectanceComponent = MbufCreateComponent(Grab3dContainer, 1, 512, 512, 16 + M_UNSIGNED, M_IMAGE,
   M_HOST_ADDRESS + M_PITCH, M_DEFAULT, AddressOfReflectance,
   M_COMPONENT_REFLECTANCE, M_NULL);

/* Set the scale settings of the created range component, using settings either provided by the */ 
/* camera manufacturer or required by the application. The coordinates in the grabbed data will */
/* be multiplied by the scale setting when converted using MbufConvert3D().                     */

MbufControlContainer(Grab3dContainer, M_COMPONENT_RANGE, M_3D_SCALE_X, 0.13f);
MbufControlContainer(Grab3dContainer, M_COMPONENT_RANGE, M_3D_SCALE_Y, 0.13f);
MbufControlContainer(Grab3dContainer, M_COMPONENT_RANGE, M_3D_SCALE_Z, 0.29f);

/* Enable the use of an invalid data value to indicate invalid points in the range component, */
/* and set that value to the invalid data value specified by your camera manufacturer.        */
MbufControlContainer(Grab3dContainer, M_COMPONENT_RANGE, M_3D_INVALID_DATA_FLAG, M_TRUE);
MbufControlContainer(Grab3dContainer, M_COMPONENT_RANGE, M_3D_INVALID_DATA_VALUE, 1984);

while(Grabbing){

   /* Grab new data into the grab buffer. */
   MdigGrab(AILDig, GrabBuffer);

   /* Convert the data in the container on which you created the components to a format supported */
   /* for processing and display. The converted data is stored in automatically allocated         */
   /* components in the destination container.                                                    */
   MbufConvert3d(Grab3dContainer, Converted3dContainer, M_NULL, M_DEFAULT, M_COMPENSATE);
}
```
