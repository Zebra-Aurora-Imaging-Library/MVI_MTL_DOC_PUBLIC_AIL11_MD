---
doctype: BoardSpecificNotes
part: ""
chapter: Radient_eV-CL
section: Allocating_independent_digitizers_on_Radient_eV-CL
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / radient_ev-cl / Allocating independent digitizers on Radient eV-CL"
---

# Allocating independent Aurora Imaging Library digitizers on Zebra Radient eV-CL

You can allocate up to 4 independent Aurora Imaging Library digitizers on your Zebra Radient eV-CL, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). Independent digitizers have different device numbers and use different acquisition paths.

## Zebra Radient eV-CL

The number of independent digitizers that you can allocate on Zebra Radient eV-CL is dependent on the version in use:

- Zebra Radient eV-CL SB has only 1 acquisition path ([`M_DEV0`](../../Reference/dig/MdigAlloc.md)).
  Zebra Radient eV-CL DB has 2 acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) and [`M_DEV1`](../../Reference/dig/MdigAlloc.md)).
  Zebra Radient eV-CL QB has 4 acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) to [`M_DEV3`](../../Reference/dig/MdigAlloc.md)).
  With both Zebra Radient eV-CL DB and QB, each acquisition path supports a video source in the Camera Link Base configuration.
- Zebra Radient eV-CL SF has only 1 acquisition path ([`M_DEV0`](../../Reference/dig/MdigAlloc.md)).
  Zebra Radient eV-CL DF has 2 acquisition paths ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) to [`M_DEV1`](../../Reference/dig/MdigAlloc.md)).
  With both Zebra Radient eV-CL SF and DF, each acquisition path supports a video source in the Camera Link Medium, Full, or 80-bit configuration.
