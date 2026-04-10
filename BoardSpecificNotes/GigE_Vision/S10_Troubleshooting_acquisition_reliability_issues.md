---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Troubleshooting_acquisition_reliability_issues
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Troubleshooting acquisition reliability issues"
---

# Troubleshooting acquisition reliability issues

A number of factors can cause acquisition reliability issues. You can use the Aurora Imaging Capture Works utility to determine if you are having reliability issues. In this utility, select a camera and start the acquisition. Then, select the **Acquisition Statistics** tab. This tab will show whether any acquired frames (images) are corrupt as a result of missing packets. The bottom of the **Acquisition Statistics** page contains the last frame that reported an error, along with error codes that can help in diagnosing the issue.

The following is a list of the common causes of corrupted frames:

- Your Gigabit Ethernet network adapter is not well-suited to your GigE Vision camera's bandwidth requirements. Use a high-bandwidth Network Interface Card (or a Zebra GevIQ frame grabber).
- Your network adapter settings have not been adjusted for GigE Vision acquisition. You need to adjust your adapter with recommended settings.
- A network device between your GigE Vision camera and your network adapter, such as an Ethernet switch, is dropping packets. See the Using an Ethernet switch with GigE Vision cameras section.
- The total bandwidth of all GigE Vision cameras streaming image data to the same network adapter exceeds its link speed.
- An insufficient inter-packet delay.
- Your camera does not have the packet resend capability.
- Your Ethernet cables are not rated Category 5e (Cat 5e) or better.
