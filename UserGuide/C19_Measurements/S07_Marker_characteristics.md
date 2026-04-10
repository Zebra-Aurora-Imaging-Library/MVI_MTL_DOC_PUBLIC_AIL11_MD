---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Marker_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Marker characteristics"
---

# Marker characteristics

A marker's characteristics are its identifying qualities, such as its search region and polarity. The more precisely you define a marker's characteristics, the more likely [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) will find the marker you want.

To locate markers, the find operation uses two phases, each of which validates a specific set of characteristics:

- Phase one, which validates essential characteristics, set using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md).
- Phase two, which validates score characteristics, set using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md).

[`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) has essential characteristic settings that you should consider both basic (you typically change them) and advanced (you rarely change them), while [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) should be considered entirely advanced.

## Essential characteristics

During the first phase of the search, Aurora Imaging Library locates markers based on whether they have all the essential characteristics, as specified with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). Aurora Imaging Library considers every characteristic set with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) an unconditional requirement and discards any marker that does not have all of them. For example, a marker that does not have the specified polarity characteristic ([`M_POLARITY`](../../Reference/meas/MmeasSetMarker.md)) can never be found, regardless of any other setting.

Marker characteristics are set to their default upon the allocation of the marker buffer ([`MmeasAllocMarker`](../../Reference/meas/MmeasAllocMarker.md)). Typically, essential characteristics other than the search region's origin or center can be set to [`M_ANY`](../../Reference/meas/MmeasSetMarker.md) if the value is unknown or not a criteria. [`M_ANY`](../../Reference/meas/MmeasSetMarker.md) generally indicates that you want to ignore that characteristic. Markers that have all the essential characteristics move on to phase two.

## Score characteristics

During the second phase of the search, Aurora Imaging Library calculates a score for each marker characteristic specified with [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md), and the marker with the highest score is found. Unlike essential characteristics, which are either met or not, [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) allows you to specify a range of acceptability, resulting in a characteristic score based on how well the calculated characteristic falls within that range. For example, you can specify that markers with a higher contrast score are better than markers with a lower contrast ([`M_EDGE_CONTRAST_SCORE`](../../Reference/meas/MmeasSetScore.md)), this allows you to find the marker with the best contrast while at the same time not eliminating markers with lower contrasts. For more information, see [Score characteristics](S14_Score_characteristics.md).

If you do not specify any score characteristic, Aurora Imaging Library only considers the marker's edge strength ([`M_STRENGTH_SCORE`](../../Reference/meas/MmeasSetScore.md)). Therefore, the marker with the strongest edges (highest edgevalues), and with all the essential characteristics, is found by default. This behavior is typically sufficient for most cases, and you need not call [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). However, in advanced applications, or when you must distinguish between similar markers, score characteristics can prove useful to fine tune your search.
