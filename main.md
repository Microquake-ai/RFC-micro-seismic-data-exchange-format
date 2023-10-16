# Request for Comment &mdash; &mu;Seismic Data Exchange Format
## Introduction
### Purpose

The objective of this Request for Comment (RFC) is to present a new, unified, and interoperable standard tailored for exchanging &mu;seismic data in mining environments. This RFC seeks valuable feedback and input from industry stakeholders, researchers, and vendors to ensure the robustness and adaptability of the proposed format.

In the current mining scenario, the absence of a standardized data format, combined with the proliferation of proprietary systems, has created barriers to efficient data exchange and integration. This results in potential inefficiencies, increased costs, and limitations in adopting multivendor solutions. Given the complexities and unique requirements of linked to the microseismic monitoring data collected by mining operations, there's a pronounced need for a format that can accommodate both triggered and continuous data, catering to a spectrum of waveform data types, be it triggered fixed length (mostly use by ESG), triggered variable length (mostly used by IMS), or continuous.

Drawing inspiration from tried-and-tested standards in earthquake seismology (miniSEED, QuakeML and StationXML), this document introduces the necessary modifications to adapt to the specifics of the mining context. The overarching aim is to usher in a comprehensive, open-source standard that promotes consistent and unhindered data interchange across a variety of systems and platforms. Through this endeavor, we anticipate fostering collaboration, improving data analysis capabilities, and paving the way for innovation in microseismic monitoring in mining.


### Scope

The aim of this standard is to define a comprehensive data format encompassing three pivotal components: waveforms, catalogue information, and inventory data. To ensure both reliability and wide adaptability, we've grounded our approach on globally acknowledged seismological formats: miniSEED for waveforms, QuakeML for catalogue data, and StationXML for inventory specifics.

While these foundational formats offer robustness, the distinct challenges and specifications of mining environments necessitate custom adaptations. Specifically, our enhancements to these formats are as follows:

#### QuakeML Adaptations:

1.  **Coordinate System**: Transition from a traditional spherical system to a Cartesian coordinate system tailored for mining contexts.
2.  **Magnitude Description**: Modifications to cater to the specific magnitude scales and measurement techniques prevalent in mining seismology.
3.  **Event Types**: Adapting the format to ensure the prescribed event types are suited to describe the mining seismic activities, that are distinct from conventional seismological events.

#### StationXML Adaptations:

1.  **Coordinate System**: Integration of a Cartesian coordinate system to more aptly represent the spatial characteristics of mining operations.

Through these adaptations, this document aims to bridge the gap between established seismological data formats and the unique requirements of the mining sector, ensuring seamless data interchange and analysis.

### Terminology

-   **Waveforms**: These are digital renditions of seismic waves. For the purpose of this standard, they are primarily encapsulated using the miniSEED format.
    
-   **Catalogue Information**: This is a structured compilation of seismic events, detailing their origins, magnitudes, and other pertinent data. Within the scope of this standard, we represent this information using the QuakeML format, augmented with extensions tailored for the mining context.
    
-   **Inventory Data**: Refers to a comprehensive registry detailing seismic instruments, pinpointing their locations, and delineating their unique characteristics. StationXML, with specific modifications for mining contexts, is the chosen format to represent this data in this standard.
    
-   **miniSEED**: A predominant data format in the realm of seismology, miniSEED is instrumental in storing seismic waveform data.
    
-   **QuakeML**: An XML-oriented format, QuakeML is crafted to articulate seismic event parameters.
    
-   **StationXML**: This is an XML-centric format devised to encapsulate the nuances and specifics of a seismic station. It encompasses details about the station's sensors and the metadata associated with the recorded data.
  
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1NDgwMzUwOCwtMTgxMDA1MTVdfQ==
-->