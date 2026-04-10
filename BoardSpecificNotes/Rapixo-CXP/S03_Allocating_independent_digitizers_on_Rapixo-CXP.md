---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CXP
section: Allocating_independent_digitizers_on_Rapixo-CXP
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cxp / Allocating independent digitizers on Rapixo-CXP"
---

# Allocating independent Aurora Imaging Library digitizers on Zebra Rapixo CXP boards

You can allocate up to four independent Aurora Imaging Library digitizers on your Zebra Rapixo CXP, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). Independent digitizers have different device numbers and use different acquisition paths. You must allocate a digitizer for each CoaXPress link. A CoaXPress link contains all the connections and components to capture from one video source.

When connecting a video source with multiple connections to Zebra Rapixo CXP, the main connection determines which device number should be used when allocating your digitizer. For example, if the main connector is plugged into CXP 0, use [`M_DEV0`](../../Reference/dig/MdigAlloc.md) to allocate the digitizer. If, however, the main connector is plugged into CXP 3, use [`M_DEV3`](../../Reference/dig/MdigAlloc.md) to allocate the digitizer. The other connectors for the same video source (called extension connectors) combine with the main connection to form a CXP link.

Both of the following configurations depict two CXP links, each with a main connection and an extension connection.

*[Image: Rapixo_connect2_rev1.png]*

*[Image: Rapixo_connect_rev1.png]*

In configuration 1, you should allocate a digitizer for video source 1 using [`M_DEV0`](../../Reference/dig/MdigAlloc.md); whereas, you should allocate a digitizer for video source 2 using [`M_DEV2`](../../Reference/dig/MdigAlloc.md).

In configuration 2, you should allocate a digitizer for video source 1 using [`M_DEV0`](../../Reference/dig/MdigAlloc.md); whereas, you should allocate a digitizer for video source 2 using [`M_DEV1`](../../Reference/dig/MdigAlloc.md). Configuration 2 has a slight speed advantage to configuration 1 when the digitizer initializes, since the available devices will be scanned for initialization from 0 to 3.
