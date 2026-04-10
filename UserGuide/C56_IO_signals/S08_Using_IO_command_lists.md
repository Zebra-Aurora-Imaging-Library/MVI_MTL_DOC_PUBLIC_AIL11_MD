---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Using_IO_command_lists
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Using IO command lists"
---

# Using I/O command lists

Some Zebra hardware products have I/O command lists (for example, Zebra 4Sight EV7). I/O command lists allow you to schedule changing the state of a bit of an I/O command register at a specified time or counter value. You can route the state of the bit, for example, to an auxiliary output signal to control a connected device at a required moment; the state of the bit can also be used to trigger some other internal device (for example, a timer). An I/O command list allows you to schedule I/O commands in any order. You can allocate and control an I/O command list using the [`MsysIo...`](../../Reference/sysIo/MsysIoAlloc.md) functions.

## Steps to use an I/O command list

The following steps provide a basic methodology for using an I/O command list on a Zebra hardware product:

1. Allocate an I/O command list using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_IO_COMMAND_LIST`](../../Reference/sysIo/MsysIoAlloc.md). You can allocate as many command lists as are available on your hardware product. When allocating an I/O command list, you must specify a signal as the counter source that will be used to schedule commands. The counter source can be either a clock, which allows you to schedule commands in time, or a specified signal, which allows you to schedule commands based on the number of transitions that occur on the signal. The latter case allows you to schedule commands, for example, based on the positional information provided by the output signal of your hardware product's rotary decoder (that is, distance).
2. If necessary, route the state of the bits of the I/O command list's register to auxiliary output signals using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_SOURCE`](../../Reference/sys/MsysControl.md). This register is referred to as an I/O command register.
3. If necessary, enable the use of one of the I/O command list's latches using [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) with the [`M_REFERENCE_LATCH_...`](../../Reference/sysIo/MsysIoControl.md) constants. Latches are used to store a timestamp or counter value upon the specified transition of a specified signal. You can then use this timestamp or counter value to schedule commands relative to this event.
4. Add commands to the I/O command list using [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md). Commands will change the state of a bit of the I/O command register at a specified time or counter value. Multiple I/O command register bits can be changed at a single moment. Commands can be added in any order.
5. Free your I/O command list using [`MsysIoFree`](../../Reference/sysIo/MsysIoFree.md), unless [`M_UNIQUE_ID`](../../Reference/sysIo/MsysIoAlloc.md) was specified during allocation.

## Scheduling I/O commands

The commands in the list are executed at a specified moment. To schedule commands, the I/O command list uses an internal counter to count clock ticks or transitions that occur on a signal specified as the counter source. The counter source is specified when you allocate the I/O command list using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md). The counter source can be a clock, an auxiliary signal, or the output signal of a rotary decoder.

You schedule commands by specifying a delay from a specific reference moment. For the reference moment, you can inquire the counter value at a specific moment through software using [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_REFERENCE_VALUE`](../../Reference/sysIo/MsysIoInquire.md). Alternatively, you can use an I/O command list's latch to store the counter value at the moment of a hardware event; for more information, see [Using an I/O command list latch](S08_Using_IO_command_lists.md).

If the I/O command list was allocated using a clock as the counter source (using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_CLOCK`](../../Reference/sysIo/MsysIoAlloc.md)), the internal counter counts the clock ticks and you schedule commands a certain amount of time, in seconds, after the reference moment. For example, you can schedule the command to execute 10 msec (0.01 sec) after a previously inquired reference moment.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm01](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm01)

If the I/O command list was allocated using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_AUX_IOn`](../../Reference/sysIo/MsysIoAlloc.md) or [`M_ROTARY_ENCODERn`](../../Reference/sysIo/MsysIoAlloc.md) as the counter source, the internal counter counts the number of signal transitions on the signal (by default, low-to-high signal transitions). For example, if you are using a rotary decoder's output signal as your counter source, you would schedule your commands based on the positional information provided by the signal (that is, distance). If your rotary decoder is set up to output a pulse each time a conveyor belt moves forward a step ([`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/sys/MsysControl.md) set to [`M_STEP_FORWARD`](../../Reference/sys/MsysControl.md)), you can, for example, add a command that should be executed a constant number of forward steps after an object on the conveyor belt passes in front of a sensor. To set up a rotary decoder so that it outputs a pulse after a specific distance or when moving forward new steps (ignoring forward steps that occurred to return to an original position after an unplanned rotary displacement in the reverse direction occurs), see [Using quadrature input from a rotary encoder](S07_Using_quadrature_input_from_a_rotary_encoder.md).

