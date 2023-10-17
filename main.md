# Request for Comment &mdash; &mu;Seismic Data Exchange Format

## Introduction

### Purpose

This Request for Comment (RFC) formally introduces a proposed unified and interoperable standard tailored for the exchange of μseismic data harvested by mine μseismic monitoring systems. Fundamental to this standard is its capacity for a lossless data export, ensuring the inclusion of all critical data elements—encompassing waveform data, comprehensive inventory information (inclusive of sensor location, type, and associated response files), and catalogue data—thus furnishing all necessary components for optimal data utility. It is imperative to emphasize that this standard's primary design does not target efficient channel-by-channel data recovery from disk storage. For such specialized retrieval requirements, alternative formats such as TileDB, Zarr, or ASDF would be more appropriate.

This proposed standard exhibits adaptability, rendering it suitable for a variety of applications, ranging from the storage and dissemination of triggered or continuous data to the integration of distributed acoustic sensing (DAS) data. By building upon the foundational principles established by recognized earthquake seismology standards—miniSEED, QuakeML, and StationXML—this document elucidates the necessary modifications tailored to address the distinctive characteristics of the mining environment. The ultimate objective is to inaugurate a comprehensive, open-source standard, facilitating unobstructed data interchange across an array of systems and platforms. This initiative aspires to cultivate collaboration, augment analytical capabilities, and stimulate innovation in the domain of microseismic monitoring within the mining industry.

In presenting this RFC, feedback, scrutiny, and constructive contributions are actively solicited from industry stakeholders, academic researchers, and equipment vendors. Such collaborative input is imperative to enhance the robustness and adaptability of the proposed format, ensuring its relevance and efficacy for future deployments.


### Scope

This standard seeks to establish a data format tailored for the exchange of μseismic data acquired by seismic systems operating within mines. The encapsulated proposal comprises three components essential for lossless translation of waveforms, catalogue, and system information and prescribe its usage. Our foundational strategy leverages widely recognized seismological formats, namely miniSEED for waveform representation, QuakeML for event metadata, and StationXML for system metadata.

At the heart of our approach are the following seismological formats:

-   **miniSEED**: Compact and versatile, miniSEED serves as the container for seismic waveform representation across various seismological applications. It guarantees the efficient compression and storage of vast seismic datasets.
    
-   **QuakeML**: An XML-based data format tailored for the representation of seismological event parameters. Its structured design enables comprehensive documentation of seismic event metadata, including origins, magnitudes, and focal mechanisms and moment tensor.
    
-   **StationXML**: This format provides a detailed and structured representation of seismic network, station, and channel metadata. It encompasses crucial information about instrument responses, location, and other attributes critical for accurate seismic data interpretation.
    
The selected building blocks are acknowledged for their robustness, compatibility, and broad acceptance within the seismological community. While miniSEED, QuakeML, and StationXML are well-designed is comprehensive, they have historically been oriented towards global seismic monitoring needs. Although they can be employed in their current form for μseismic monitoring in mines, they would benefit from nuanced modifications to more closely align with the unique demands and intricacies of μseismic monitoring within mining environments. Consequently, our proposed adaptations to these foundational formats are summarized as follows:

#### QuakeML Adaptations:

1.  **Coordinate System**: Transition from a traditional spherical system to a Cartesian coordinate system tailored for mining contexts.
2.  **Magnitude Description**: The magnitude description requires the inclusion of the corner frequency for completeness.  
3.  **Event Types**: Adapting the format to ensure the prescribed event types are suited to describe the mining seismic activities, that are distinct from conventional seismological events.

#### StationXML Adaptations:

1.  **Coordinate System**: Integration of a Cartesian coordinate system to more aptly represent the spatial characteristics of mining operations.

Through these adaptations, this document aims to bridge the gap between established seismological data formats and the unique requirements of the mining sector, ensuring seamless data interchange and analysis. This document also prescribes the use of those format and provides usage examples

### Terminology

-   **Waveforms**: These are digital renditions of seismic waves. For the purpose of this standard, they are primarily encapsulated using the miniSEED format.
    
-   **Catalogue Information**: This is a structured compilation of seismic events, detailing their origins, magnitudes, and other pertinent data. Within the scope of this standard, we represent this information using the QuakeML format, augmented with extensions tailored for the mining context.
    
-   **Inventory Data**: Refers to a comprehensive registry of all the seismic instrument along the acquisition chain. It comprises detailed information on sensor location, orientation and response. 
    
-   **miniSEED**: A predominant data format in the realm of seismology, miniSEED is instrumental in storing seismic waveform data. 
    
-   **QuakeML**: An XML-oriented format, QuakeML is crafted to articulate seismic event parameters.
    
-   **StationXML**: This is an XML-centric format devised to encapsulate the nuances and specifics of a seismic station. It encompasses details about the station's sensors and the metadata associated with the recorded data.

### Rationale

#### Need for a New Standard

Microseismic data plays a pivotal role in enhancing both the safety and efficiency of underground mining operations. While it's one of several essential tools in the arsenal of modern mining techniques, the current scenario of diverse data formats can sometimes complicate seamless integration and analysis.

-   **Unified Data Integration**: The presence of multiple proprietary formats can require tailored solutions for consolidating microseismic data from various sources or vendors. This might not only increase costs but also lead to potential inconsistencies in data interpretation.

#### Goal

Acknowledging these subtleties, the proposed standard comes with the following objectives:

-   **Promote Interoperability**: A common format ensures a smooth exchange, processing, and analysis of microseismic data across a range of systems, championing collaboration and data coherence regardless of vendor distinctions.
    
