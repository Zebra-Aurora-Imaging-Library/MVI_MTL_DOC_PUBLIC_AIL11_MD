---
doctype: BoardSpecificNotes
part: ""
chapter: 4Sight
section: Additional_4Sight_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / 4sight / Additional 4Sight notes User guide"
---

# Miscellaneous user guide information for Zebra 4Sight

This section includes additional user guide information for Zebra 4Sight. The information found in this section might be a reiteration of content previously documented.

In general, related Zebra auxiliary FPGA supports RS485 half-duplex auto mode and the maximum output toggle rate is 60 KHz.

## Licensing

When activating a temporary license or registering the software license for Aurora Imaging Configurator, a firmware update might be launched, or there could be issues when writing the license to the system's EEPROM. To prevent these problems, first close Aurora Imaging Configurator. Then, manually start the firmware updater tool, located in the installed Tools folder (for example, _C:\Program Files\Aurora Imaging Library\11\Tools_). After the update is complete, you can re-launch Aurora Imaging Configurator.

Zebra 4Sight XV6/XV7 supports Aurora Imaging Library licensing fingerprint storage on the disk (for example, on the SSD). Re-initializing the disk will remove the license, however, the license can be recovered and reapplied. Contact your Zebra representative for more information.
