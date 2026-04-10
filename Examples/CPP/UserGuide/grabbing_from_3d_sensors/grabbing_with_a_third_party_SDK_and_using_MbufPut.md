---
title: "grabbing_with_a_third_party_SDK_and_using_MbufPut"
description: "Putting 3D data grabbed using a third-party SDK in an Aurora Imaging Library container"
ms.snippet: "userguide.grabbing_from_3d_sensors.grabbing_with_a_third_party_SDK_and_using_MbufPut"
ms.language: "C++"
ms.env: "vc"
ms.version: "10.4"
---

# grabbing_with_a_third_party_SDK_and_using_MbufPut

> Putting 3D data grabbed using a third-party SDK in an Aurora Imaging Library container

**Language:** C++ | **Version:** 10.4+

```cpp
/* Obtain data from a third-party SDK that grabs 3D data in four 512 by 512 blocks       */
/* representing X-coordinates, Y-coordinates, Z-coordinates, and reflectance respectively. */

/* Allocate a container that will store the data obtained from the third-party SDK */
/* and a container that will store data converted for processing and display.      */
AIL_ID Unconverted3dContainer = MbufAllocContainer(AilSystem, M_PROC, M_DEFAULT, M_NULL);
AIL_ID Converted3dContainer = MbufAllocContainer(AilSystem, M_DISP + M_PROC, M_DEFAULT, M_NULL);

/* Allocate a 3-band range component and a 1-band reflectance component  */
/* to store the data so that it can be converted using MbufConvert3d() */
AIL_ID RangeComponent = MbufAllocComponent(Unconverted3dContainer, 3, 512, 512, 16 + M_UNSIGNED
   , M_IMAGE + M_PROC, M_COMPONENT_RANGE, M_NULL);
AIL_ID ReflectanceComponent = MbufAllocComponent(Unconverted3dContainer, 1, 512, 512, 16 + M_UNSIGNED
   , M_IMAGE + M_PROC, M_COMPONENT_INTENSITY, M_NULL);

/* Allocate arrays to store the data grabbed using the third-party SDK */
unsigned short XCoordinates[512 * 512];
unsigned short YCoordinates[512 * 512];
unsigned short ZCoordinates[512 * 512];
unsigned short ReflectanceValues[512 * 512];

/* Set the scale settings of the allocated range component, using settings either provided by the     */
/* camera manufacturer or required by the application. The coordinates in the data obtained from the  */
/* third-party SDK will be multiplied by the scale setting when converted using MbufConvert3d().      */
MbufControlContainer(Unconverted3dContainer, M_COMPONENT_RANGE, M_3D_SCALE_X, 0.13f);
MbufControlContainer(Unconverted3dContainer, M_COMPONENT_RANGE, M_3D_SCALE_Y, 0.13f);
MbufControlContainer(Unconverted3dContainer, M_COMPONENT_RANGE, M_3D_SCALE_Z, 0.29f);

/* Enable the use of an invalid data value to indicate invalid points in the range component.                  */
/* The default invalid data value is 0. You can change the invalid data value using M_3D_INVALID_DATA_VALUE.   */
/* In some cases, it might be simpler to allocate a confidence component and use it to indicate invalid points */
/* in the grabbed data instead.                                                                                */
MbufControlContainer(Unconverted3dContainer, M_COMPONENT_RANGE, M_3D_INVALID_DATA_FLAG, M_TRUE);

/* Grab and convert the data */
while (Grabbing){

   /* Using the third-party SDK, grab the data and copy the appropriate values into each array.                         */
   /* You might need to use the third-party SDK to perform additional operations on the data (such as calibrating it)   */
   /* before copying it into the arrays. Ensure that invalid points have a Z-coordinate of 0, so that these values will */
   /* be marked as invalid data in the converted container.                                                             */

   /* Use MbufPutColor() and MbufPut() to copy the data from the arrays into the previously allocated components */
   MbufPutColor(RangeComponent, M_SINGLE_BAND, 0, XCoordinates);
   MbufPutColor(RangeComponent, M_SINGLE_BAND, 1, YCoordinates);
   MbufPutColor(RangeComponent, M_SINGLE_BAND, 2, ZCoordinates);
   MbufPut(ReflectanceComponent, ReflectanceValues);

   /* Convert the data in the container in which you put the data for processing and display.            */
   /*  The converted data is stored in automatically allocated components in the destination container.  */
   MbufConvert3d(Unconverted3dContainer, Converted3dContainer, M_NULL, M_DEFAULT, M_COMPENSATE);
}
```
