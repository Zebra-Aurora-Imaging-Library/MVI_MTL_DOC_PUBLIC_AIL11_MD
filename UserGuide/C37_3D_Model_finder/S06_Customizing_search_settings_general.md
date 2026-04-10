---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Customizing_search_settings_general
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Customizing search settings general"
---

# Customizing search settings

With successive calls to [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md), you can customize the search settings of the 3D model finder context or of the model in the 3D model finder context. You can inquire about any 3D model finder context setting or individual model setting using [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md).

Fundamental model search settings include the maximum expected model coverage, the acceptance and certainty levels (which determine what is considered a match), and the expected number of occurrences to find. For model-type specific search settings, see [Search settings specific to geometric models](S07_Search_settings_specific_to_geometric_models.md) and [Search settings specific to surface models](S08_Search_settings_specific_to_surface_models.md).

Most of the model search settings can affect the speed and robustness of your application. It is recommended that you begin with the default settings, and then, as your application demands, adjust the individual settings as required.

> **Note:** Note that changing control type settings of a find 3D model finder context or model requires preprocessing the context again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md).

## Maximum expected coverage

The model coverage is the percentage of the model's surface covered with inlier points found in the occurrence. The model coverage of an occurrence defines its score. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md). Note that the default maximum expected coverage is model specific. Cylinder and sphere models have a default maximum expected coverage of 45%, whereas box, rectangular plane, and surface models have a default maximum expected coverage of 100%. An occurrence found with a coverage greater than or equal to the maximum expected coverage will have a score of 100%. To learn more about maximum expected coverage and how the score is calculated, see [Score](S05_Determining_what_is_a_match.md).

## Acceptance level

The acceptance level determines the minimum score required for an occurrence to be considered a match. The acceptance level is defined relative to the maximum expected model coverage, such that setting [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md) to 50% indicates that returned occurrences must have a total model coverage greater than or equal to half the value of [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md). You can use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md) to set the acceptance level.

Aurora Imaging Library will search your point cloud for the required number of occurrences, returning the occurrences with the best scores greater than or equal to the acceptance level. If the score of an occurrence is less than the specified acceptance level, it is not considered a match and is not returned as an occurrence.

For example, setting [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md) and [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md) to 100% means that you will only accept occurrences containing model surfaces that are completely covered. That is, valid inlier points cover the entirety of the model's surface at the location of the occurrence. Perfect matches are generally unobtainable in real 3D scenes because of noise and distortion introduced when grabbing 3D data. You should use a reasonable acceptance level that is high enough to avoid false matches, but not so high that occurrences are missed. If your point clouds have considerable noise and/or distortion, or if occlusion of occurrences is expected, you might have to set the acceptance level below the default value of 60%.

## Certainty level

The certainty level is used to speed up the search when very good matches are expected often. You can set the certainty level for the score of a specified model using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) (the default setting is 90%).

Any occurrence whose score is greater than or equal to the certainty level is considered a certain match. The certainty level is defined relative to the maximum expected model coverage, such that setting [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md) to 100% indicates that certain matches must have a total model coverage equal to the maximum expected model coverage.

The certainty level determines the score above which (or equal to) the algorithm can assume that it has found a certain match. If you specify the number of expected occurrences, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md), the search will end as soon as the module finds an amount of certain matches equal to the number to which you set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md), without searching the rest of the point cloud for better matches. If no certain matches are found, Aurora Imaging Library will search the entire point cloud and return the matches with the highest scores greater than or equal to the acceptance level.

If necessary, when searching for a fixed number of occurrences in a point cloud, you can ensure that Aurora Imaging Library always returns those occurrences with the highest match score among those found, by setting the certainty level of the model to 100%. Aurora Imaging Library will return occurrences with the best score(s) greater than or equal to the acceptance or those with scores of 100%. Note that doing so will probably increase the search time.

## Expected number of occurrences

You can set the expected number of occurrences for the model in your 3D model finder context.

To do so, use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md). The default value is 1. To find all occurrences of a model, set [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md) to [`M_ALL`](../../Reference/3dmod/M3dmodControl.md).

Note that setting the number of occurrences to [`M_ALL`](../../Reference/3dmod/M3dmodControl.md) can significantly slow the [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operation, depending on the complexity of your point cloud. It is recommended that you specify the exact number of expected occurrences whenever possible.

Once the number of occurrences for the model has been found, with scores greater than or equal to the certainty level, the search will stop. If there are fewer certain matches than the specified expected number of occurrences, the search will continue until the best matches with the highest scores greater than or equal to the acceptance level are found.
