---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Additional_industrial_communication_notes_PLC_ladder_logic
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Additional industrial communication notes PLC ladder logic"
---

# Miscellaneous user guide information for Industrial Communication: PLC ladder logic

This section includes additional user guide information for the Industrial Communication examples. The information found in this section might be a reiteration of content previously documented.

Specific Aurora Imaging Library examples are provided to demonstrate communication with an automation controller (that is, a PLC) using the Mcom module. A PLC needs to be programmed in a specific way for the examples to run properly. The following section explains how to program a PLC using ladder logic to work with the relevant examples.

1. The ladder logic initially generates a trigger pulse with a duration of 1 second.
   *[Image: comm_ladder_logic1.png]*
2. When the trigger pulse is generated, the ladder logic sets the trigger bit and resets the ready ACK-bit on the worker device.
   *[Image: comm_ladder_logic2.png]*
3. When the ladder logic receives the trigger ACK-bit from the worker device, it resets the trigger bit.
   *[Image: comm_ladder_logic3.png]*
4. Finally, when the ladder logic receives the ready bit from the worker device, it moves the result from the input to the output, and it sets the ready ACK-bit.
   *[Image: comm_ladder_logic4.png]*

The following table summarizes the sequence produced by the ladder logic (PLC):

| Output (Set by the PLC) | T1 | T2 | T3 | T4 | T5 | T6 |
| --- | --- | --- | --- | --- | --- | --- |
| Trigger (1-bit) | 1 | 1 | 0 | 0 | 0 | 0 |
| Ready ACK (1-bit) | 0 | 0 | 0 | 0 | 1 | 1 |
| Result copy (8-bit) | 0 | 0 | 0 | 0 | 22 | 22 |

The following table summarizes the sequence produced by the Aurora Imaging Library examples:

| Input (Set by the example) | T1 | T2 | T3 | T4 | T5 | T6 |
| --- | --- | --- | --- | --- | --- | --- |
| Trigger ACK (1-bit) | 0 | 1 | 1 | 0 | 0 | 0 |
| Ready (1-bit) | 0 | 0 | 0 | 1 | 1 | 0 |
| Result (8-bit) | 0 | 0 | 0 | 22 | 22 | 22 |
