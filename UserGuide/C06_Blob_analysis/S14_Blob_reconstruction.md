---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Blob_reconstruction
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Blob reconstruction"
---

# Blob reconstruction

Although the Blob Analysis module is used mainly for blob feature calculation purposes, some of the [`Mblob...`](../../Reference/blob/MblobAlloc.md) functions can be used to perform blob image reconstruction. An example of this is the [`MblobReconstruct`](../../Reference/blob/MblobReconstruct.md) function.

Blob reconstruction allows you to remove small particles in an image without distorting the shape of the remaining blobs, which can occur when successive erosion or dilation operations are performed on an image.

Using the different operations of the [`MblobReconstruct`](../../Reference/blob/MblobReconstruct.md) function, you can:

- **Reconstruct blobs from a seed image**. The [`M_RECONSTRUCT_FROM_SEED`](../../Reference/blob/MblobReconstruct.md) operation copies to the destination buffer only those blobs that have a corresponding seed pixel in the seed buffer. Blobs that are not seeded are replaced according to the selected processing mode.
  *[Image: BlobReconstruction2.png]*
  Note that when performing this operation in grayscale processing mode and with [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) set, blobs in the source image which are not seeded are filled with the average grayscale value of the background.
- **Delete blobs that touch a border of the image**. The [`M_ERASE_BORDER_BLOBS`](../../Reference/blob/MblobReconstruct.md) operation copies only those blobs that do not touch the border to the destination buffer.
  *[Image: BlobReconstruction.png]*
  Note that when performing this operation in grayscale processing mode and with [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) set, border blobs are filled with the average grayscale value of the background.
- **Fill holes in blobs.** The [`M_FILL_HOLES`](../../Reference/blob/MblobReconstruct.md) operation copies the source buffer to the destination buffer, filling those blobs that contain holes according to the selected processing mode. Holes that touch the image border are not considered to be holes.
  *[Image: BlobReconstruction3.png]*
  Note that when performing this operation in grayscale processing mode, the holes are filled with the average grayscale value of the blob.
- **Extract holes from blobs.** The [`M_EXTRACT_HOLES`](../../Reference/blob/MblobReconstruct.md) operation copies only the holes within the blobs found in the source buffer. Holes that touch the image border are not considered to be holes.
  *[Image: BlobReconstruction4.png]*
  Note that when performing this operation in grayscale processing mode, the copied blob holes are filled with the average grayscale value of the blob in the source buffer. Also, when using the grayscale mode with [`M_FOREGROUND_ZERO`](../../Reference/blob/MblobReconstruct.md) set, the blobs are filled with the average grayscale value of the background.

Finally, you can perform other types of blob reconstruction by calling specific Aurora Imaging Library functions, one after the other. For example, to fill all blobs with a required value, use [`MblobSelect`](../../Reference/blob/MblobSelect.md) in conjunction with [`MblobDraw`](../../Reference/blob/MblobDraw.md) and/or [`MblobLabel`](../../Reference/blob/MblobLabel.md). You can use [`MblobLabel`](../../Reference/blob/MblobLabel.md) to fill the blobs with their label values. You could use[`M_DRAW_BLOBS`](../../Reference/blob/MblobDraw.md) to fill the blobs with a user-specified value. Alternatively, you could use [`M_DRAW_BLOBS_CONTOUR`](../../Reference/blob/MblobDraw.md) + [`M_DRAW_HOLES_CONTOUR`](../../Reference/blob/MblobDraw.md) to only fill the borders of the blobs with the user-specified value.
