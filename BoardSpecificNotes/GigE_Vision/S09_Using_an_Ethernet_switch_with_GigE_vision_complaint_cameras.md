---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Using_an_Ethernet_switch_with_GigE_vision_complaint_cameras
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Using an Ethernet switch with GigE vision complaint cameras"
---

# Using a Gbit Ethernet switch with GigE Vision-compliant cameras

If you need to connect to more cameras than you have network adapters (or network ports if using a multi-port network adapter) in your computer, a switch can provide the required expansion capabilities without increasing the complexity of your network (as would be the case with adding a router) while optimizing network throughput. When using a Gbit Ethernet switch, you must ensure that your Gbit Ethernet switch's performance metrics are adequate for your application's needs, particularly with regard to the bandwidth requirements of your applications. For more information, refer to your Gbit Ethernet switch's documentation for the switching capacity and packets per second capacity of your switch.

In addition, your Gbit Ethernet switch should support the following: jumbo packets, Ethernet (IEEE 802.3x) flow control, and port grouping (if using link-aggregated GigE Vision-compliant cameras through a switch).

Jumbo packets and Ethernet flow control are typically disabled by default and must be enabled if needed.

If port grouping (link aggregation) is required, it must be manually configured using the switch's configuration utility.

Finally, if you need to use IP multicast, make sure your switch supports the following: IGMP snooping and IGMP querying (unless your network has a multicast router to handle IGMP). IGMP snooping and IGMP querying are typically disabled by default and must be enabled if needed.
