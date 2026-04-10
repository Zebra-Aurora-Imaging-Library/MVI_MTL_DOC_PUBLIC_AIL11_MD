---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CoF
section: Allocating_independent_digitizers_on_Rapixo-CoF
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cof / Allocating independent digitizers on Rapixo-CoF"
---

# Allocating independent Aurora Imaging Library digitizers on Zebra Rapixo CoF boards

You can allocate up to four independent Aurora Imaging Library digitizers on your Zebra Rapixo CoF, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). Independent digitizers have different device numbers and use the same acquisition path. You must allocate a digitizer for each CoaXPress link. A CoaXPress link contains all the connections and components to capture from one video source.
