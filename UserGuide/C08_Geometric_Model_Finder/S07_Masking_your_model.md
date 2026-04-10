---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Masking_your_model
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Masking your model"
---

# Masking your model

Once you have added a model to your geometric ([`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md)) or controlled geometric ([`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md)) type of Model Finder context, you can then mask any irrelevant, inconsistent, or featureless areas of your model using [`MmodMask`](../../Reference/mod/MmodMask.md). You cannot mask models added to an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

For models in supported contexts, you can define three types of masks: a "don't care", a "flat region", and a "weighted region" mask. You can use these types of masks with the same model. Note that "don't care" and "flat region" masks are not supported for synthetic models.

With a "don't care" mask ([`M_DONT_CARE`](../../Reference/mod/MmodMask.md)), Aurora Imaging Library ignores the masked regions when searching for occurrences of the model. Masked edges in the model edge map and edges in the corresponding regions of the target edge map will be ignored and will not contribute to either the score or the target score. These regions might be noise edges, unwanted edges, inconsistent features, or simply regions that are irrelevant to your search.

In the following example, a mobile phone is used as the model. However, occurrences of this model have displays containing inconsistent characters from target image to target image. Since these characters are variable, they should be masked in the model.

*[Image: Masking.png]*

With a "flat region" mask ([`M_FLAT_REGIONS`](../../Reference/mod/MmodMask.md)), the masked regions are expected to be featureless in the found occurrence, and any edges present in these regions will reduce the target score. Masked edges in the model edge map will be ignored while edges in the corresponding regions of the target edge map will lower the target score. Features that are present in a region where none are expected can indicate that an error has occurred, for example in the manufacturing process.

With a "weighted region" mask ([`M_WEIGHT_REGIONS`](../../Reference/mod/MmodMask.md)), the extracted model features have weights according to the values of the "weighted region" mask. The weights of the features are used to calculate the score; although the weights themselves don't affect the target score, edges corresponding to negative weights will be ignored when calculating the target score. The valid range for the weights of the mask is -127 to +127.

Positive weights indicate how significant it is for a given pixel to be part of an edge in the occurrence; the more positive the weight, the greater the influence on the score. The absence of a pixel that corresponds to a very positive weight reduces the score more than the absence of one with a lower weight. The longer the edges with a positive weight, the more significant the edges to the target coverage and to the target score.

Negative weights indicate how significant it is for a given pixel not to be part of an edge of the occurrence; the more negative the weight, the more the influence on the score. The presence of a pixel that corresponds to a very negative weight will reduce the score more than the presence of one that corresponds to a less negative weight.

The effect of positive and negative weights on the score is relative. A "weighted region" mask that has negative weights of -100 and positive weights of 100 will have the same score as the same mask where the negative weight is -1 and the positive weight is 1.

Note that weights that have no correspondence to edges in the model have no effect on the score.

"Weighted regions" are particularly useful when the pattern you are searching for in the target also exists as a subset of other patterns; that is, you cannot otherwise identify the required pattern uniquely. In these cases, you have to create a model that includes the unwanted extra edges and mask the extra edges with negative weights.

In the example below, the square is the shape that is being searched for, while every other shape is unacceptable. The "weighted region" mask will cause the one acceptable shape to be found with a high score, while ensuring that the two other shapes that are similar are found with a low score.

*[Image: MaskExamples.png]*

A separate image buffer is used to create each mask and each buffer must be of the same size as the model. Note that if you modify the margin of a synthetic model's bounding box after a mask has been applied, the mask will be discarded since it is no longer the same size as the model. You must then re-apply a mask that is of the same size as the new model box.

For a "don't care" or "flat region" mask, the buffer must be a 1-band 8-bit unsigned buffer, where any non-zero pixel value is considered a masked pixel. For a "weighted region" mask, the buffer must be a 1-band 8-bit signed buffer and the valid range for the weights of the mask is -127 to +127. This buffer can be calibrated; however, even if the model is calibrated, the mask is applied on a pixel basis.

Note that if you expect significant misalignment between the model and the target, you might need to compensate for this by slightly increasing the region of the "don't care" or "flat region" pixels in the mask.

If you want to verify the mask that has been applied to a model, you can use [`MmodDraw`](../../Reference/mod/MmodDraw.md) to draw the model's masked pixels. If necessary, you can clear a model's current mask by setting the mask to [`M_NULL`](../../Reference/mod/MmodMask.md).

Finally, when you change the masked pixels of a model, you must preprocess the search model again.
