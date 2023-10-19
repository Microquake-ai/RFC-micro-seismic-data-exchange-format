
# Request For Comment &mdash; Content for &mu;Seismic Data Exchange

## Introduction

### Purpose
The purpose of this document is to invite comments on a suggested format to allow for standardized and consistent access to μseismic data collected by mine μseismic monitoring systems. This format aims to enable a seamless and lossless exchange between different platforms. The objective of the proposed standard **does not** concern how seismic data are **internally managed** within each proprietary platform, although the capability of the present format would be suited for that purpose, it only focuses on how the data are exchanged.

The proposed standard applies to a wide range of data including triggered event, continuous recording, and DAS monitoring. It allows for different data types to be combined. 

Access to the μseismic data is inconsistent among various vendors, and occasionally within different implementations by the same vendor. Variations arise from site to site, often driven by third-party requirements. Such inconsistencies lead to inefficiencies, complicating the integration of various systems that offer complementary products and services, and making data usage unnecessarily challenging.

In light of recent advancements in μseismic monitoring technology, the imperative for a standardized format becomes even more salient. The microseismic monitoring landscape is evolving, marked by the deployment of increasingly expansive monitoring systems. These systems, given their extensive scale, inherently produce voluminous data sets. Furthermore, the incursion of Distributed Acoustic Sensing (DAS) in mining adds another layer of complexity, both in terms of data volume and diversity. DAS, with its high spatial resolution and continuous monitoring capability, generates data at an unprecedented scale. A robust and standardized format is crucial to efficiently manage, process, and interpret these burgeoning data streams, ensuring that the rich information they provide can be harnessed effectively and cohesively.

This document proposes a file structure and format. At this stage, we seek alignment on the content of the information transferred and consensus on the container format.


### Scope

With the increasing volume of μseismic data collected, especially in modern expansive monitoring systems and with technologies like Distributed Acoustic Sensing (DAS), there is a need for efficient management and handling of this data. The scope of this proposed standard is to define the content and nature of μseismic information provided to third parties and to suggest a format that facilitates efficient storage, retrieval, and exchange, of complete information required to further analyse the data.

Computational efficiency in storing and retrieving large volume of data that are prescribed by increasingly expansive system and the adoption of the DAS technology, is central. The ability to provide a storage mechanism flexible and extensible enough to package both triggered and continous data is another objective.

The standard focuses on the foundational elements and seismic information intrinsic to μseismic monitoring including:

-   **Waveforms** — This includes the raw, unprocessed waveform for both continuous and triggered data.
-   **Catalog Information** — This covers event information derived from waveform data processing.
-   **Inventory or System Information** — Details about the instruments (sensors and data acquisition modules) used in the data acquisition chain are included here, along with other critical data processing information, such as velocity, density, and attenuation values or models.

### Rationale

#### Need for a New Standard

The increase in microseismic data, particularly from expansive monitoring systems and new technologies like Distributed Acoustic Sensing (DAS), underscores the pressing need for a more efficient and unified data format. Currently, the varied nature of microseismic data formats hinders streamlined integration and analysis, posing challenges in managing and deriving value dataset collected by in-mine monitoring system. The lossy and incoherent nature of current data exchange formats are hindering innovation and make the utilization of &mu;seismic data unecessarily difficul. Given the critical importance of microseismic data in ensuring the safety and improving the operations of underground mining, establishing a standardized format and mechanism of echange becomes imperative. The propose standard objective is to facilitate more straightforward data access, efficient storage, and smoother data exchanges across different platforms for a variety of data types.

#### Goal

The primary objective of this RFC is to introduce a proposed data content, packaging format, and naming conventions that are aimed at enhancing interoperability, consistency, and innovation within the sphere of microseismic monitoring. As we acknowledge the integral role of microseismic data in underground mining operations' safety and efficiency, this proposal serves as a step forward in harmonizing and optimizing the way in which data is accessed, managed, and exchanged across diverse platforms in the industry.

To this end, this proposal seeks to:

-   **Promote Universality**: By implementing a unified format tailored specifically for microseismic monitoring systems, the intent is to transcend vendor-specific or third-party constraints, allowing seamless integration across a range of platforms.
    
-   **Ensure Scalability**: Given the rising volume of data being generated by expansive microseismic monitoring systems, particularly with the advent of Distributed Acoustic Sensing (DAS) technology, the proposed format is designed to accommodate large datasets without compromising on efficiency or speed of data retrieval.
    
-   **Enhance Clarity and Coherence**: Implementing a standardized naming convention ensures a structured approach to data management, reducing ambiguities and promoting swift, clear data identification and processing.
    
-   **Facilitate Innovation**: With a consistent, standardized foundation in place, researchers, developers, and industry professionals will be better poised to innovate, iterate, and develop new tools, methodologies, and solutions that harness the full potential of microseismic data.
    
In summary, the goal is to provide a robust framework that not only addresses the present challenges posed by varied data formats but also anticipates and caters to the evolving needs of the microseismic monitoring industry. Through this proposed standard, we endeavor to usher in an era of streamlined data access, management, and exchange that can truly capitalize on the advancements in microseismic monitoring technology.

### RFC Process and Schedule

