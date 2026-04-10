---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Bead
section: Steps_to_validate_beads
module_tag: bead
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / bead / Steps to validate beads"
---

# Steps to validate beads

The following steps provide a basic methodology for using the Aurora Imaging Library Bead module:

1. Allocate a bead context to hold your bead templates, using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md).
2. Allocate a bead result buffer to hold the results of the verification operation, using [`MbeadAllocResult`](../../Reference/bead/MbeadAllocResult.md).
3. Add a bead template to the bead context, using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md).
   You can repeat this step to add multiple templates.
4. Specify training settings, using [`MbeadControl`](../../Reference/bead/MbeadControl.md).
5. Train the templates, using [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).
   If necessary, you can perform certain drawing operations based on the trained templates, using [`MbeadDraw`](../../Reference/bead/MbeadDraw.md).
6. Verify the training status of the templates in the context, using [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md).
   If the templates are not completely trained, you can use [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) and [`MbeadControl`](../../Reference/bead/MbeadControl.md) to modify the templates and then re-call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).
7. Specify verification settings, using [`MbeadControl`](../../Reference/bead/MbeadControl.md).
8. Verify beads in a target image against the bead templates, using [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).
   If necessary, you can perform certain drawing operations based on bead results, using [`MbeadDraw`](../../Reference/bead/MbeadDraw.md).
9. Retrieve the required bead results from the result buffer, using [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md).
   If necessary, you can use [`MbeadControl`](../../Reference/bead/MbeadControl.md) to modify verification settings and then re-call [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).
10. If necessary, save your bead context, using [`MbeadSave`](../../Reference/bead/MbeadSave.md) or [`MbeadStream`](../../Reference/bead/MbeadStream.md).
   Once you save a bead context, you can later restore it, using [`MbeadRestore`](../../Reference/bead/MbeadRestore.md) or [`MbeadStream`](../../Reference/bead/MbeadStream.md). Restoring a bead context instead of creating it again can be more efficient, especially if the restored context requires no further modifications. You can save or restore a bead context to/from a file or memory stream.
11. Free all your allocated bead objects, using [`MbeadFree`](../../Reference/bead/MbeadFree.md), unless [`M_UNIQUE_ID`](../../Reference/bead/MbeadAlloc.md) was specified during allocation.
