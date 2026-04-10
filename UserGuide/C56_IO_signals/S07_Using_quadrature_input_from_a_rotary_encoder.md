---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Using_quadrature_input_from_a_rotary_encoder
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Using quadrature input from a rotary encoder"
---

# Using quadrature input from a rotary encoder

Some Zebra products, typically frame grabbers, have a rotary decoder that can decode quadrature input (2-bit Gray code derived from two signals) from a rotary encoder. A rotary encoder is a device that provides information about the position and direction of a rotating shaft (for example, that of a conveyor belt). For each change in position of the rotating shaft, the rotary encoder also changes position (step); for each rotary encoder step, one of four possible Gray codes is transmitted (in a precise sequence) to the rotary decoder. Upon decoding a Gray code, the rotary decoder increments or decrements its internal counter depending on the direction (determined by the sequence of the Gray code). Since each rotary encoder step corresponds to a fixed linear displacement of an object on, for example, a conveyor belt moved by a rotating shaft, the position of the object can be calculated. In addition, the rotary decoder can output a signal (typically a pulse) upon a specific rotary decoder counter value and/or direction of movement, to trigger a grab or a timer.

Rotary encoders are used for multiple purposes; in imaging, they are typically used in conjunction with a line scan camera to grab images of objects on a conveyor belt according to the displacement of the objects and independent of speed of the conveyor belt. You can have the rotary decoder output a signal to trigger a timer to start exposing the line scan camera's CCD based on the counter value (a required displacement), as well as send the timer output to trigger the grab of the line.

The following illustration depicts the possible inputs and outputs related to the rotary decoder. Some or all of the inputs and outputs can be used (depending on your requirements and if your board supports them). The animation demonstrates the path of a signal once it is transmitted by the rotary encoder.

*[Image: I_O_Rotary_Loop]*

|  |
||
| 1 | [`M_ROTARY_ENCODER_BIT0_SOURCE`](../../Reference/dig/MdigControl.md) | 7 | [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md) |
| 2 | [`M_ROTARY_ENCODER_BIT1_SOURCE`](../../Reference/dig/MdigControl.md) | 8 | [`M_ROTARY_ENCODER_DIRECTION`](../../Reference/dig/MdigControl.md) |
| 3 | [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigControl.md) | 9 | [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) |
| 4 | [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/dig/MdigControl.md) | 10 | [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) |
| 5 | [`M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`](../../Reference/dig/MdigControl.md) | 11 | [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) |
| 6 | [`M_ROTARY_ENCODER_MULTIPLIER`](../../Reference/dig/MdigControl.md) | 12 | [`M_ROTARY_ENCODER_FRAME_END_READ`](../../Reference/dig/MdigControl.md) |

By default, each rotary decoder is associated with the acquisition path of the current digitizer; that is, rotary decoder _n_ is used on acquisition path _n-1_, where _n_ is hardware-specific (for example, on Zebra Rapixo CXP, it can be from 1 to 4). Some boards support using any rotary decoder on any acquisition path. If your board supports this type of interconnectivity, use the combination constant [`M_ROTARY_ENCODERn`](../../Reference/dig/MdigControl.md) to specify the required rotary decoder to use.

## Interpreting the direction of movement from the Gray code

