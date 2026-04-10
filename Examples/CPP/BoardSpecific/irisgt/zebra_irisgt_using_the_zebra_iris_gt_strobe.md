---
title: "zebra_irisgt_using_the_zebra_iris_gt_strobe"
description: "Iris GT strobe"
ms.snippet: "boardspecific.irisgt.zebra_irisgt_using_the_zebra_iris_gt_strobe"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# zebra_irisgt_using_the_zebra_iris_gt_strobe

> Iris GT strobe

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_ID AilApplication,  /* Application identifier.  */
       AilSystem,       /* System identifier.       */
       AilDisplay,      /* Display identifier.      */
       AilDigitizer,    /* Digitizer identifier.    */ 
       AilImage;        /* Image buffer identifier. */

AIL_DOUBLE ExposureTime = 9000 * 1000;
AIL_DOUBLE ExposureTimeDelay = ExposureTime / 3;
AIL_DOUBLE StrobeLength = ExposureTime + ExposureTimeDelay;
AIL_DOUBLE StrobeTimeDelay = 0;
AIL_DOUBLE StrobeLevel = 128;

//Allocate defaults.
MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, 
                 &AilDigitizer, &AilImage);
//Set the grab trigger source to be an external signal.
MdigControl(AilDigitizer, M_GRAB_TRIGGER_SOURCE, M_AUX_IO8);

//Specify to trigger the grab on the falling edge of the grab trigger signal. 
//Note that this could be set to the default, which is the same as M_EDGE_RISING.
MdigControl(AilDigitizer, M_GRAB_TRIGGER_ACTIVATION, M_EDGE_FALLING);

//Setup the exposure settings (in nanoseconds)
MdigControl(AilDigitizer, M_EXPOSURE_TIME, ExposureTime); 
MdigControl(AilDigitizer, M_EXPOSURE_DELAY, ExposureTimeDelay); 

//Invert the timer output.
MdigControl(AilDigitizer, M_TIMER_OUTPUT_INVERTER+M_TIMER2, M_ENABLE);

//Sets the grab trigger source signal as the trigger source.
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE+M_TIMER2, M_GRAB_TRIGGER);

//Sets a delay before the active portion of the stobe (in nanoseconds).
MdigControl(AilDigitizer, M_TIMER_DELAY+M_TIMER2, StrobeTimeDelay);

//Sets the duration of the active portion of the strobe (in nanoseconds) during the exposure period.
//If set to M_INFINITE, the signal will always be ON.
MdigControl(AilDigitizer, M_TIMER_DURATION+M_TIMER2, StrobeLength);

//Enable timer 2 to output a pulse-train strobe signal and CCS output.
MdigControl(AilDigitizer, M_TIMER_STATE+M_TIMER2, M_ENABLE);

//Sets the intensity level of the strobe (0-255).
MdigControl(AilDigitizer, M_LIGHTING_BRIGHT_FIELD, StrobeLevel); 

//Enable the pulse-train strobe signal on M_AUX_IO4.
MsysControl(AilSystem, M_IO_SOURCE + M_AUX_IO4, M_TIMER2);

//Enable the grab trigger.
MdigControl(AilDigitizer, M_GRAB_TRIGGER_STATE, M_ENABLE);

//Grab continuously.
MdigGrabContinuous(AilDigitizer, AilImage);

//Wait.
MosGetch();

//Stop continuous grab.
MdigHalt(AilDigitizer);

//Disable the timer.
MdigControl(AilDigitizer, M_TIMER_STATE+M_TIMER2, M_DISABLE);

//Disable the grab trigger.
MdigControl(AilDigitizer, M_GRAB_TRIGGER_STATE, M_DISABLE);

//Free the defaults.
MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AilImage);
```