If a command has been added to the I/O command list to modify a specific bit of the I/O command register, and you attempt to add a different command to operate on the same bit at the same moment, the original entry will be overwritten. However, multiple commands can be added to execute at the same moment, as long as the commands are affecting different I/O command register bits.

## Commands that can be added to the I/O command list

Using the [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md) function, you can add the following commands to an I/O command list:

- Cause a rising edge ([`M_EDGE_RISING`](../../Reference/sysIo/MsysIoCommandRegister.md)). Changes the specified bit such that the associated signal will transition from low to high, if it is low.
- Cause a falling edge ([`M_EDGE_FALLING`](../../Reference/sysIo/MsysIoCommandRegister.md)). Changes the specified bit such that the associated signal will transition from high to low, if it is high.
- Cause an active high pulse ([`M_PULSE_HIGH`](../../Reference/sysIo/MsysIoCommandRegister.md)). This command actually adds two commands to the list: an [`M_EDGE_RISING`](../../Reference/sysIo/MsysIoCommandRegister.md) followed by an [`M_EDGE_FALLING`](../../Reference/sysIo/MsysIoCommandRegister.md) command a specified duration afterwards.
- Cause an active low pulse ([`M_PULSE_LOW`](../../Reference/sysIo/MsysIoCommandRegister.md)). This command actually adds two commands to the list: an [`M_EDGE_FALLING`](../../Reference/sysIo/MsysIoCommandRegister.md) followed by an [`M_EDGE_RISING`](../../Reference/sysIo/MsysIoCommandRegister.md) command a specified duration afterwards.
- Cause a very short active high pulse ([`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md)). Unlike [`M_PULSE_HIGH`](../../Reference/sysIo/MsysIoCommandRegister.md), a single command producing the shortest possible pulse is added to the list. This operation should not be output to external devices since it might be filtered out as noise. It can, however, be used to trigger internal hardware devices, such as timers.

When adding a command to change an I/O command register bit such that a pulse is generated on the associated signal, two commands are actually added; one command to transition the signal at the specified moment and one command to transition the signal back to its original state after the specified pulse duration. Note that if you want to specify the duration of the pulse in seconds, but the counter source is not a clock (that is, the [`CounterSrc`](../../Reference/sysIo/MsysIoAlloc.md) parameter of [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) was set to anything other than [`M_CLOCK`](../../Reference/sysIo/MsysIoAlloc.md)), use a timer to generate the pulse and the I/O command list to trigger the timer to generate the pulse; in this case, schedule an [`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md) command and use the affected I/O command register bit as the trigger source of a timer.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm02](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm02)

If you always want the command to change an I/O command register bit, use the + [`M_AUTO_REGISTER`](../../Reference/sysIo/MsysIoCommandRegister.md). Rather than waiting on a specified condition, the I/O command register bit is changed every time the latch is triggered. Note that, [`M_AUTO_REGISTER`](../../Reference/sysIo/MsysIoCommandRegister.md) must be used with[`M_LATCHn`](../../Reference/sysIo/MsysIoCommandRegister.md).

## Using an I/O command list latch