We propose the following RFC process that should lead to the standard adoption. It is unclear at this time where and by who the standard will be published. 

#### Process:

1.  **Drafting**: Development of the initial RFC document.
2.  **Publication**: Making the RFC available for a wider audience for review and comment.
3.  **Review**: A period of review where stakeholders provide feedback.
4.  **Revision**: Updates to the RFC based on feedback received.
5.  **Final Comment Period**: A last opportunity for feedback before a the final standard is published.
6.  **Adoption**: Adoption of the RFC.
7.  **Publication**: If adopted, the Standard is officially published.
8.  **Implementation**: Introduction of the recommended practices or standards from the RFC.
9.  **Review Cycle**: Periodic reviews of the RFC's effectiveness post-implementation.

#### Schedule:

-   1.  **Drafting and Initial Publication**: Completed as of 2023-10-20.
    
2.  **Review**: Given the complexities and the necessity to involve various stakeholders, we suggest allocating four months to the review. This will ensure that all participants have ample time to understand, discuss, and provide thoughtful feedback.
    -   **Start**: 2023-11
    -   **End**: 2023-03
   
3.  **Revision**: Considering the extended review period and potential for substantial feedback, one months will be set aside for revisions.
    -   **Start**: 2023-03
    -   **End**: 2023-04
   
4.  **Final Comment Period**: This phase allows stakeholders another chance to review the revised RFC and provide final comments. Two months seems reasonable for this step, given the extended timeline for other phases. 
    -   **Start**: 2023-04
    -   **End**: 2023-06
 
5.  **Adoption**: The adoption mechanism has to be determined. There is currently no body governing microseismic monitoring.   
    -   **Date**: 2023-06
    
6.  **Official Publication**: After finalizing the adoption, a week is allocated for any final edits, formatting, and dissemination of the standard.
    -   **Date**: 2023-07
    
7.  **Implementation and Review Cycle**: As before, this is a longer-term step and its start would be dependent on the decisions made and the complexity of the implementation.
    -   **Start**: 2023-07
    -   **Review Cycle**: Every 6 months (or another interval that makes sense based on the nature of the standard and its implications)

## Proposal

### Overview

Our proposal encompasses three categories of data: the waveforms, the catalog data, and the inventory and system information. It also concerns system metadata such as velocity models. 

We propose using the `ASDF` format with a `.asdf` extension to store the waveforms, the catalog (QuakeML), and inventory (StationXML) information. The `ADSF` format provides a convenient and comprehensive mechanism to store the provenance and lineage information related to the data genesis and modification to provide traceability and data history. Note that the `ASDF` format also provides a convenient way to store auxiliary data that could, for instance, include:

-   **Ambient Noise Correlations**: Pre-computed cross-correlations between stations, which can be essential for techniques like ambient noise tomography.
    
-   **Instrument Response**: Though typically part of the StationXML, having the actual instrument response curves stored as auxiliary data can facilitate more direct correction of the waveforms without referencing external databases.

- **Rays and Ray Parameters from Ray Tracing**:  Storing detailed ray paths and relevant parameters is useful for higher order analyses such as travel time and attenuation tomography, and moment tensor inversion to name only those. By integrating these data directly within the `ASDF` format, researchers can achieve a holistic understanding of the seismic event and the underlying geophysical processes, ensuring that they have all pertinent information at their fingertips without resorting to supplementary datasets or computations.

- **Velocity Information**: If relevant the velocity and velocity model could be stored alongside the seismic data. 

- **Alternative location**: Alternative locations obtain from statistical analyses including jacknife or Montecarlo associated, could provide benefit to correctly assess the system performance.
    
-   **Processing Logs**: Detailed logs of processing steps performed on the data, which can be crucial for ensuring data quality and understanding potential issues or artifacts.

To ensure interoperability, the information in the provided files must be consistent. The sensor naming convention shall remain consistent throughout. Additionally, the locations of sensors and events should be expressed using a unified coordinate system, which must also be used for grids, if applicable.

### Why `ASDF` format?

The Adaptable Seismic Data Format (ASDF) was introduced in response to the need for an efficient and comprehensive format for seismic data storage and retrieval. Developed through collaborative research by experts in the seismology field, ASDF aims to address the complexities associated with managing diverse seismic datasets. The format has been designed with adaptability in mind, ensuring both ease of use and interoperability across various platforms. As a result of its well-structured architecture, ASDF has gained widespread acceptance within the seismology community. 

The `ASDF` format was designed to efficiently store and access large-scale array-oriented scientific data. Its design addresses the challenges posed by cloud and distributed storage, allowing for concurrent reads and writes. The format shines in scenarios where data needs to be analyzed in chunks without loading the entire dataset into memory, making it particularly apt for multidimensional arrays. With built-in support for compression and chunking, ASDF facilitates rapid data access regardless of the storage backend, whether it's file systems, object storage, or databases.

