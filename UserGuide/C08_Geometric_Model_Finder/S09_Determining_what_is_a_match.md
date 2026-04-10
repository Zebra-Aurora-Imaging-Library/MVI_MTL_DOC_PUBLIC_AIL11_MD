---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Determining_what_is_a_match
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Determining what is a match"
---

# Determining what is a match

Before customizing your search settings, it is necessary to understand how a match between your model and occurrences in the target is determined. The score ([`M_SCORE`](../../Reference/mod/MmodGetResult.md)) and the target score ([`M_SCORE_TARGET`](../../Reference/mod/MmodGetResult.md)) are the primary factors in determining which occurrences are considered matches with the models in your Model Finder context.

## Score, target score, and fit score

The **score** is a measure of the active edges in the model found in the occurrence, weighted by the deviation in position of these common edges. Active edges in the model not found in the occurrence reduce the score. The **target score** is a measure of edges found in the occurrence that are not present in the original model (that is, extra edges), weighted by the deviation in position of the common edges. Edges found in the occurrence that are not present in the model will reduce the target score. Due to the nature of models in a shape-type context, target score is not supported for these types of models. Instead, for these models, there is a fit score. The **fit score** is a measure of the correlation of the edges in the model to those of the occurrence. For a model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model scores are calculated as follows:

- Score = Model coverage x [1 - (Fit error weighting factor x Normalized Fit Error)].
- Target Score = Target coverage x [1 - (Fit error weighting factor x Normalized Fit Error)].

For a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model and fit scores are calculated as follows:

- Score = (Model coverage/Max coverage) x [1 - (Fit error weighting factor x Normalized Fit Error)].
- Fit score = [1 - Normalized Fit Error].

> **Note:** Note that for all Model Finder context types, the terms within the square brackets result in a number between 0.0 and 1.0.

The model coverage, target coverage, and fit error components of the score and target score are explained below.

Note that the target coverage ([`M_TARGET_COVERAGE`](../../Reference/mod/MmodGetResult.md)) result type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder result buffer, which is why the target score ([`M_SCORE_TARGET`](../../Reference/mod/MmodGetResult.md)) is not supported either.

## Model and target coverage

The model coverage and target coverage are defined as follows:

- **Model coverage**. The model coverage is the percentage of the total length of the model's active edges found in the occurrence. 100% indicates that for each of the model's active edges, a corresponding edge was found in the occurrence.
- **Target coverage**. The target coverage is the percentage of the total length of edges present within the occurrence's bounding box, corresponding to the model's active edges. Thus, a target coverage score of 100% means that no extra edges were found. Lower scores indicate that features or edges found in the target (result occurrence) are not present in the model.
  *[Image: Coverage.png]*

Using a weighted mask with a model can affect the score and target score. In cases where a weighted mask is used, the model coverage would be as below:

*[Image: ModelCoverageFormula.png]*

The target coverage is as follows:

*[Image: TargetCoverageFormula.png]*

## Fit error

**Fit error**. The fit error is a measure of how well the edges of the occurrence correspond to those of the model. The fit error is calculated as the average quadratic distance, in pixels or calibrated units, between the edgels in the occurrence and the corresponding active edges in the model (the mean squared error):

*[Image: FitErrorEq.png]*

A perfect fit gives a fit error of 0.0. The fit error weighting factor (between 0.0 - 100.0) determines the importance to place on the fit error when calculating the score and target score.

*[Image: FitError.png]*

## Interpreting results

The following diagram illustrates how the model coverage, target coverage, and fit error work together to provide details about the nature of a result occurrence.

*[Image: Scores.png]*