The quadrature input (Gray code) is transmitted to the frame grabber using two input signals. To specify on which auxiliary input signals to receive bit 0 and bit 1 of the Gray code, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_BIT0_SOURCE`](../../Reference/dig/MdigControl.md) and [`M_ROTARY_ENCODER_BIT1_SOURCE`](../../Reference/dig/MdigControl.md), respectively. Note that for some boards, if you specify the auxiliary input signal for one of the bits, the corresponding auxiliary input signal will automatically be assigned for the other bit. For information on which auxiliary input signals support quadrature input, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product.

For each change in position of the rotating shaft monitored by a rotary encoder, the rotary encoder also changes position (step); for each rotary encoder step, one of the following four possible Gray codes is transmitted in a precise sequence to the rotary decoder. If the rotating shaft changes direction, the rotary encoder transmits the Gray code in the reverse sequence. The following animation shows how gray code affects the counter when output mode is set to FORWARD_ONLY.

*[Image: I_O_Rotary_Increment]*

Each rotary encoder step (and consequently, each Gray code transmitted), increments or decrements the rotary decoder's counter. Incrementing the counter corresponds to an object on a conveyor belt moving in the forward direction; therefore, decrementing the counter corresponds to an object on a conveyor belt moving in the backwards direction. You must specify the direction of movement (consequently, whether to increment or decrement the counter) occurring when the Gray code sequence is 00 - 01 - 11 - 10, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_DIRECTION`](../../Reference/dig/MdigControl.md) set to either [`M_FORWARD`](../../Reference/dig/MdigControl.md) or [`M_BACKWARD`](../../Reference/dig/MdigControl.md), as per the following table:

| Set [`M_ROTARY_ENCODER_DIRECTION`](../../Reference/dig/MdigControl.md) to | If Gray code sequence is | And the conveyor belt is moving | So that the rotary decoder's counter |
| --- | --- | --- | --- |
| [`M_FORWARD`](../../Reference/dig/MdigControl.md) | 00 - 01 - 11 - 10 | Forward | Increments > **Note:** With Zebra Radient eV-CL, decrements. |
| Consequently | 00 - 10 - 11 - 01 | Backward | Decrements > **Note:** With Zebra Radient eV-CL, increments. |
| [`M_BACKWARD`](../../Reference/dig/MdigControl.md) | 00 - 01 - 11 - 10 | Backward | Decrements > **Note:** With Zebra Radient eV-CL, increments. |
| Consequently | 00 - 10 - 11 - 01 | Forward | Increments > **Note:** With Zebra Radient eV-CL, decrements. |

## Using the rotary decoder's counter

You can use the rotary decoder's counter to calculate the position, length, or displacement of an object. This calculation is done based on the fact that each rotary step increments (or decrements, depending on the direction) the counter, and that each step corresponds to a fixed linear displacement, determined by the physical setup. To calculate this type of information, you might need to mark a certain position or event with a known counter value, (for example, when you start scanning an item on a conveyor belt). You can reset the counter to zero or force it to 0xFFFFFFFF upon the rising edge of a signal (such as that of an auxiliary input signal). To do so, specify the signal using [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/dig/MdigControl.md) or [`M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`](../../Reference/dig/MdigControl.md), respectively. Also, if at any time you want to reset the counter to 0, you can use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigControl.md). Note that you can inquire the current value of the counter using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigInquire.md).

You can have the rotary decoder internally generate a trigger upon a specific counter value using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). You can then use this trigger in the following ways:

- Output the trigger to a timer; refer to [Using the rotary decoder's output to trigger a timer or a grab](S07_Using_quadrature_input_from_a_rotary_encoder.md).
- Output the trigger to the grab controller; refer to [Using the rotary decoder's output to trigger a timer or a grab](S07_Using_quadrature_input_from_a_rotary_encoder.md).
- Use the trigger to decimate (subsample) the rotary decoder's output; refer to [Pixel aspect ratio](S07_Using_quadrature_input_from_a_rotary_encoder.md).
- Hook a function to the trigger by calling [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_ROTARY_ENCODER`](../../Reference/dig/MdigHookFunction.md).

Note that, you can inquire the counter value at the end of the last grab using [`MdigInquire`](../../Reference/dig/MdigInquire.md)with [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigInquire.md). You can also retrieve the counter value at the end of a frame within the scope of a function hooked to a grab end or grab frame end event, using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigGetHookInfo.md). In either case, you must first set the rotary decoder to store this information using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_FRAME_END_READ`](../../Reference/dig/MdigControl.md).

## Using the rotary decoder's output to trigger a timer or a grab

As mentioned earlier, the rotary decoder can output a signal (typically a pulse) upon a specific rotary decoder counter value and/or direction of movement. If your frame grabber has a rotary decoder and supports timers, you can use the output of the rotary decoder to trigger a timer using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md) or [`M_ROTARY_ENCODERn`](../../Reference/dig/MdigControl.md). Similarly, you can send the rotary decoder output (independently or simultaneously) to the grab controller to trigger an image or line grab, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to either [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md) or [`M_ROTARY_ENCODERn`](../../Reference/dig/MdigControl.md).

You can specify upon which counter value and/or direction of movement to output a pulse, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md). For example, the animation below depicts how 3 different output modes affect the images grabbed of items on a conveyor belt as it moves back and forth. In all 3 cases, the rotary decoder's output pulse is used to trigger a timer to start the exposure of a line scan camera's CCD, and start the grab of the line.

