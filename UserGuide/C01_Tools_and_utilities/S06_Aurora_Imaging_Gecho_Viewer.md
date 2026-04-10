---
doctype: UserGuide
part: "Getting started"
chapter: Tools_and_utilities
section: Aurora_Imaging_Gecho_Viewer
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / Tools_utilities_and_general_information / Aurora Imaging Gecho Viewer"
---

# Aurora Imaging Gecho Viewer

Aurora Imaging Gecho Viewer is an event logging utility for Aurora Imaging Library applications that use a Zebra frame grabber (for example, Zebra Rapixo CXP). It allows you to log, visualize, and analyze hardware and software events that are seen by your frame grabber (board/system) during the execution of your Aurora Imaging Library application (for example, camera communication, auxiliary I/Os, timers, CXP triggers, on-board frame grabber events, and library application calls related to the frame grabber). It can generate RAW, CSV, or JSON files that you can use to track the detected event, such as errors, warnings, and other hardware related/generated messages (for example, unexpected camera removal, missed frames, and latency issues/user-hook latency).

Debugging machine vision systems that use frame grabbers in high-end inspection can be tedious, time consuming, and error prone. Aurora Imaging Gecho Viewer simplifies and streamlines this complex process, allowing you to see events from the perspective of your frame grabber so even the fastest and most complicated machine vision systems can be easily analyzed and debugged. Aurora Imaging Gecho Viewer runs Aurora Imaging Gecho in the background to log the real-time events with minimal overhead.

For the Zebra Rapixo CXP, Aurora Imaging Gecho Viewer includes example traces (logging samples) for the following events:

- Camera disconnects.
- Frames missed by hardware overtriggering.
- Frames missed by software latency.

You can access Aurora Imaging Gecho Viewer from the Tools folder installed with the library. For more information, refer to the Zebra website.