The ASDF (Adaptable Seismic Data Format) file structure is meticulously designed to cater to the specific needs of modern seismology. Central to its architecture is the seamless integration of seismic waveforms and associated metadata, ensuring the cohesiveness of data and information. The format organizes its content into two principal components: (1) the seismic waveforms, stored as time series data, and (2) the metadata, which incorporates station and event information using standardized formats such as StationXML and QuakeML. This harmonization ensures uniformity in the representation of seismic stations and earthquake event details. Processing histories, provenance details, as well as auxiliary data like adjoint sources and receiver functions, can also be embedded, further enhancing the format's comprehensive nature. A distinguishing trait of ASDF is its optimization for parallel I/O operations, positioning it as a prime choice for high-performance computing environments. The principles, design considerations, and virtues of the ASDF architecture have been expansively documented in the Geophysical Journal International (Krischer et al., 2016) [1](https://chat.openai.com/c/78d37cb0-1768-4534-8a30-dad9fe3d2f0c#user-content-fn-1%5E).

By adopting `ASDF`, we can store data and metadata in formats closely aligned with those widely accepted by the seismology community. It's feasible to encapsulate waveforms, event catalogs, and system information within a single container, enabling standalone usage. Additionally, the format can be tailored to accommodate variations in the information required for triggered and continuous data.

The benefits of using the `ASDF` format become particularly evident when considering the nature of waveform data. More broadly, the main benefit of employing the `ASDF` format to store seismic data and metadata include:

1.  1.  **Integrated Storage and Organization**: ASDF is a comprehensive and well-defined format that seamlessly integrates seismological waveform data and its associated metadata, such as event and station information. This unified approach simplifies concurrent access, data organization, and sharing, addressing many of the limitations of previous fragmented systems. By encapsulating all essential components in a standardized manner, ASDF reduces the dependency on custom, non-reusable scripts and enhances collaboration between research groups with varied internal structures.
    
2.  **Full Provenance Support**: One of the standout features of ASDF is its capability to store the complete provenance graph, capturing the history and evolution of data. This provenance tracking ensures that the origins of the data and the operations performed on it are documented, addressing issues arising from team changes, software updates, or potential processing bugs. The format thereby offers transparency and traceability, features absent or limited in other data formats.
    
3.  **Synthetic Seismogram Storage**: ASDF pioneers in accommodating proper storage and exchange of synthetic seismograms. This includes comprehensive documentation on the numerical solver used, earthquake parameters, the Earth model, and all factors influencing the simulation's outcome. Given the computational intensity of generating high-frequency waveform simulations in realistic Earth models, this preservation and thorough documentation add immense value to the seismological community.
    
4.  **Efficiency in File Management**: ASDF offers a significant reduction in the number of files required for various tasks. A single ASDF file can effectively replace tens to hundreds of thousands of individual waveform files. This consolidation not only results in raw performance and organizational advantages but also addresses challenges like file count quota limits, especially on supercomputing platforms. Furthermore, the format supports efficient parallel I/O on suitable hardware, making it ideal for scalable parallel data processing workflows.
    
5.  **Versatile Data Type Support**: Beyond seismograms, ASDF is adept at accommodating diverse data types prevalent in seismology. Whether it's spectral estimations, cross-correlations, adjoint sources, receiver functions, or other data types, ASDF ensures organized, self-describing storage. This versatility ensures that seismologists can maintain a consistent data storage paradigm across various facets of their research.

This format is particularly suited to meet the requirements of tomorrow.

## Naming Convention and Cross Referencing

To enable effortless cross referecing between the different elements contained in the file adopting an consistent and standard naming convention is important. 

This section is concerned about the cross-referencing of information related to channels allowing the waveform data, catalogue, inventory and other auxiliary data to be consistent and properly associated.


