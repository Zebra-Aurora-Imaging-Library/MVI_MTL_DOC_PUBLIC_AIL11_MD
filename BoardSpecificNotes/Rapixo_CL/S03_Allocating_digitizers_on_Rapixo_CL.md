---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Allocating_digitizers_on_Rapixo_CL
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Allocating digitizers on Rapixo CL"
---

# Allocating independent Aurora Imaging Library digitizers on Zebra Rapixo CL boards

Depending on the version of Zebra Rapixo CL, you can allocate up to 4 independent Aurora Imaging Library digitizers on the board, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). Independent digitizers have different device numbers and use different acquisition paths. The following is the number of available acquisition paths on each version of the board and the device number to use to allocate a path for the digitizer when calling [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).

- Zebra Rapixo CL DB has two acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) and [`M_DEV1`](../../Reference/dig/MdigAlloc.md)).
  Zebra Rapixo CL Pro DB has two acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) and [`M_DEV1`](../../Reference/dig/MdigAlloc.md)).
  Zebra Rapixo CL Pro QB has four acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) to [`M_DEV3`](../../Reference/dig/MdigAlloc.md)).
  With Zebra Rapixo CL and both Zebra Rapixo CL Pro DB and QB, each acquisition path supports a video source in the Camera Link Base configuration.
- Zebra Rapixo CL SF has only one acquisition path ([`M_DEV0`](../../Reference/dig/MdigAlloc.md)).
  Zebra Rapixo CL Pro SF has only one acquisition path ([`M_DEV0`](../../Reference/dig/MdigAlloc.md)).
  Zebra Rapixo CL Pro DF has two acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) and [`M_DEV1`](../../Reference/dig/MdigAlloc.md)).
  With Zebra Rapixo CL SF and both Zebra Rapixo CL Pro SF and DF, each acquisition path supports a video source in any Camera Link configuration.
