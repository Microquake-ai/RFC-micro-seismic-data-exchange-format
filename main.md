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
  
### Rationale

#### Need for a New Standard

Microseismic data are broadly used in underground mining, guiding pivotal decisions safety critical and shaping operational strategies. However, the current landscape, devoid of a universally recognized data format, presents numerous challenges:

-   **Vendor Lock-in**: The proprietary nature of data formats means mines can inadvertently tether themselves to a single vendor's seismic system. This captivity limits their agility in embracing emerging technologies or pivoting to alternative systems that may offer enhanced features or economic advantages.
    
-   **Data Interoperability Concerns**: A lack of standardization complicates the integration of microseismic data emanating from diverse sources or vendors. Each integration often demands tailor-made solutions, escalating costs and heightening the risk of data discrepancies.
    
-   **Stifled Innovation**: An absent standard can impede competition, potentially throttling the pace of technological evolution. When clients are ensnared by data format incompatibilities, vendors might experience diminished motivation to push technological boundaries.
    

#### Goal

Given these challenges, this standard is sculpted with the following paramount objectives:

-   **Promote Interoperability**: Championing a standardized format ensures the fluid exchange, processing, and analysis of microseismic data across a spectrum of systems, regardless of vendor affiliations.

<!--    
-   **Boost Vendor Competition**: A harmonized standard reshapes the competitive landscape, nudging vendors to rival based on equipment excellence, sophisticated data analysis tools, and unparalleled customer support, rather than the confines of proprietary data formats.
--!>
    
-   **Leverage Tried-and-Tested Seismological Standards**: Instead of charting an entirely new path, this standard is anchored in time-tested seismological data formats. While these provide a sturdy and reliable bedrock, bespoke extensions are woven in to cater to the mining sector's distinct requisites.
    
-   **Pave the Way for Technological Progress**: A universally embraced standard lays the groundwork for revolutionary advances in microseismic monitoring and analysis, ensuring the mining industry remains at the forefront of cutting-edge innovations.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY0NzcyMDU0NywtMzU0ODAzNTA4LC0xOD
EwMDUxNV19
-->