-   **Trace Identifier**: A unique identifier for the trace (see Obspy [documentation](https://docs.obspy.org/master/packages/autogen/obspy.core.event.resourceid.ResourceIdentifier.html) for information on the recommended _resource identifier_ structure). The trace identifier format and convention should be normalized and align with the SEED naming convention including the standard part. Deviation regarding the naming shall, howerver, be permitted to allow longer names to be utilized.  
-   **Location Identification**: The location identification convention described [here](https://ds.iris.edu/ds/newsletter/vol1/no1/1/specification-of-seismograms-the-location-identifier/). This convention has been adapted for μseismic monitoring in the mining context.
    -   **Network Code [network_code]** — Code representing the network.
    -   **Station Code [station_code]** — Code representing the station containing the digitizer.
    -   **Location Code [location_code]** — Code representing the instrument.
    -   **Channel Code [channel_code]** — The three (3) alphanumerical code representing the channel shall follow the FDSN standard naming convention of August 2000 described in the SEED document [Appendix A](http://www.fdsn.org/pdf/SEEDManual_V2.4_Appendix-A.pdf). For instance, a typical 14 Hz or 15 Hz omnidirectional geophone code would be GH?, where ? would be replaced by the appropriate component orientation code.

#### Notes:

**Naming Convention**: The full name of a component or channel is usually created as follows: **channel name** = **network code**.**station code**.**location code**.**channel code**. 

**Network, Station, and Location Codes**: The mentioned convention is often not strictly adhered to by μseismic system providers. Flexibility in applying the convention is crucial. Typically, no distinction is made between the station and location, and each instrument is assigned a unique code that may or may not refer to the data acquisition station. In such cases, we suggest using the **Station Code** field to store the _instrument_ code and populate the location code with 01 or 00. In a network, each combination of **Station Code**.**Location Code**.**Channel Code** should be unique.

#### Suggested Naming Convention and Usage

- **Network Code**: The network code should represent a physical or logical network. A mine or mining complex can include multiple networks. The network name should be compact but be easily recognized and understood. For instance, the Oyu Tolgoi Hugo North Underground Lift 1 network can be represented by the following accronym: OTHNL1.
- **Station Code**: We recommend associating the station code to an instrument or a group of instrument. The Station code should convey relevant information on the sensor location and provide context to the seismic system stakeholders. An example for an adequate station code, given an instrument is installed along the Haulage Level Access Drive 2, would be HLAD2.
- **Location Code**: If the Station Code refers to a group of instruments, for instance, instruments installed in a long borehole, connected to the same acquisition station, the location code can be used to differentiate the instrument within the group. The location code should be kept short. It can simply be a number converying the relative order or in the case of a borehole installation, a measure of the location along the hole.

## Data Format and Adaptation
### Overview

The key is to ensure consistencies between the data and information to allow efficient cross-referencing, retrieval and usage. The ASDF file will serve a s main container. There should be one ASDF file per event or chunk of continuous data. Grid information should not be included in the ASDF file but rather be stored in a separate file

In this section we will cover a series of data types some are to be included as part of the ASDF file some provided as additional files:

- **ASDF Core**
  - [REQUIRED] **Waveforms** 
  - [REQUIRED FOR EVENT DATA, NOT INCLUDED FOR CONTINUOUS DATA] **Catalog**
  - [REQUIRED] **Inventory/System** 
- **ASDF Auxiliary**
  - [REQUIRED] **System Information** &mdash; important system information not included in the StationXML
  - [REQUIRED/READ ONLY] **Lookup Table for Event Type Conversion**  &mdash; as described later in the document we suggest a mapping between the event type prescribed by the QuakeML standardard ant the event types encoutered in mining.
  - [REQUIRED] **Processing Logs** 
  - [OPTIONAL] **Ray and Ray Parameters** &mdash; from ray tracing
  - [OPTIONAL] **Alternative Location** &mdash; from Montecarlo or Jacknife analyses

- **Grids** [NOT INCLUDED, SEPARATE FILE] &mdash; suited for velocity, velocity derivatives (e.g., slowness) attenuation, density, and travel times.

ASDF Data Structure Overview
```matematica
ASDF File
|
└─── Waveforms
└─── QuakeML
└─── StationXML
└─── AuxiliaryData
   └─── SystemInfo
   └─── EventConversionLookup
   └─── Ray
   └─── AlternativeLocations   
```


We made the explicit choice to use the QuakeML and StationXML for **catalog** and **inventory** information, aligning with prevailing standards in the seismic community. The principal advantage of this decision is the assured compatibility with a multitude of existing tools and software in the seismic domain. Furthermore, both QuakeML and StationXML are inherently flexible and adaptable. These formats not only offer comprehensive sets of parameters to detail seismic events and station metadata, but they are also designed to accommodate custom parameters and extensions. This flexibility has been instrumental in our initiative. We've leveraged the extensibility of these formats to incorporate specialized μseismic parameters or attributes, ensuring that QuakeML and StationXML are tailored to address the unique nuances and requirements of microseismic monitoring. This adaptation ensures that the data format remains both relevant to the broader seismic community and aptly suited for microseismic applications.

However, like all choices, leveraging these formats comes with its challenges. While their use ensures widespread compatibility, the interpretation of file content may require nuanced processing and considerations. Some adaptations might be essential to cater specifically to the μseismic monitoring context, possibly involving the addition of auxiliary fields or annotations. The goal, throughout these modifications, remains to balance ease-of-use with the specificity required for μseismic data, ensuring that users can maximize the value derived from the data while minimizing the overhead of adaptations and interpretations.

### ASDF Core

#### Waveform Data

The waveform data represents the raw vibrations recorded directly by the sensors. For convenience, waveform data can be provided in physical units native to the instrument recording the data of $m$, $m/s$​, or $m/s^2$ for displacement, velocity, and acceleration, respectively. However, if size is a concern, storing the ADC counts as integers is more suitable. Storing the ADC count as integers allows for the more efficient use of compression algorithm and will allow the data to be more compact.

Alongside the amplitude values, additional metadata describing the instrument recording the data and time series parameters should be provided for each trace. Each trace should be stored with its own metadata information adopting the convention described in the previous section.

In addition to the trace identification information the following metadata must be present and accurately described the timeserie parameters

- **Sampling Rate [`sampling_rate`]** &mdash; The signal's sampling rate in samples per second.
- **Calibration Factor [`calib`]** &mdash; This value is optional and defaults to 1.0 if not provided. It represents the calibration factor should the sensor deviate from the typical response.
-   **Start Time [`starttime`]** &mdash; Date and time of the first data sample given in UTC

The other parameters can be derived from the sampling rate, start time and the waveform data:

- **Number of Point [`npts`]** &mdash; Number of sample points. Measured by counting the number of points in the time serie.
- **Sampling Interval [`delta`]** &mdash; sample distance in seconds. Computed as the inverse of the sampling_rate (`1/sampling_rate`).
- **End Time [`endtime`]** &mdash; Date and time of the last data sample given in UTC  `endtime = startime + npts * delta`

#### Catalog

The catalog includes information related to an event, a trigger, or a series of events. We suggest storing the catalog information in a QuakeML-like format adapted to μseismic data (see QuakeML [documentation](https://quake.ethz.ch/quakeml)). The suggested changes affect the following QuakeML objects:

-   **Event** — The event_type field is restricted to specific values that are not suited for μseismic monitoring.
-   **Origin** — The position is expressed in latitude and longitude. This will need to be changed to x, y, and z.
-   **Magnitude** — The magnitude object would benefit from adding fields to store the corner frequency, as well as P-wave and S-wave Energy. This allows for a broad range of source parameters to be computed on the fly, rather than stored in the magnitude object. This approach is typically used in mine seismology.

To ensure compatibility we suggest using the QuakeML data type as is and use a lookup table to convert QuakeML to the mining event types. The lookup table is to be stored as a dictionary as auxiliary data. The suggested mapping is described in the table below: 

| Event Type (mining)            | Event Type (QuakeML)        |
|-------------------------------------|-----------------------------|
| earthquake/large event              | earthquake                  |
| seismic event                       | induced or triggered event  |
| offsite event                       | atmospheric event           |
| rock burst                          | rock burst                  |
| fall of ground/rockfall             | cavity collapse             |
| blast                               | explosion                   |
| blast sequence                      | accidental explosion        |
| development blast                   | industrial explosion        |
| production blast                    | mining explosion            |
| far away blast/open pit blast       | quarry blast                |
| offsite blast                       | nuclear explosion           |
| paste firing                        | chemical explosion          |
| calibration blast                   | controlled explosion        |
| other blast/slashing                | experimental explosion      |
| mid-shift blast/slash blast         | industrial explosion        |
| raise bore                          | hydroacoustic event         |
| crusher noise                       | road cut                    |
| orepass noise                       | collapse                    |
| drilling noise                      | acoustic noise              |
| electrical noise                    | thunder                     |
| scaling noise                       | anthropogenic event         |
| mechanical noise                    | crash                       |
| test pulse                          | sonic boom                  |
| unidentified noise/other noise      | other event                 |
| duplicate                           | boat crash                  |
| unknown                             | plane crash                 |
| tap test/test                       | avalanche                   |


The above mapping is implemented in the μQuake library.

#### Origin &mdash; Coordinate System

The QuakeML format was designed to store seismic data at a regional and global scale. Consequently, the QuakeML format has adopted a spherical coordinate system, storing location information in latitude and longitude.

Using latitude and longitude to describe mining-related events is possible but not convenient. It would require precisely knowing the series of transformations needed to convert the position expressed in the local coordinate system into longitude and latitude. We suggest overriding the latitude and longitude information and using x, y, and z instead. The use of x, y, and z notation, as opposed to the right-handed (easting, northing, and elevation) or (northing, easting, down) triplets, is to provide flexible notation and ensure consistency between the mine and the seismic system's coordinate systems.

The μQuake implementation exploits the QuakeML extra parameters to store the x, y, and z values and associated errors.

#### Magnitude &mdash; Corner Frequency, and Energy

The QuakeML standard does not include objects suited to store the corner frequency. The energy could be stored in the amplitude or station_magnitude object. However, this isn't convenient, as it's preferable for the magnitude and energy information to be used in tandem.

The μQuake implementation stores the `corner_frequency`, the `energy_p`, `energy_s`, and associated error alongside the magnitude information in the extra parameter of the Magnitude object.

### System or Inventory Information

The system or inventory information is crucial for any seismic network as it provides comprehensive details about the stations and channels that are a part of that network. The StationXML format, which is an established standard in the seismic community, is designed to hold such inventory metadata.

However, when tailoring the use of StationXML to the μseismic domain, especially in the mining context, certain modifications are required to better fit the data's unique characteristics and requirements.

#### Station & Channel &mdash; Coordinates

In a typical seismic application, the StationXML format uses latitude and longitude to describe the geographical position of stations and channels. While this makes sense for global and regional seismic networks, it's not as intuitive or convenient for the μseismic monitoring within a mining setup. In such a scenario, the geographical location is often best described using a local coordinate system.

Thus, we recommend replacing the latitude and longitude fields in the StationXML format with `x`, `y`, and `z` coordinates for both the station and channel locations. This change aligns with the earlier modifications made for QuakeML, ensuring consistency across different components of the μseismic system.

## System Information Storage in ASDF's Auxiliary Data

### Background

To ensure accurate interpretation and utilization of seismic data, it's essential to provide comprehensive system information. This information offers the necessary context about the recording environment, location, and equipment specifics, which can be pivotal in analysis phases.

### Proposed Data Structure

#### Directory Structure:

-   Within the `AuxiliaryData` section of the ASDF file, we introduce a dedicated `SystemInfo` directory.
-   This directory will consolidate all relevant information about the seismic system setup.

#### System Attributes:

The `SystemInfo` directory will house the following attributes:

-   `country`: The nation where the system is located.
-   `time_zone`: Local time zone of the system or the offset from Coordinated Universal Time (UTC). The time zone name must be found with the [IANA time zone database](https://www.iana.org/time-zones)
-   `site_name`: Name or identifier of the seismic site.
-   `latitude`: Geographic latitude of the system's location.
-   `longitude`: Geographic longitude of the system's location.
-   `depth`: Altitude of the system.
-   `installation_date`: Date when the system was set up.
-   `removal_date`: Date when the system was removed (if applicable).
-   `monitoring_target`: The monitoring target (e.g. underground, open-pit, surface)
-   `p_velocity`: The P-wave velocity (relevant when homogeneous velocity model is used)
-   `s_velocity`: The S-wave velocity (relevant when homogeneous velocity model is used)
-   `density`: Rock mass density (usually set to 2700 $kg/m^3$)
-   `p_quality`: P quality factor
-   `s_quality`: S quality factor
-   `system_vendor`: Seismic System Vendor
-   `Description`: Additional remarks or details about the site or instrumentation.

### Implementation Notes:

### Data Structure Inside ASDF for SystemInfo

Inside `SystemInfo`, each attribute (like `country`, `time_zone`, `latitude`, etc.) are a dataset. Datasets in HDF5 (and thus ASDF) are essentially multidimensional arrays of data elements, accompanied by metadata. For many of our attributes, these datasets would just be single values (like a single string for `country`), but the structure allows for more complex data if needed.

For example:

```mathematica
`AuxiliaryData
│
└───SystemInfo
    │   Country = "USA"
    │   time_zone = "PST"
    │   latitude = 34.0522
    │   longitude = -118.2437
    │   ...` 
```

### Code Examples

To interact with ASDF, we can use Python with the `pyasdf` library. Here's a hypothetical code snippet to add `SystemInfo` to an ASDF file:

```python
import pyasdf

# Load an existing ASDF dataset or create a new one
with pyasdf.ASDFDataSet("path_to_asdf_file.h5", mode="a") as ds:

    # Create a new auxiliary data group for SystemInfo
    system_info = ds.create_auxiliary_data_group("SystemInfo")

    # Add data to this group
    system_info.create_dataset(name="country", data="USA")
    system_info.create_dataset(name="time_zone", data="PST")
    system_info.create_dataset(name="latitude", data=34.0522)
    system_info.create_dataset(name="longitude", data=-118.2437)
    # ... similarly for other attributes

# Check the data
with pyasdf.ASDFDataSet("path_to_asdf_file.asdf") as ds:
    print(ds.auxiliary_data.SystemInfo.Country[:])  # Should print 'USA'` 
```

For reading the `SystemInfo`:
```python
import pyasdf

with pyasdf.ASDFDataSet("path_to_asdf_file.asdf") as ds:
    country = ds.auxiliary_data.SystemInfo.country[:]
    timezone = ds.auxiliary_data.SystemInfo.TimeZone[:]
    latitude = ds.auxiliary_data.SystemInfo.Latitude[:]
    longitude = ds.auxiliary_data.SystemInfo.Longitude[:]
    # ... and so on` 
```

It's important to note that this example assumes the `pyasdf` library has (or will have) methods named as described. In practice, depending on the actual `pyasdf` version and methods available, there may be variations in how you interact with the file. Always refer to the documentation of the library for accurate and up-to-date information.


### System Metadata and Grid Data

To fully enable the interpretation of the seismic data, accessing information on the velocity models is essential. Other important although not as essential are the attenuation model if they exist and the density values or model. The velocity values/models are increasingly represented on a grid. 

In this section, we propose using a simple grid format for data exchange.  


#### Grid Format Overview

The grid format is structured to represent the velocity model in a systematic, gridded layout. It comprises two main components: the header, which contains metadata about the grid, and the grid values that reflect the actual data.

##### Header

The header is a crucial section that offers insights into the grid's specifications, such as its dimensions, the starting point (origin) of the grid, and the intervals or spacings between grid points. A sample header in Python dictionary format might look like:

```python
header = {
    "NX": 100,            # Number of grid points in the X direction
    "NY": 100,            # Number of grid points in the Y direction
    "NZ": 50,             # Number of grid points in the Z direction
    "ORIGX": 0.0,         # X coordinate of the grid origin
    "ORIGY": 0.0,         # Y coordinate of the grid origin
    "ORIGZ": 0.0,         # Z coordinate of the grid origin
    "SPACEX": 0.1,        # Grid spacing in the X direction
    "SPACEY": 0.1,        # Grid spacing in the Y direction
    "SPACEZ": 0.2,        # Grid spacing in the Z direction
}
```

This dictionary effectively captures the grid's spatial boundaries, intervals, and the type of data contained.

##### Grid Data

The grid data values follow the header. Organized in a sequence, they represent their positions within the grid. For simplicity and efficiency, these values can be represented as a 2D numpy array:

```python
import numpy as np

# For demonstration purposes, initializing a 2D array with NXxNYxNZ dimensions
grid_values = np.zeros((header["NX"], header["NY"], header["NZ"]))

```

In this representation, the grid values are stored in a structured manner, allowing for easy indexing and operations. Each cell in the array corresponds to a grid point in the velocity model, with its value representing the velocity (or density or attenuation, depending on the grid type) at that point.

### Data Format for Storing Ray Information in ASDF

#### Background

For efficient microseismic monitoring and analysis, the inclusion of ray tracing data alongside waveform and inventory data is beneficial. In keeping with the extensibility and flexibility features of the ASDF format, we propose a systematic structure to store the ray information.

#### Proposed Data Structure

##### Directory Structure:

-   Within the `AuxiliaryData` section of the ASDF file, we introduce a dedicated `Rays` directory.
-   Individual rays are uniquely identified by a unique `resource_id` and each has its own sub-directory under `Rays` and adopt a naming convention linking them to the an instrument using the `network`, `station` and `channel` codes.

##### Ray Attributes:

Each ray's sub-directory will include the following attributes:

-   `network_code`: Identifier for the network. Standard naming conventions of seismic networks are to be employed.
-   `station_code`: Identifier for the station. This aligns with the standard naming conventions employed for seismic stations.
-   `location_code`: A code that uniquely identifies different data streams at a single station. It allows users to distinguish between data from closely spaced instruments or multiple data collection strategies.
-   `arrival_id`: The unique `ResourceIdentifier` of the associated seismic arrival included in the QuakeML section of the ASDF file.
-   `phase`: The seismic phase ("P" or "S").
-   `azimuth`: Azimuth in degrees.
-   `takeoff_angle`: Takeoff angle in degrees.
-   `travel_time`: Travel time between the seismic source and the recording site in seconds.
-   `earth_model_id`: A `ResourceIdentifier` representing the velocity model used.
-   `length`: Computed length of the ray representing the distance along the raypath between an event and a sensor
-   `baz`: Computed back azimuth of the ray.
-   `incidence_angle`: Computed incidence angle of the ray.

##### Ray Datasets:

-   A dataset named `nodes` in each ray's sub-directory, storing the array of nodes that describe the ray path.

#### Implementation Notes:

1.  **Creating Rays in ASDF**:
    
    -   Users are encouraged to use existing ASDF software tools to initiate and modify the ASDF file structure.
    -   During ray storage, a serialization function or method will be required to transform the `Ray` object into the above-proposed ASDF-compatible structure.
    
2.  **Retrieving Rays from ASDF**:
    -   A corresponding deserialization function or method will be essential to read rays from the ASDF format, converting them back into `Ray` objects.
   
3.  **Compatibility and Integration**:
    -   The proposed structure ensures seamless integration with waveform and inventory data. The use of `network_code`, `station_code`, and `location_code` ensures that rays can be directly associated with specific waveform data and inventory components.

#### Point Cloud Data
#### Lookup Table for Event Type Conversion

## Implementation using the &mu;Quake Python Library

Within the context of this RFC, which proposes specific adaptations and formats tailored to microseismic monitoring, the `uquake` library stands as a practical implementation of these recommendations. Developed in Python and built upon ObsPy, `uquake` has been specially crafted to cater to the distinctive needs of microseismic monitoring in mining environments. This includes, amongst other capabilities, the handling of local coordinate systems and unique event types frequently observed in mine microseismic monitoring. For those seeking an immediate and tangible application of the proposed data format recommendations from this RFC, `uquake` offers a ready solution. It is open source, distributed under AGPL license and publicly available and can be easily accessed and installed via pip using `pip install uquake`.

Note that there are alternative libraries addressing the same issues &mu;Quake tackles. The author is, howerver, less familiar with the alternatives. Notable alternatives are

The Niosh [Obsplus](https://github.com/niosh-mining/obsplus)
[DASCORE](https://github.com/DASDAE/dascore)

### Implementation using `uquake`

The `uquake` library provides the necessary tools to handle gridded data in seismology. One of the primary objects to manage such grid data is the `Grid` class from `uquake.core.grid.base`. This class is designed to hold regular grids and can be employed for both 2D and 3D data structures.

#### Initializing a Grid

The `Grid` object can be initialized using either a numpy array representing the grid data or by specifying the grid dimensions. When using the latter approach, the grid is initialized with a default or specified value.

Let's consider a simple example to illustrate this:
```python
from uquake.core.grid.base import Grid
import numpy as np

# Define grid header specifications

header = {
    "NX": 100,  
    "NY": 100,  
    "NZ": 50,   
    "ORIGX": 0.0,  
    "ORIGY": 0.0,  
    "ORIGZ": 0.0,  
    "SPACEX": 0.1,  
    "SPACEY": 0.1,  
    "SPACEZ": 0.2,  
}

# Initialize the grid using grid dimensions and set default value as 0

grid_dims = (header["NX"], header["NY"], header["NZ"])
spacing = (header["SPACEX"], header["SPACEY"], header["SPACEZ"])
origin = (header["ORIGX"], header["ORIGY"], header["ORIGZ"])

grid = Grid(data_or_dims=grid_dims, spacing=spacing, origin=origin, value=0)

# Write the modified or new grid to a file
grid.write("path_to_new_grid_file.grid")
```
Here, we've initialized a 3D grid using the dimensions specified in the header. The grid is filled with a default value of 0.

#### Reading and Writing Grids

The `Grid` object is equipped with methods to read and write grid data, making it easy to load existing grids or store newly created or modified grids.

Here's an example of how one might read from and write to a grid file:

```python
# all versions
from uquake.core.grid import read_grid

# Reading a grid from a file
grid_from_file = Grid.read("path_to_grid_file.grid")
```

```python
# from version 2.0.1
# Reading a grid from a file
grid_from_file = Grid.read("path_to_grid_file.grid") 
```

This approach simplifies the process of handling gridded data in seismology by using the `uquake` library's built-in functionalities.

## Rationale

Understanding the motivations and reasons behind any proposed change is essential to ensure clarity, gain stakeholder buy-in, and ensure the technical merits of the proposal align with the broader goals.

1. **Choice of Data Packaging Formats**: The decision to employ ASDF for the encapsulation of both catalog information and waveform data, combined with the inherent XML structure for QuakeML, is rooted in their respective proficiencies and widespread acceptance within the industry. ASDF provides an efficient mechanism for storing and fetching data, especially when dealing with substantial datasets. Concurrently, the integral XML structure guarantees interoperability with systems customarily developed for QuakeML processing.

2.  **Adapting QuakeML for μseismic Data**: Traditional QuakeML formats, while robust for standard seismology needs, are not fully suited to address the nuances of μseismic data. The unique requirements of μseismic monitoring, especially in mining contexts, necessitate modifications to the existing format. By making these adjustments, the system becomes more flexible and directly applicable to the μseismic domain, allowing for better data representation and interpretation.
    
3.  **Use of a Grid for Velocity Models**: Seismic data interpretation largely hinges on the accuracy and clarity of velocity models. The industry trend has shifted towards grid representations of these models due to their higher precision and ease of use. Implementing a standard grid format for data exchange promotes interoperability and standardization across systems and tools.
    
4.  **Introduction of the `uquake` Library**: The seismic domain has various software libraries that cater to specific needs. The choice of `uquake` was driven by its capabilities to manage the proposed data structures efficiently and its extensibility to cater to future modifications. By embedding support for the newly proposed data structures directly within `uquake`, users can expect a seamless experience and avoid the complexities associated with multi-tool workflows.
    
5.  **Latitude and Longitude to X, Y, Z Conversion**: Given that most mining operations work on local coordinate systems rather than global ones, expressing positions in terms of x, y, and z becomes not just convenient, but also essential. This shift eliminates the need for complex transformations and ensures data is immediately useful for local operations and analysis.

In summary, the proposed changes and implementations in this RFC stem from a direct need to address the unique challenges posed by μseismic monitoring, especially in mining contexts. They reflect both the evolution of seismic data interpretation methodologies and the tools used in the industry. Through these proposals, we aim to establish a more streamlined, precise, and standardized approach to μseismic data management and interpretation.

### Feedback Mechanism

Feedback and contributions from the community are essential to refining and improving this RFC. There are two primary ways through which stakeholders, developers, and interested parties can provide their feedback:

1.  **GitHub Issues**:
    
    -   Navigate to the [RFC micro-seismic data exchange format repository on GitHub](https://github.com/Microquake-ai/RFC-micro-seismic-data-exchange-format/tree/main).
    -   Go to the `Issues` tab.
    -   Click on `New Issue` to create a new issue.
    -   Provide a concise title and detailed description of your feedback, suggestions, or concerns.
    -   Once submitted, the issue will be visible to the community, and the project maintainers will review and address it as appropriate.
 
2.  **Email**:
    
    -   If you prefer a more direct approach or have feedback that you'd like to keep private, you can send an email to [rfc_format@microquake.ai](mailto:rfc_format@microquake.ai).
    -   Please provide a clear subject line relevant to your feedback to ensure swift handling of your email.
    -   While we appreciate all feedback, do note that due to the volume of emails, it might take some time before you receive a response.

Whether you choose to leave an issue on GitHub or send an email, your feedback is invaluable. It aids in ensuring that the proposed micro-seismic data exchange format is robust, relevant, and addresses the needs of the community.

Thank you for taking the time to review this RFC and for your contributions towards its continual improvement.

## References

Krischer, L., Smith, J. A., Lei, W., Lefebvre, M., Ruan, Y., & Tromp, J. (2016). An Adaptable Seismic Data Format. Geophysical Journal International, 207(2), 1003–1011. [Link](https://academic.oup.com/gji/article/207/2/1003/2583765)









<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMTkxMDgxNjcsMTIwNjg3MzYzNywxMz
k0NDY5NjY2LDEwNzQwMDkzNzgsMjAyMzI0OTE4OCwtMTIzNTAy
Mjc5MywtNjU3MTY5NDc2LC0xNDUwNzc2NzQzLDc1NjU5MTkwOS
w2MjE2MTY0MDEsMTgxMDY2ODUzNiw3MTk2MzMwOTUsMTMzOTM1
MDEzLC0yMTQ1NDg1NDIxLC0xODczNjQyODI0LC04MDM0MTc0OD
QsLTExMjQ1OTk5MzksMTQyOTE5MjkyNiwyOTE2ODkxMzYsMTk5
NDQ5NTYzMl19
-->