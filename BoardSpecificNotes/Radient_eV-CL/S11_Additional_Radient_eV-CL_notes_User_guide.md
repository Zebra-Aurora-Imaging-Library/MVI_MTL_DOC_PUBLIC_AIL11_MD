---
doctype: BoardSpecificNotes
part: ""
chapter: Radient_eV-CL
section: Additional_Radient_eV-CL_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / radient_ev-cl / Additional Radient eV-CL notes User guide"
---

# Miscellaneous user guide information for Zebra Radient eV-CL

This section includes additional user guide information for Zebra Radient eV-CL. The information found in this section might be a reiteration of content previously documented.

## Supported features and standards

The Zebra Radient eV-CL supports firmware updates without power-cycling.

The Zebra Radient eV-CL supports RGB 10/12 bits Gain and Offset. For more information, see **M_SHADING_CORRECTION**.

The Zebra Radient eV-CL supports the following standards:

- Camera Link version 2.1.
- GenICam GenApi version 3.4.2.
- GenICam GenCP Camera Link Module version 1.3.
- GenICam for Camera Link (CLProtocol) version 1.2. Note that this requires a third-party CLProtocol communication library (DLL) which is supplied by the camera vendor.

## Particularities

The Bayer conversion hardware supports a line width of up to 8192 bytes.

The clock generator on Zebra Radient eV-CL SB is limited to a base clock frequency range from 9 to 97 MHz. Note that using certain base clock frequencies will affect timer accuracy. For additional information, contact your Zebra technical support representative.

*[Image: sc_Radient_eV-CL_clockgenerator.png]*

Data latches and shading correction are not supported on Zebra Radient eV-CL SB.

When using a JAI camera, the Camera Link Camera Control bits on the Zebra Radient eV-CL must be activated for stable communication with the camera (either with JAI or Zebra software). The bits can be activated using Aurora Imaging Intellicam in the **Other** tab of the **DCF** dialog.

*[Image: sc_Radient_eV-CL_JAIcamera.png]*

When editing a DCF for Camera Link using Aurora Imaging Intellicam, Feature Browser will close and reopen. Reopening a Feature Browser window might be subject to a noticeable delay due to camera initialization. To avoid this, uncheck "Automatically compute registers" in the **Preferences Register Compute** menu tab. However, you will then need to manually compute the DCF registers using the **DCF Compute Registers** menu tab.

## Code snippets

In the following code snippet, an encoder is used to trigger a grab when a certain position is reached. After 20 frames of 1000 lines (or 20000 lines) are grabbed, the grab moves slightly to the right and repeats the process in reverse.

```
/* The camera must be set to trigger the acquisition of a line on the rising edge of the timer output signal. */
         
/* Reset the decoder counter to 0. */
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION, 0);
         
/* Program the timer to trigger on a decoder signal. */
MdigControl(AilDigitizer, M_TIMER_CLOCK_SOURCE, M_SYSCLK);
MdigControl(AilDigitizer, M_TIMER_DELAY, timerDelayInUs);
MdigControl(AilDigitizer, M_TIMER_DURATION, timerDurationInUs);
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE, M_ROTARY_ENCODER);
MdigControl(AilDigitizer, M_TIMER_STATE, M_ENABLE);
         
/* Program the decoder to start triggering when position 1000 is reached then continue triggering at a multiple of '1'. */
MdigControl(AilDigitizer, M_ROTARY_ENCODER_OUTPUT_MODE, M_POSITION_TRIGGER_MULTIPLE);
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION_START_TRIGGER, 1000);
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION_TRIGGER, 1);
         
/* Start MdigProcess to grab a sequence (strip) of 20 frames of 1000 lines each for a total of 20000 lines (bufferingSize == 20). */
MdigProcess(AilDigitizer, AilGrabBufferList, bufferingSize, M_SEQUENCE, M_SYNCHRONOUS, ProcessingFunction, &UserHookData);
         
/* Once the end of sequence is reached, the stage will start scanning in the other direction (the decoder counter will decrement). */
/* Program the decoder to start triggering when counter value 21000 is reached then continue triggering at a multiple of '1'. */
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION_START_TRIGGER, 21000);
         
/*Start MdigProcess to grab a sequence of 20 frames of 1000 lines each for a total of 20000 lines (bufferingSize == 20). */
MdigProcess(AilDigitizer, AilGrabBufferList, bufferingSize, M_SEQUENCE, M_SYNCHRONOUS, ProcessingFunction, &UserHookData);
```

The LinearEncoderScan animation shown below illustrates this example.

*[Image: Radient_eV-CL_LinearEncoderScan]*

In the following code snippet, the board's decoder is set to trigger a pulse only when a linear stage is moving forward. The counter decrements when the stage moves in reverse. The grab is only triggered when the counter hits a new positive value; as such, no line that was already grabbed will be grabbed again.

```
/* The camera must be set to trigger the acquisition of a line on the rising edge of the timer output signal. */
         
/* Set the rotary decoder to send a trigger only when the stage is moving forward. */
MdigControl(AilDigitizer, M_ROTARY_ENCODER_DIRECTION, M_FORWARD); 
         
/* Specifies to set the counter to 0xFFFFFFFF upon a decrement. */
MdigControl(AilDigitizer, M_ROTARY_ENCODER_FORCE_VALUE_SOURCE, M_STEP_BACKWARD_WHILE_POSITIVE);
         
/* Specifies to output a pulse upon a rotary decoder counter increment, only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the increment occurs; when interpreting the counter value as signed, this would be the positive counter value range. */ 
MdigControl(AilDigitizer, M_ROTARY_ENCODER_OUTPUT_MODE, M_STEP_FORWARD_WHILE_POSITIVE); 
         
/* Set the rotary decoder counter to 0. */
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION_TRIGGER, 0);
         
/* Set the signal that the timer should output at every rotary decoder trigger. */ 
MdigControl(AilDigitizer, M_TIMER_DELAY, timerDelayInUs);
MdigControl(AilDigitizer, M_TIMER_DURATION, timerDelayInUs); 
         
/* Specifies to use the output (set with M_ROTARY_ENCODER_OUTPUT_MODE) of the default rotary decoder as the trigger source. */
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE, M_ROTARY_ENCODER); 
         
/* The trigger rate divider can be used to reduce the trigger rate. */
MdigControl(AilDigitizer, M_TIMER_TRIGGER_RATE_DIVIDER, 1); 
         
/* Enable the timer. */
MdigControl(AilDigitizer, M_TIMER_STATE, M_ENABLE); 
         
 /* Start the processing. The processing function is called with every frame grabbed. */
MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize, M_START, M_DEFAULT, ProcessingFunction, &UserHookData);
```