Each I/O command list on your Zebra hardware product has at least two latches that can be used to store the value of the command list's internal counter in hardware, upon a signal transition of a specified input signal. Using the latch, you can obtain the counter value at a particular moment; this is more precise than inquiring it in software using [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_REFERENCE_VALUE`](../../Reference/sysIo/MsysIoInquire.md). The counter value is stored in the latch until another signal transition occurs, which will overwrite the contents of the latch. To retrieve the latch's content, use [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_REFERENCE_LATCH_VALUE`](../../Reference/sysIo/MsysIoInquire.md) or, if within the scope of a system hook-handler function, use [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with [`M_REFERENCE_LATCH_VALUE`](../../Reference/sys/MsysGetHookInfo.md).

*[Image: IO_command_list_latch.png]*

To enable the use of a latch, use [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) with [`M_REFERENCE_LATCH_STATE`](../../Reference/sysIo/MsysIoControl.md). To change the input signal which triggers storing the counter value, use [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) with [`M_REFERENCE_LATCH_TRIGGER_SOURCE`](../../Reference/sysIo/MsysIoControl.md). To change the signal transition upon which to store the counter value in the latch, use [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) with [`M_REFERENCE_LATCH_ACTIVATION`](../../Reference/sysIo/MsysIoControl.md).

As mentioned, if another signal transition occurs on the specified input signal, the contents of the latch will be overwritten with the counter value of that moment. To store the latch's contents in software (for example, into a queue) before it is overwritten, you can allow the specified input signal to generate an interrupt and hook a function to this event which retrieves the counter value stored by the latch. To enable interrupt generation on a signal, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_INTERRUPT_STATE`](../../Reference/sys/MsysControl.md). To attach a hook-handler function to an I/O interrupt event, use [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_IO_CHANGE`](../../Reference/sys/MsysHookFunction.md). To retrieve the counter value stored by the latch in the hook-handler function, use [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with [`M_REFERENCE_LATCH_VALUE`](../../Reference/sys/MsysGetHookInfo.md).

## Examples

The I/O command list can be used for many applications. The following sub-sections present 4 possible applications:

- Always grabbing an image when the conveyor belt is at a given position.
- Parts traveling along a conveyor belt that are fixed in position.
- Different parts traveling along a conveyor belt that are ejected by different devices.
- Parts traveling along a conveyor belt that are not fixed in position.

### Always grabbing an image when the conveyor belt is at a given position

In this example, parts are moving along a conveyor belt but are fixed in their position on the conveyor belt. The parts are always taking 100 rotary encoder steps forward before reaching a camera, which will take a picture of the conveyor belt beneath it. For simplicity, the conveyor belt can only move forward and the camera is an areascan camera. A sensor detects each time a part is on the conveyor belt, and a rotary encoder is used to tell the camera when the part is best placed to take the picture. The camera is triggered by a rising pulse.

*[Image: IO_command_list_autoregister.png]*

An I/O command list is used to cause the camera's trigger to activate. The rotary decoder output is used as the counter source of the I/O command list so that scheduling of commands is based on a specified number of rotary encoder steps (distance that the part travels). A latch stores the value of the command list's internal counter automatically after 100 rotary encoder steps. Once the latch fires, the camera trigger's pulse rises, and a grab occurs. When the grab is complete, some processing is performed. This process repeats for each part passing the sensor. By using [`M_AUTO_REGISTER`](../../Reference/sysIo/MsysIoCommandRegister.md) with a registered command, you guarantee that every time the latch fires, a grab will occur.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401)

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402)

Where the processing function is:

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403)

### Parts traveling along a conveyor belt that are fixed in position example

In this example, parts are moving along a conveyor belt but are fixed in their position on the conveyor belt. The parts are always taking 500 rotary encoder steps forward before reaching an ejector, which will discard the part if processing fails. For simplicity, the conveyor belt can only move forward and the camera is a frame-scan camera. A sensor detects each time a part is under the camera's field of view; the sensor's signal is used to trigger the grab. The ejector requires a pulse of 10 msec (0.01 sec) to successfully discard a part.

*[Image: IO_command_list_examples_case1.png]*

An I/O command list is used to schedule the ejection of a part if it fails processing. The rotary decoder output is used as the counter source of the I/O command list so that scheduling of commands is based on rotary encoder steps (distance that the part travels). A latch stores the value of the command list's internal counter upon the trigger signal from the sensor; the hooked function retrieves the counter value stored in the latch and adds it to a queue. When the grab is complete, some processing is performed and a decision is made to discard or keep the part. If the part must be discarded, a command is added to discard the part 500 rotary encoder steps after the grab started. The ejector requires a pulse of 10 msec; however, if you use [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md) with [`M_PULSE_HIGH`](../../Reference/sysIo/MsysIoCommandRegister.md) to add a pulse command, you would need to specify the duration of the pulse in rotary decoder output signal transitions (in distance instead of time), since the rotary decoder output signal is the counter source. To specify the pulse duration in time so that it is the correct width, an [`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md) command is added instead and a timer is also used; the [`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md) command affects an I/O command register bit that triggers the timer that outputs a signal with an appropriate pulse.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case101](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case101)

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case102](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case102)

Where the function hooked to the I/O interrupt event and the processing function are:

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case103](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case103)

### Different parts traveling along a conveyor belt that are ejected by different devices example

In this example, three different parts are moving along a conveyor belt and each different part needs to be redirected to a different path. Three ejectors redirect the different parts to the correct path. Each ejector is a known distance away from a sensor that triggers the grab. Parts of type "A" are redirected by ejector A, which is 800 rotary encoder steps away from the sensor; parts of type "B" are redirected by ejector B, which is 1000 rotary encoder steps away from the sensor. Parts of type "C" are redirected by ejector C, which is 1200 rotary encoder steps away from the sensor. All three ejectors require a pulse of 10 msec (0.01 sec) to successfully discard a part.

*[Image: IO_command_list_examples_case3.png]*

Again, an I/O command list is used to schedule the ejection of a part, and the rotary decoder output is used as the counter source of the I/O command list. A latch is used to store the command list's internal counter value upon the trigger signal from the sensor; the hooked function retrieves the counter value stored in the latch and adds it to a queue. When the grab is complete, some processing is performed and the part's type is determined. If the part is of type A, a command is added to discard the part 800 rotary encoder steps after the grab started; if the part is of type B, a command is added to discard the part 1000 rotary encoder steps after the grab started. If the part is of type C, a command is added to discard the part 1200 rotary encoder steps after the grab started. Since the rotary decoder output signal is the counter source, timers are again used. To specify the pulse duration in time (10 msec), an [`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md) command is added to the command list to affect an I/O command register bit that triggers a timer that outputs a signal with an appropriate pulse.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case301](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case301)

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case302](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case302)