-   **Promote Innovation**: A harmonized framework can invigorate a healthy competitive milieu. Vendors are then positioned to focus on elevating equipment quality, finessing data analysis methodologies, and providing top-tier customer support, free from data format nuances.
    
-   **Leverage Established Seismological Standards**: This standard harnesses the strength of existing seismological data formats. While they form the foundational backbone, specific extensions are melded in to meet the unique requirements of the mining industry.
    
-   **Catalyze Technological Advancements**: With a universally endorsed standard, the mining sector is primed to adopt and assimilate breakthroughs in microseismic monitoring and analysis, ensuring continued advancement in safety and operational efficiency.

-  **Prescribe Usage**: The propose format is comprehensive and flexible. To ensure consistency, portability and compatibility, the format must be used in a consistent fashion across multiple application.

### Data Format Specification for &mu;Seismic Data Exchange (MDE v1.0)

#### Introduction

Within the evolving &mu;seismic industry, the &mu;Seismic Data Exchange Format (MDE v1.0) presents a solution for efficient and adaptable lossless and comprehensive data exchange mechanisms. Denoted by the `.mde` extension, this format captures the intricate details of microseismic data, fostering data sharing across various platforms and applications. The proposed format aims to provide the required flexibility to represent most types of seismic data including but not necessarily limited to triggered, continuous and distributed acoustic sensing (DAS)..

#### Format Components Overview

The MDE v1.0 standard is based on three components some used as is and some adapted to align with the need  &mu;seismic data:

**1. MiniSEED**  
Esteemed in the seismic community, miniSEED’s compact nature serves to represent both continuous data streams and individual event recordings. Salient features:

-   **Compactness**: Facilitates efficient storage and transfer of extensive seismic datasets.
-   **Versatility**: Encapsulates both continuous data and event-focused recordings.
-   **Independence**: Remains unaffected by specific recording equipment.
-   **Streaming**: The format is designed for streaming purpose breaking down the structure in blocks of predefined size. A block size of 4096 is recommended, but the block size can be adapted to  

By retaining the core essence of miniSEED, the MDE v1.0 format guarantees compatibility with existing seismological tools and platforms.

The **MiniSEED** format allows for the waveform data to be stored with acquisition parameters such as sampling rate, start time, and the number of sample to be independently defined on a channel by channel basis. The format 

_Reference to miniSEED official documentation_

**2. QuakeML v2.0 Adaptations for Microseismic Data**  
While retaining the foundational structure of QuakeML v2.0, some adaptations for the microseismic realm include:

-   **Origin**: Utilizes Cartesian coordinates, specifically Easting, Northing, and a Z-coordinate that can represent either Depth or Elevation. An associated flag indicates the direction of the Z-coordinate.
-   **Magnitude**: Enhanced to capture corner frequencies and energy parameters.
-   **Event Types**: Augmented to encompass specific event types relevant to mining scenarios. This was achieved by adapting standard QuakeML types to include those prevalent in the mining sector.

_Note on derived source parameters and on-the-spot calculations_

**3. StationXML v1.2 for Microseismic Data**  
StationXML v1.2, primarily used for detailing metadata associated with seismic stations, has been tailored for the microseismic context:

-   **Hierarchy & Definitions**: The inherent hierarchy (Inventory > Networks > Stations > Locations > Channels) remains, yet with nuances adjusted for microseismology.
-   **Microseismic-specific Adaptations**: Introduces attributes such as Cartesian coordinates and channel orientation, which have been modified or introduced to meet the specialized needs of the microseismic landscape.

_Deviation from the standard includes the utilization of Easting, Northing, and Z-coordinates for station and channels, channel orientation based on the cosine vector, and the allowance for longer station names._



#### File Packaging and Structure

MDE v1.0 suggests a distinct packaging strategy. For **triggered data**, the package should comprise three components: `catalog.xml`, `stream.mseed`, and `inventory.xml`. For **continuous data**, only `stream.mseed` and `inventory.xml` are necessary. The files should be packaged using the tar protocol, then compressed using gzip. Upon decompression, the `.mde` package would reveal these constituent files, each echoing with event catalogs, waveform data, or inventory information respectively.

#### Coordinate System Conversion

The Cartesian system is designed to be straightforward and unambiguous. For seamless interoperability, metadata should facilitate conversion to and from spherical coordinates. This includes potential usage in the imperial coordinate system (e.g., feet). Conversion can be achieved through operations like rotation, translation, and scaling.

_Python libraries like `pyproj` can aid in this transformation. For example:_

```python
import pyproj
# Define the conversion using pyproj
converter = pyproj.Transformer.from_crs("epsg:xxxx", "epsg:yyyy")

# Convert coordinates
easting, northing, z = converter.transform(x, y, z)
```

#### Concluding Remarks

With the inception of MDE v1.0, the microseismic community gains a robust and specialized data interchange instrument. This standard marries tried-and-tested formats with purpose-driven modifications, ensuring an optimized and trustworthy data exchange paradigm.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ2OTIwNjEzOSwyMTEwNzA0OTk2LC0xMD
A2MjY3NzI3LDE3NDIyMzYsLTE2NjM3NTAwNDAsMjY5NTMzNjUx
LDIxMjk4MTAzNDEsLTE4NzgwMDczMCw4MTYxMDc0ODQsLTQ2NT
UyOTI3MywyMDAwNzc0NDI5LC0zNTQ4MDM1MDgsLTE4MTAwNTE1
XX0=
-->