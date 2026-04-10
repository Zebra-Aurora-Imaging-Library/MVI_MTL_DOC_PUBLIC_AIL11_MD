---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Link_aggregation
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Link aggregation"
---

# Link aggregation

Some GigE Vision cameras have more than one network interface because the total bandwidth required exceeds 1 Gbit/sec. In this case, they transmit their images over both interfaces and expect the receiving computer to have two network adapters that are configured for link aggregation to receive the video stream. Link aggregation is a method of combining multiple network connections in parallel, to increase the throughput beyond what a single connection can sustain. Note that, while some cameras automatically support link aggregation (determined by the number of connections made from the camera to the network), the settings of your Gigabit Ethernet network adapter must be updated to allow link aggregation.

The Zebra GigE Vision driver deduces that a GigE Vision camera is transmitting across multiple network interfaces, and that the network adapter is using link-aggregation, when the reported link speed is greater than 1 Gbit/sec but less than 10 Gbit/sec, excluding link speeds of 2.5 and 5 Gbit/sec. Detecting this type of connection between the GigE Vision camera and network adapter allows the Zebra GigE Vision driver to automatically adjust its packet resend engine to fit the requirements of the connection between your camera and network adapter, since they tend to stream packets out of order.

Link aggregation has been incorporated into version 2.0 of the GigE Vision specification; this has formalized its use with GigE Vision cameras and Gbit Ethernet network adapters.