Where the function hooked to the I/O interrupt event (which queues the latch) is the same as the previous example and the processing function is:

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case303](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case303)

### Parts traveling along a conveyor belt that are not fixed in position example

In this example, parts are moving along a conveyor belt, shifting in position as they move along after they are grabbed, but not moving ahead of any other part on the conveyor. The parts take a variable number of steps before reaching the ejector, which will discard the part if processing fails. Two sensors are placed along the conveyor belt. Sensor A is placed where the part is completely in the camera's field of view; while, sensor B is placed near the ejector such that when the part is no longer detected, it is completely in front of the ejector. The ejector requires a pulse of 10 msec (0.01 sec) to successfully discard a part.

*[Image: IO_command_list_examples_case2.png]*

As in the previous examples, an I/O command list (I/O command list B) is used to schedule the ejection of a part; in this case, the signal from sensor B is used as the counter source of the I/O command list so that commands can be scheduled based on the number of parts that have passed sensor B. Another I/O command list (I/O command list A) is used to get a count of the number of parts that have triggered sensor A when a new part is detected; in this case, the signal from sensor A is used as both the counter source and the trigger that latches the internal counter's value. The grab is triggered and a hooked function is called each time there is a trigger signal from sensor A. The hooked function retrieves and queues the number of parts that have passed by sensor A (by inquiring the latched value of I/O command list A) and the number of parts that have passed by sensor B (by inquiring the latched value of I/O command list B) at this moment. When the grab is complete, some processing is performed and a decision is made to discard or keep the part. If the part must be discarded, a command is added to I/O command list B to discard the part after the number of parts between sensor A and sensor B have passed by sensor B since the part in question was grabbed. Since the signal from sensor B is the counter source of I/O command list B but the duration of the ejector pulse must be specified in time (10 msec), an [`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md) command is added to the command list to affect an I/O command register bit that triggers a timer that outputs a signal with an appropriate pulse.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case201](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case201)

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case202](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case202)

Where the function hooked to the I/O interrupt event and the processing function are:

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case203](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case203)
