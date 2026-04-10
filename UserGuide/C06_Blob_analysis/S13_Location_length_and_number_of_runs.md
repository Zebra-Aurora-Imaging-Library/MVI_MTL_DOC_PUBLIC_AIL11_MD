---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Location_length_and_number_of_runs
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Location length and number of runs"
---

# Location, length and number of runs

A run is defined as a horizontal series of consecutive foreground pixels. The Blob Analysis module can be used to calculate run-related information such as the length of a run, and the X and Y-coordinates of each run in a specific blob. Enable these features for calculation with the [`M_RUNS`](../../Reference/blob/MblobControl.md) group of features using [`MblobControl`](../../Reference/blob/MblobControl.md). To retrieve the run information, use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md). The retrievable run information includes the total number of runs for each blob ([`M_NUMBER_OF_RUNS`](../../Reference/blob/MblobGetResult.md)), or for all included blobs ([`M_TOTAL_NUMBER_OF_RUNS`](../../Reference/blob/MblobGetResult.md)), and the length and coordinates of each run in a specific blob ([`M_RUN_...`](../../Reference/blob/MblobGetResult.md)).