*[Image: I_O_Backwards_Movement]*

The setup for case 3 is typically used to prevent grabbing unwanted or duplicate lines when an unplanned rotary displacement in the reverse direction occurs. The following steps must be included in the set-up of the rotary decoder in case 3:

1. Before starting to grab the first frame, reset the rotary decoder's counter to 0, using [`MdigControl`](../../Reference/dig/MdigControl.md) with either [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/dig/MdigControl.md) or [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigInquire.md).
2. Specify to force the counter to -1 upon a reverse displacement (counter decrements), and only if the counter value was positive before the reverse displacement occurred, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_STEP_BACKWARD_WHILE_POSITIVE`](../../Reference/dig/MdigControl.md).
3. As originally mentioned, specify to have the rotary decoder output a pulse upon a forward displacement (counter increments), and only if the counter value was positive before the forward displacement occurred, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) set to [`M_STEP_FORWARD_WHILE_POSITIVE`](../../Reference/dig/MdigControl.md).

The following table shows an example of what happens to the rotary decoder's counter value and which lines of an image are grabbed of an object on a conveyor belt, if it is setup as described for case 3.

| Camera scanning | Rotary decoder's counter value: | [`M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`](../../Reference/dig/MdigControl.md)is set to [`M_STEP_BACKWARD_WHILE_POSITIVE`](../../Reference/dig/MdigControl.md) Force counter to -1? (Encoder reversed direction and counter positive before reversed) | [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md)is set to [`M_STEP_FORWARD_WHILE_POSITIVE`](../../Reference/dig/MdigControl.md) Rotary decoder outputs a pulse? (Counter incremented and was positive before increment) | Line grabbed? |
| --- | --- | --- | --- | --- |
| Line 0 | 0 | ... | ... | No |
| Line 1 | 1 | No | Yes | Yes |
| ... | ... | ... | ... | ... |
| Line 72 | 72 | No | Yes | Yes |
| Line 73 | 73 | No | Yes | Yes |
| Line 74 | 74 | No | Yes | Yes |
| Line 75 | 75 | No | Yes | Yes |
| Line 74 | -1 | Yes | No | No |
| Line 73 | -2 | No | No | No |
| Line 72 | -3 | No | No | No |
| Line 73 | -2 | No | No | No |
| Line 74 | -1 | No | No | No |
| Line 75 | 0 | No | No | No |
| Line 76 | 1 | No | Yes | Yes |
| Line 77 | 2 | No | Yes | Yes |
| ... | ... | ... | ... | ... |

In the example above, the counter is reset to 0 before the camera scans the first line of the image. Image lines are grabbed when the rotary decoder outputs a pulse, which only happens if the counter increments and the counter is a positive value before the increment occurs. After the camera scans line 75, the conveyor belt takes a step in the reverse direction, while the counter is a positive value. Normally, the counter would have decremented by 1; however, since [`M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`](../../Reference/dig/MdigControl.md)is set to [`M_STEP_BACKWARD_WHILE_POSITIVE`](../../Reference/dig/MdigControl.md), the counter is forced to -1. The counter now reads a negative value and so when the conveyor belt continues to move in the reverse direction for another 2 steps, the counter decrements by 2, but the rotary decoder does not output a pulse; therefore, lines are not grabbed for any of these steps. After the camera scans line 72 (for the second time), the conveyor belt resumes moving in its original direction. When the camera scans line 75 (for the second time), it is not grabbed, since the counter value was negative before the counter incremented to 75. Line 76 is the first line to be grabbed after the conveyor belt reversed direction.

## Pixel aspect ratio

As mentionned earlier, each rotary step corresponds to a fixed displacement. For example, there is a fixed displacement of an object on a conveyor belt each time the rotary encoder changes position. If you want to grab an image of the object on the conveyor belt with a line scan camera, it is important to set up the equipment to minimize distortion. To maintain a 1:1 pixel aspect ratio for each lined grabbed, the distance that each pixel represents along the horizontal direction of the scanned line should be the same as the displacement that occurs per step (which is equivalent to the distance between lines scanned).

The animation below shows a common conveyor belt setup using a line scan camera and the effects of rotary speed and displacement on a grabbed image.

*[Image: RotaryEncoder]*

If you are not able to control the displacement per rotary step or the position of the camera relative to the object, you can set up the rotary decoder to output a signal to trigger a new line scan and grab once a certain displacement has occurred. Case 1 shows an image of the object with the ideal 1:1 pixel aspect ratio. In case 2, the width (_x_) of the pixel is smaller than its height (_y_), which means that to compensate for the larger displacement, the rotary decoder needs to output more pulses per rotary step to scan lines at a higher frequency. If available for your board, the rotary decoder can output multiple pulses per step, when [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_STEP_...`](../../Reference/dig/MdigControl.md) control values, and where the multiplying factor is specified using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_MULTIPLIER`](../../Reference/dig/MdigControl.md). Note that when applying the multiplying factor, the rotary decoder's counter increments by the same factor for each step. For example, if the multiplying factor is 4 and the counter value is 6, when the next step occurs, the counter will increment through to 10, instead of 7. In case 3, the width (_x_) of the pixel is larger than its height (_y_), which means that to compensate for the smaller displacement, the rotary decoder needs to output fewer pulses than there are rotary steps. If available for your board, you can decimate (subsample) the signal that is output from the rotary decoder before sending it to a timer or grab controller. To do so, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md) set to the decimation value and reset the counter each time it reaches this value.

The following animation shows the effect of the multiplier on a rotary encoder.

*[Image: I_O_Rotary_Multiplier]*

In the following code snippet, a rotary encoder is used as a trigger source for an on-board timer; the timer output specifies when to scan a new line and sets the line scan camera's exposure time to 1 msec. The decimation is 10; that is, the rotary decoder only outputs a pulse once every 10 increments (rotary steps in the forward direction); so, the timer is only triggered once every 10 increments.

> **Code example:** [userguide.io_signals.using_quadrature_input_from_a_rotary_encoder01](userguide.io_signals.using_quadrature_input_from_a_rotary_encoder01)

The following timing diagram illustrates a rotary decoder that outputs a pulse for every 5 position changes of the rotary encoder (decimation = 5). Each rotary decoder output pulse triggers the timer to start exposing the line scan camera's CCD. The timer output also triggers the grab controller to grab a line.

*[Image: I_O_rotary_timing.png]*

The following timing diagram also illustrates when a line is exposed and grabbed if a decimation value of 5 is applied. However, in this example, the speed of the connected rotary device is now decelerating, indicated by the increased spacing (time lapse) between each step. When comparing this timing diagram with the one above, both rotary decoders output a pulse at the same step (position), illustrating that the speed of the rotary device does not effect the line grabbed when using a rotary encoder.

*[Image: I_O_rotary_timing_rate.png]*
