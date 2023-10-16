# Request for Comment &mdash; &mu;Seismic Data Exchange Format
## Purpose

The objective of this Request for Comment (RFC) is to present a new, unified, and interoperable standard tailored for exchanging &mu;seismic data in mining environments. This RFC seeks valuable feedback and input from industry stakeholders, researchers, and vendors to ensure the robustness and adaptability of the proposed format.

In the current mining scenario, the absence of a standardized data format, combined with the proliferation of proprietary systems, has created barriers to efficient data exchange and integration. This results in potential inefficiencies, increased costs, and limitations in adopting multivendor solutions. Given the complexities and unique requirements of linked to the microseismic monitoring data collected by mining operations, there's a pronounced need for a format that can accommodate both triggered and continuous data, catering to a spectrum of waveform data types, be it triggered fixed length (mostly use by ESG), triggered variable length (mostly used by IMS), or continuous.

Drawing inspiration from tried-and-tested standards in earthquake seismology (miniSEED, QuakeML and StationXML), this document introduces the necessary modifications to adapt to the specifics of the mining context. The overarching aim is to usher in a comprehensive, open-source standard that promotes consistent and unhindered data interchange across a variety of systems and platforms. Through this endeavor, we anticipate fostering collaboration, improving data analysis capabilities, and paving the way for innovation in microseismic monitoring in mining.

## Scope

The aim of this standard is to define a comprehensive data format encompassing three pivotal components: waveforms, catalogue information, and inventory data. To ensure both reliability and wide adaptability, we've grounded our approach on globally acknowledged seismological formats: miniSEED for waveforms, QuakeML for catalogue data, and StationXML for inventory specifics.

While these foundational formats offer robustness, the distinct challenges and specifications of mining environments necessitate custom adaptations. Consequently, this document delves into the integration of Cartesian coordinate systems, distinct from the traditional spherical systems, and the expansion of naming conventions within the QuakeML and StationXML formats to aptly cater to the mining milieu.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MTAwNTE1XX0=
-->